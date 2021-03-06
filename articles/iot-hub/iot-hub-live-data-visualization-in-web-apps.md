---
title: "Azure IoT 中心的实时传感器数据可视化 – Web 应用 | Microsoft Docs"
description: "使用 Azure Web 应用可视化从传感器收集的温度和湿度数据并将其发送到 Azure IoT 中心。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "实时数据可视化, 即时数据可视化, 传感器数据可视化"
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 788b3a6ce98c6668a8efc41f66c7e39b8aeda2db
ms.lasthandoff: 03/30/2017


---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-azure-web-apps"></a>使用 Azure Web 应用可视化 Azure IoT 中心的实时传感器数据

![传感器、IoT 设备、IoT 中心与 Azure Web 应用之间的连接](media/iot-hub-live-data-visualization-in-web-apps/1_sensor-iot-device-azure-iot-hub-web-app-connection.png)

> [!NOTE]
> 在开始本教程之前，请确保已完成[将 ESP8266 连接到 Azure IoT 中心](/iot-hub-arduino-huzzah-esp8266-get-started.md)。 在[将 ESP8266 连接到 Azure IoT 中心](/iot-hub-arduino-huzzah-esp8266-get-started.md)教程中，你已设置 IoT 设备和 IoT 中心，并部署了一个在设备上运行的示例应用程序。 该应用程序会将收集的传感器数据发送到 IoT 中心。

## <a name="what-you-learn"></a>学习内容

本课程讲解如何通过运行一个托管在 Azure Web 应用上的 Web 应用程序来可视化 Azure IoT 中心接收的实时传感器数据。 如果你想要尝试使用 Power BI 可视化 IoT 中心的数据，请参阅[使用 Power BI 可视化 Azure IoT 中心的实时传感器数据](iot-hub-live-data-visualization-in-power-bi.md)。

## <a name="what-you-do"></a>准备工作

- 在 Azure 门户中创建一个 Azure Web 应用。
- 添加一个使用者组，让 IoT 中心做好数据访问准备。
- 将 Web 应用配置为从 IoT 中心读取传感器数据。
- 上载要在 Web 应用中托管的 Web 应用程序。
- 打开该 Web 应用，查看 IoT 中心的实时温度和湿度数据。

## <a name="what-you-need"></a>所需条件

- 满足已完成的教程[将 ESP8266 连接到 Azure IoT 中心](iot-hub-arduino-huzzah-esp8266-get-started.md)所述的以下要求：
  - 一个有效的 Azure 订阅。
  - 已订阅中创建一个 Azure IoT 中心。
  - 一个可向 Azure IoT 中心发送消息的客户端应用程序。
- Git。 （[下载 Git](https://www.git-scm.com/downloads)）。

## <a name="create-an-azure-web-app"></a>创建 Azure Web 应用

1. 在 [Azure 门户](https://ms.portal.azure.com/)中，单击“新建” > “Web + 移动” > “Web 应用”。
1. 输入唯一的作业名称，验证订阅，指定资源组和位置，选择“固定到仪表板”，然后单击“创建”。

   建议选择资源组所在的同一位置。 这有助于加快处理速度，降低数据传输费用。

   ![创建 Azure Web 应用](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

## <a name="add-a-consumer-group-to-your-iot-hub"></a>将使用者组添加到 IoT 中心

应用程序使用使用者组从 Azure IoT 中心提取数据。 在本课程中，你将创建一个使用者组，供 Web 应用程序用来从 IoT 中心读取数据。

若要将使用者组添加到 IoT 中心，请执行以下步骤：

1. 在 [Azure 门户](https://ms.portal.azure.com/)中打开 IoT 中心。
1. 在左窗格中单击“终结点”，在中间窗格中选择“事件”，在右窗格的“使用者组”下面输入名称，然后单击“保存”。

   ![在 Azure IoT 中心内创建使用者组](media/iot-hub-live-data-visualization-in-web-apps/3_add-consumer-group-iot-hub-azure.png)

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>将 Web 应用配置为从 IoT 中心读取数据

1. 打开刚刚预配的 Web 应用。
1. 单击“应用程序设置”，然后在“应用设置”下面添加以下键/值对：

   | 键                                   | 值                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | 从 IoT 中心资源管理器获取                               |
   | Azure.IoT.IoTHub.DeviceId             | 从 IoT 中心资源管理器获取                               |
   | Azure.IoT.IoTHub.ConsumerGroup        | 添加到 IoT 中心的使用者组的名称  |

   ![将包含键值对的设置添加到 Azure Web 应用](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>上载要在 Web 应用中托管的 Web 应用程序

我们在 GitHub 中提供了一个 Web 应用程序，它可以显示 IoT 中心的实时传感器数据。 你只需将 Web 应用配置为使用 Git 存储库，从 GitHub 下载该 Web 应用程序，然后将它上载到 Azure，以便在 Web 应用中托管。

1. 在 Web 应用中，单击“部署选项” > “选择源” > “本地 Git 存储库”。

   ![将 Azure Web 应用部署配置为使用本地 git 存储库](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

1. 单击“设置连接”，创建用于连接到 Azure 中 Git 存储库的用户名和密码，然后单击“确定”。

   ![为 Web 应用设置 Azure 中 git 存储库的用户名和密码](media/iot-hub-live-data-visualization-in-web-apps/6_web-app-set-user-password-git-repo-azure.png)

1. 单击“确定”完成配置。
1. 单击“概述”并记下“Git 克隆 URL”的值。

   ![获取 Azure Web 应用的 git 克隆 URL](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

1. 在本地计算机上打开命令提示符或终端窗口。
1. 从 GitHub 下载 Web 应用程序并将它上载到 Azure，以便在 Web 应用中托管。 为此，请运行以下命令：

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!Note]
   > “Git 克隆 URL”是 Web 应用的“概述”页中显示的 Git 存储库的 URL。\<\>

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>打开该 Web 应用，查看 IoT 中心的实时温度和湿度数据

在 Web 应用的“概述”页上，单击 URL 打开 Web 应用。

![获取 Azure Web 应用的 URL](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

此时应会看到 IoT 中心的实时温度和湿度数据。

![显示实时温度和湿度的 Azure Web 应用页](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

## <a name="next-steps"></a>后续步骤
现已成功使用 Azure Web 应用可视化 Azure IoT 中心的实时传感器数据。

还可以通过另一种方法可视化 Azure IoT 中心的数据。 请参阅[使用 Power BI 可视化 Azure IoT 中心的实时传感器数据](iot-hub-live-data-visualization-in-power-bi.md)。

若要继续了解 IoT 中心入门知识并浏览其他 IoT 方案，请参阅：

- [使用 iothub-explorer 管理云设备消息传送](iot-hub-explorer-cloud-device-messaging.md)
- [将 IoT 中心消息保存到 Azure 数据存储](iot-hub-store-data-in-azure-table-storage.md)

