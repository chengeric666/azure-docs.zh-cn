## <a name="typical-output"></a>典型输出
下面通过 Hello World 示例来说明写入到日志文件的输出。 添加了换行符和制表符来增强可读性：

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>代码段
此部分讨论 Hello World 示例中的部分重要代码。

### <a name="gateway-creation"></a>创建网关
开发人员必须编写 *网关进程*。 此程序创建内部基础结构（中转站）、加载模块，以及进行正常运行所需的所有设置。 该 SDK 提供 **Gateway_Create_From_JSON** 函数，允许用户从 JSON 文件启动网关。 若要使用 **Gateway_Create_From_JSON** 函数，必须将 JSON 文件的路径传递给它，以便指定要加载的模块。 

可以在 Hello World 示例的 [main.c][lnk-main-c] 文件中找到网关进程的代码。 为了增强可读性，下面的代码段显示的是简化版网关进程代码。 该程序创建一个网关，在卸除该网关之前，会等待用户按 **Enter** 键。 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
} 
```

JSON 设置文件包含要加载的模块的列表以及模块之间的链接。
每个模块必须指定：

* **name**：模块的唯一名称。
* **loader**：一个知道如何加载所需模块的加载程序。  加载程序是一个扩展点，用于加载不同类型的模块。 我们提供了用于以原生 C、Node.js、Java 和 .NET 编写的模块的加载程序。 Hello World 示例仅使用了“原生”加载程序，因为此示例中的所有模块都是以 C 编写的动态库。有关使用以不同语言编写的模块的详细信息，请参阅 [Node.js](https://github.com/Azure/azure-iot-gateway-sdk/blob/develop/samples/nodejs_simple_sample/)、[Java](https://github.com/Azure/azure-iot-gateway-sdk/tree/develop/samples/java_sample) 或 [.NET](https://github.com/Azure/azure-iot-gateway-sdk/tree/develop/samples/dotnet_binding_sample) 示例。
    * **name**：用来加载模块的加载程序的名称。  
    * **entrypoint**：包含模块的库的路径。 在 Linux 上，这是一个 .so 文件，而在 Windows 上，这是一个 .dll 文件。 请注意，此入口点特定于所使用的加载程序的类型。 例如，Node.js 加载程序的入口点是一个 .js 文件，Java 加载程序的入口点是一个类路径 + 类名称，.NET 加载程序的入口点是一个程序集名称 + 类名称。

* **args**：模块所需的任何配置信息。

以下代码显示了 Linux 上用来声明 Hello World 示例的所有模块的 JSON。 模块是否需要参数取决于模块的设计。 在此示例中，logger 模块使用的参数是输出文件的路径，而 Hello World 模块则不使用任何参数。

```
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

JSON 文件还包含要传递到中转站的模块之间的链接。 链接具有两个属性：

* **源**：来自 `modules` 部分的模块名称，或“\*”。
* **接收器**：来自 `modules` 部分的模块名称。

每个链接都会定义消息路由和方向。 来自模块 `source` 的消息会传递到模块 `sink`。 `source` 可能会设置为“\*”，指示来自任何模块的消息都会由 `sink` 接收。

以下代码显示了 Linux 上用来配置 Hello World 示例中所用模块之间的链接的 JSON。 模块 `hello_world` 生成的每条消息都会被模块 `logger` 使用。

```
"links": 
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="hello-world-module-message-publishing"></a>Hello World 模块消息发布
可以在[“hello_world.c”][lnk-helloworld-c]文件中找到“hello world”模块发布消息时使用的代码。 以下代码段显示的是修改版，添加了更多的注释，并删除了部分处理错误的代码以增强可读性：

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Hello World 模块消息处理
Hello World 模块不需处理其他模块发布到中转站的任何消息。 因此，在 Hello World 模块中实施消息回调时，使用的是不执行任何操作的函数。

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>记录器模块消息发布和处理
Logger 模块接收来自中转站的消息，并将其写入文件中。 它不发布任何消息。 因此，Logger 模块的代码不会调用 **Broker_Publish** 函数。

[logger.c][lnk-logger-c] 文件中的 **Logger_Recieve** 函数是中转站发起的回调，用于将消息传递给 Logger 模块。 以下代码段显示的是修改版，添加了更多的注释，并删除了部分处理错误的代码以增强可读性：

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>后续步骤
若要了解如何使用 IoT 网关 SDK，请参阅：

* [IoT 网关 SDK - 使用 Linux 通过模拟设备发送设备到云消息][lnk-gateway-simulated]。
* GitHub 上的 [Azure IoT 网关 SDK][lnk-gateway-sdk]。

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!--HONumber=Dec16_HO1-->


