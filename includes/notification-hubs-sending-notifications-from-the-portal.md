

通常，推送通知是在后端服务（例如，Mobile Apps，或者使用兼容库的 ASP.NET）中发送的。 如果你的后端没有可用的库，则你也可以使用 REST API 直接发送通知消息。 

下面是你可能想要查看的有关发送通知的其他教程列表：

* Azure Mobile Apps：有关如何从通知中心集成的 Mobile Apps 后端发送通知的示例，请参阅 [Add Push Notifications to your iOS App](../articles/app-service-mobile/app-service-mobile-ios-get-started-push.md)（将推送通知添加到 iOS 应用）。  
* ASP.NET： [使用通知中心将通知推送到用户](../articles/notification-hubs/notification-hubs-aspnet-backend-ios-apple-apns-notification.md)。
* Azure 通知中心 Java SDK：有关从 Java 发送通知的信息，请参阅 [如何通过 Java 使用通知中心](../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md) 。 这种方法已在 Eclipse for Android 开发环境中进行测试。
* PHP： [如何通过 PHP 使用通知中心](../articles/notification-hubs/notification-hubs-php-push-notification-tutorial.md)。

在本教程的下一部分中，你将了解如何使用 [通知中心 REST 接口](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) 直接在应用程序中发送通知消息。 所有已注册的设备将接收任何设备发送的通知。  



<!--HONumber=Jan17_HO1-->


