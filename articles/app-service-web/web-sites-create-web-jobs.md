---
title: "使用 Web 作业运行后台任务"
description: "了解如何在 Azure Web 应用中运行后台任务。"
services: app-service
documentationcenter: 
author: tdykstra
manager: erikre
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 5d0d46447c3e0a3a1047e2bbedd44bbd46dd7f1b
ms.lasthandoff: 03/01/2017


---
# <a name="run-background-tasks-with-webjobs"></a>使用 Web 作业运行后台任务
## <a name="overview"></a>概述
可使用&3; 种方式在 [Azure 应用服务](http://go.microsoft.com/fwlink/?LinkId=529714) Web 应用的 WebJobs 中运行程序或脚本：按需、连续或按计划。 使用 Web 作业无需支付额外的费用。

本文说明如何使用 [Azure 门户](https://portal.azure.com)部署 WebJobs。 有关如何使用 Visual Studio 或连续交付过程进行部署的信息，请参阅[如何将 Azure WebJobs 部署到 Web 应用](websites-dotnet-deploy-webjobs.md)。

Azure WebJobs SDK 简化了许多 Web 作业编程任务。 有关详细信息，请参阅[什么是 WebJobs SDK](websites-dotnet-webjobs-sdk.md)。

 Azure Functions 提供了另一种从无服务器环境或从应用服务应用运行程序和脚本的方法。 有关详细信息，请参阅 [Azure Functions 概述](../azure-functions/functions-overview.md)。

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="acceptablefiles"></a>可接受的脚本或程序文件类型
接受以下文件类型：

* .cmd、.bat、.exe（使用 Windows cmd）
* .ps1（使用 Powershell）
* .sh（使用 Bash）
* .php（使用 php）
* .py（使用 Python）
* .js（使用 Node）
* .jar（使用 java）

## <a name="CreateOnDemand"></a>在门户中创建按需 Web 作业
1. 在 [Azure 门户](https://portal.azure.com)的“Web 应用”边栏选项卡中，依次单击“所有设置”>“WebJobs”以显示“WebJobs”边栏选项卡。
   
    ![Web 作业边栏选项卡](./media/web-sites-create-web-jobs/wjblade.png)
2. 单击 **“添加”**。 “添加 WebJob”对话框显示。
   
    ![添加 Web 作业边栏选项卡](./media/web-sites-create-web-jobs/addwjblade.png)
3. 在“名称”下，提供 WebJob 的名称。 名称必须以字母或数字开头，并且不能包含除“-”和“_”以外的任何特殊字符。
4. 在“运行方式”框中，选择“按需运行”。
5. 在“文件上传”框中，单击文件夹图标，然后浏览到包含脚本的 zip 文件。 Zip 文件应包含可执行文件 (.exe .cmd .bat .sh .php .py .js)，以及运行程序或脚本所需的所有支持文件。
6. 选中“创建”，将脚本上传到 Web 应用。 
   
    “WebJobs”边栏选项卡上的列表中会显示为 WebJob 指定的名称。
7. 若要运行 WebJob，请在列表中右键单击其名称，然后单击“运行”。
   
    ![运行 Web 作业](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateContinuous"></a>创建连续运行 Web 作业
1. 若要创建连续执行的 WebJob，请按照与创建单次运行的 WebJob 相同的步骤操作，但在“运行方式”框中选择“连续”。
2. 若要启动或停止连续 WebJob，右键单击列表中的 WebJob，然后单击“启动”或“停止”。

> [!NOTE]
> 如果 Web 应用在多个实例上运行，则连续运行的 Web 作业会在所有实例上运行。 按需 WebJobs 和计划 WebJobs 在 Microsoft Azure 针对负载平衡所选择的单个实例上运行。
> 
> 要使连续 Web 作业在所有实例上可靠运行，请启用 Web 应用的 AlwaysOn* 配置设置；否则，Web 作业将在 SCM 主机站点闲置时间太长时停止运行。
> 
> 

## <a name="CreateScheduledCRON"></a>使用 CRON 表达式创建计划的 Web 作业
此方法可用于在基本、标准或高级模式下运行的 Web 应用，但需要在应用上启用“AlwaysOn”设置。

若要将按需 Web 作业变成按计划的 Web 作业，只需在 Web 作业 zip 文件的根目录中包含 `settings.job` 文件。 此 JSON 文件应包括 `schedule` 属性和 [CRON 表达式](https://en.wikipedia.org/wiki/Cron)，如下例所示。

CRON 表达式由 6 个字段组成：`{second} {minute} {hour} {day} {month} {day of the week}`。

例如，若要每 15 分钟触发一次 Web 作业，`settings.job` 需要：

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

其他 CRON 计划示例：

* 每隔 1 小时（即每当分钟数为 0 时）：`0 0 * * * *` 
* 从上午 9 点到下午 5 点每隔一小时：`0 0 9-17 * * *` 
* 每天上午 9:30：`0 30 9 * * *`
* 每个工作日的上午 9:30：`0 30 9 * * 1-5`

**注意**：从 Visual Studio 部署 WebJob 时，请确保将 `settings.job` 文件属性标记为“如果较新则复制”。

## <a name="CreateScheduled"></a>使用 Azure 计划程序创建计划的 Web 作业
以下备用技术利用 Azure 计划程序。 在这种情况下，Web 作业没有计划的任何直接知识。 而是将 Azure 计划程序配置为按计划触发 Web 作业。 

Azure 门户尚不能创建计划 WebJob，但在增添该功能之前，可使用[经典门户](http://manage.windowsazure.com)执行此类操作。

1. 在[经典门户](http://manage.windowsazure.com)中，转到 WebJob 页面，然后单击“添加”。
2. 在“运行方式”框中，选择“按计划运行”。
   
    ![新的计划作业][NewScheduledJob]
3. 为作业选择“计划程序区域”，然后单击对话框右下角的箭头进入下一个屏幕。
4. 在“创建作业”对话框中，选择想要的“定期”类型：“一次作业”或“定期作业”。
   
    ![安排定期计划][SchdRecurrence]
5. 此外，选择“开始”时间：“现在”或“特定时间”。
   
    ![计划开始时间][SchdStart]
6. 如果你想要在特定时间开始，请在“开始时间”下选择开始时间值。
   
    ![计划在特定时间启动][SchdStartOn]
7. 如果你选择定期作业，请使用“定期间隔”选项指定执行频率，并使用“结束时间”选项指定结束时间。
   
    ![安排定期计划][SchdRecurEvery]
8. 如果选择“周”，可以选中“按特定计划”框，并指定你想要在星期几运行作业。
   
    ![计划每周的每一天][SchdWeeksOnParticular]
9. 如果选择“月”并选中“按特定计划”框，可以设置在每月的特定编号“日”运行作业。 
   
    ![计划每月的特定日期][SchdMonthsOnPartDays]
10. 如果选择“工作日”，可以选择想要在一个月中的哪个或哪些工作日运行作业。
    
     ![计划一个月中的特定工作日][SchdMonthsOnPartWeekDays]
11. 最后，还可以使用“次数”选项，选择想要在一个月中的哪一周（第一周、第二周、第三周等）的指定工作日运行作业。
    
    ![计划一个月中的特定周的特定工作日][SchdMonthsOnPartWeekDaysOccurences]
12. 创建一个或多个作业后，其名称将与其状态、计划类型和其他信息一起显示在“Web 作业”选项卡中。 保留最近 30 个 WebJobs 的历史记录信息。
    
    ![作业列表][WebJobsListWithSeveralJobs]

### <a name="Scheduler"></a>计划作业和 Azure 计划程序
可在[经典门户](http://manage.windowsazure.com)的“Azure 计划程序”页中进一步配置计划作业。

1. 在“WebJobs”页上，单击作业的**计划**链接，以导航到 Azure 计划程序门户页。 
   
   ![链接到 Azure 计划程序][LinkToScheduler]
2. 在计划程序页上，单击该作业。
   
    ![计划程序门户页上的作业][SchedulerPortal]
3. 打开“作业操作”页，从中可以进一步配置作业。 
   
    ![作业操作 PageInScheduler][JobActionPageInScheduler]

## <a name="ViewJobHistory"></a>查看作业历史记录
1. 若要查看作业的执行历史记录（包括使用 WebJobs SDK 创建的作业），请单击“WebJobs”边栏选项卡的“日志”列下方的相应链接。 （如果需要，你可以使用剪贴板图标将日志文件页的 URL 复制到剪贴板中。）
   
    ![日志链接](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. 单击链接将打开 Web 作业的详细信息页。 此页显示运行命令的名称、运行的最后时间及其成功或失败状态。 在“最近运行的作业”下，单击某个时间以查看更多详细信息。
   
    ![WebJobDetails][WebJobDetails]
3. “WebJob 运行详细信息”页显示。 单击“切换输出”，查看日志内容的文本。 输出日志使用文本格式。 
   
    ![Web 作业运行详细信息][WebJobRunDetails]
4. 若要在单独的浏览器窗口中查看输出文本，请单击“下载”链接。 若要下载文本本身，请右键单击该链接，并使用你的浏览器选项来保存该文件的内容。
   
    ![下载日志输出][DownloadLogOutput]
5. 页面顶部的 **WebJobs** 链接是访问历史记录仪表板上的 WebJobs 列表的简捷方法。
   
    ![Web 作业列表链接][WebJobsLinkToDashboardList]
   
    ![历史记录仪表板中的 Web 作业列表][WebJobsListInJobsDashboard]
   
    单击其中一个链接可将你带到所选作业的 Web 作业详细信息页。

## <a name="WHPNotes"></a>说明
* 如果没有对 scm（部署）站点发出任何请求，并且未在 Azure 中打开 Web 应用的门户，则免费模式下的 Web 应用可能会在 20 分钟后超时。 对实际站点发出的请求不会对此进行重置。
* 需要对连续作业的代码进行编写，才能使其在无穷循环中运行。
* 仅当 Web 应用启动后连续作业才能连续运行。
* 基本和标准模式提供了 AlwaysOn 功能，启用该功能可防止 Web 应用进入空闲状态。
* 仅可以调试连续运行 Web 作业。 不支持按计划或按需调试 Web 作业。

## <a name="NextSteps"></a>后续步骤
有关详细信息，请参阅 [Azure WebJobs 推荐资源][WebJobsRecommendedResources]。

[PSonWebJobs]:http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx
[WebJobsRecommendedResources]:http://go.microsoft.com/fwlink/?LinkId=390226

[OnDemandWebJob]: ./media/web-sites-create-web-jobs/01aOnDemandWebJob.png
[WebJobsList]: ./media/web-sites-create-web-jobs/02aWebJobsList.png
[NewContinuousJob]: ./media/web-sites-create-web-jobs/03aNewContinuousJob.png
[NewScheduledJob]: ./media/web-sites-create-web-jobs/04aNewScheduledJob.png
[SchdRecurrence]: ./media/web-sites-create-web-jobs/05SchdRecurrence.png
[SchdStart]: ./media/web-sites-create-web-jobs/06SchdStart.png
[SchdStartOn]: ./media/web-sites-create-web-jobs/07SchdStartOn.png
[SchdRecurEvery]: ./media/web-sites-create-web-jobs/08SchdRecurEvery.png
[SchdWeeksOnParticular]: ./media/web-sites-create-web-jobs/09SchdWeeksOnParticular.png
[SchdMonthsOnPartDays]: ./media/web-sites-create-web-jobs/10SchdMonthsOnPartDays.png
[SchdMonthsOnPartWeekDays]: ./media/web-sites-create-web-jobs/11SchdMonthsOnPartWeekDays.png
[SchdMonthsOnPartWeekDaysOccurences]: ./media/web-sites-create-web-jobs/12SchdMonthsOnPartWeekDaysOccurences.png
[RunOnce]: ./media/web-sites-create-web-jobs/13RunOnce.png
[WebJobsListWithSeveralJobs]: ./media/web-sites-create-web-jobs/13WebJobsListWithSeveralJobs.png
[WebJobLogs]: ./media/web-sites-create-web-jobs/14WebJobLogs.png
[WebJobDetails]: ./media/web-sites-create-web-jobs/15WebJobDetails.png
[WebJobRunDetails]: ./media/web-sites-create-web-jobs/16WebJobRunDetails.png
[DownloadLogOutput]: ./media/web-sites-create-web-jobs/17DownloadLogOutput.png
[WebJobsLinkToDashboardList]: ./media/web-sites-create-web-jobs/18WebJobsLinkToDashboardList.png
[WebJobsListInJobsDashboard]: ./media/web-sites-create-web-jobs/19WebJobsListInJobsDashboard.png
[LinkToScheduler]: ./media/web-sites-create-web-jobs/31LinkToScheduler.png
[SchedulerPortal]: ./media/web-sites-create-web-jobs/32SchedulerPortal.png
[JobActionPageInScheduler]: ./media/web-sites-create-web-jobs/33JobActionPageInScheduler.png


