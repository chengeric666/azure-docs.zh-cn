下表列出了特定于服务总线消息的配额信息。 有关服务总线的定价及其他配额的信息，请参阅[服务总线定价](https://azure.microsoft.com/pricing/details/service-bus/)概述。

| 配额名称 | 范围 | 类型 | 超出时的行为 | 值 |
| --- | --- | --- | --- | --- |
| 每个 Azure 订阅的基本/标准命名空间数上限 |命名空间 |静态 |后续请求更多基本/标准命名空间将被门户拒绝。 |100|
| 每个 Azure 订阅的高级命名空间数上限 |命名空间 |静态 |后续请求更多高级命名空间将被门户拒绝。 |10 |
| 队列/主题大小 |实体 |创建队列/主题时定义。 |系统将拒绝传入消息，且调用代码将收到异常。 |1、2、3、4 或 5 GB。<br /><br />如果已启用[分区](../articles/service-bus-messaging/service-bus-partitioning.md)，最大队列/主题大小是 80 GB。 |
| 命名空间上的并发连接数 |命名空间 |静态 |系统将拒绝后续的附加连接请求，且调用代码将收到异常。 REST 操作不计入并发 TCP 连接数。 |NetMessaging：1,000<br /><br />AMQP：5,000 |
| 队列/主题/订阅实体上的并发连接数 |实体 |静态 |系统将拒绝后续的附加连接请求，且调用代码将收到异常。 REST 操作不计入并发 TCP 连接数。 |受每个命名空间的并发连接限制的约束。 |
| 队列/主题/订阅实体上的并发接收请求数 |实体 |静态 |系统将拒绝后续的接收请求，且调用代码将收到异常。 此配额适用于一个主题上所有订阅的并发接收操作总数。 |5,000 |
| 每个服务命名空间的主题/队列数 |系统范围 |静态 |将拒绝后续的在服务命名空间中创建新主题或队列的请求。 因此，如果是通过 [Azure 门户][Azure portal]配置的，将生成错误消息。 如果是通过管理 API 调用的，调用代码将收到异常。 |10,000<br /><br />服务命名空间中主题和队列的数目之和必须小于或等于 10,000。<br/>这不适用于高级层，因为其中的所有实体已分区。 |
| 每个服务命名空间的分区主题/队列数 |系统范围 |静态 |将拒绝后续的在服务命名空间中创建新分区主题或队列的请求。 因此，如果是通过 [Azure 门户][Azure portal]配置的，将生成错误消息。 如果是通过管理 API 调用的，调用代码将收到 **QuotaExceededException** 异常。 |基本和标准层 - 100<br />高级层 - 1,000<br/><br />在每个命名空间中，每个分区队列或主题的实体配额不会超过 10,000 个。 |
| 任一消息实体路径的最大大小：队列或主题 |实体 |静态 |- |260 个字符 |
| 任一消息实体名称的最大大小：命名空间、订阅或订阅规则 |实体 |静态 |- |50 个字符 |
| 队列/主题/订阅实体的消息大小 |系统范围 |静态 |将拒绝超过这些配额的传入消息，且调用代码会收到异常。 |最大消息大小：256KB（[标准层](../articles/service-bus-messaging/service-bus-premium-messaging.md)）/1MB（[高级层](../articles/service-bus-messaging/service-bus-premium-messaging.md)）。 <br /><br />**注意**由于系统开销问题，此限制通常略小一点。<br /><br />最大标头大小：64KB<br /><br />属性包中的最大标头属性数：**byte/int.MaxValue**<br /><br />属性包中属性的最大大小：没有明确的限制。 受最大标头大小限制。 |
| 队列/主题/订阅实体的消息属性大小 |系统范围 |静态 |将生成 **SerializationException** 异常。 |每个属性的最大消息属性大小为 32K。 所有属性的累计大小不得超过 64K。 这一点适用于整个 [BrokeredMessage](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.aspx) 标头，其中既有用户属性，又有系统属性（如 [SequenceNumber](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.sequencenumber.aspx)、[Label](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.label.aspx)、[MessageId](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) 等）。 |
| 每个主题的订阅数 |系统范围 |静态 |将拒绝后续的为主题创建附加订阅的请求。 因此，如果是通过门户配置的，将显示错误消息。 如果是通过管理 API 调用的，调用代码将收到异常。 |2,000 |
| 每个主题的 SQL 筛选器数 |系统范围 |静态 |将拒绝任何后续的在主题中创建附加筛选器的请求，且调用代码会收到异常。 |2,000 |
| 每个主题的相关性筛选器数 |系统范围 |静态 |将拒绝任何后续的在主题中创建附加筛选器的请求，且调用代码会收到异常。 |100,000 |
| SQL 筛选器/操作的大小 |系统范围 |静态 |将拒绝后续的创建附加筛选器请求，且调用代码将收到异常。 |筛选器条件字符串的最大长度：1024 (1K)。<br /><br />规则操作字符串的最大长度：1024 (1K)。<br /><br />每个规则操作的最大表达式数：32。 |
| 每个命名空间、队列或主题的 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) 规则数 |实体、命名空间 |静态 |将拒绝后续的创建附加规则请求，且调用代码将收到异常。 |最大规则数：12。 <br /><br /> 在服务总线命名空间中配置的规则适用于该命名空间中的所有队列和主题。 |

[Azure portal]: https://portal.azure.com

<!--HONumber=Dec16_HO1-->


