---
title: "在 BizTalk 服务中不同状态下可执行的任务 | Microsoft Docs"
description: "不同 MABS 状态下允许执行的操作：停止、启动、重启、挂起、恢复、删除、缩放、更新配置和备份"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 331c03cd0819aa4935f9b486ff38f54d23d6a7fd
ms.openlocfilehash: e3d5f89b1c8525f791e73667d6f7cd6a999ab971


---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>使用 BizTalk 服务状态可以和不可以执行的操作
根据 BizTalk 服务的当前状态，有一些操作不一定能够在 BizTalk 服务上完成。

例如，在 Azure 经典门户中预配新的 BizTalk 服务。 在其成功完成后，BizTalk 服务处于 `active` 状态。 在活动状态下，可以停止、挂起和删除 BizTalk 服务。 如果停止 BizTalk 服务但停止失败，则 BizTalk 服务将转为 `StopFailed` 状态。 在 `StopFailed` 状态下，可以重启 BizTalk 服务。 如果尝试执行某一不允许的操作，例如恢复，将会发生以下错误：

`Operation not allowed`

## <a name="view-the-possible-states"></a>查看可能的状态

下表列出了在 BizTalk 服务处于某一特定状态时可执行的操作。 ✔ 意味着处于该状态时允许执行该操作。 空白条目表示处于该状态时不能执行的操作。

| 服务状态 | 开始 | 停止 | 重新启动 | 挂起 | 恢复 | 删除 | 缩放 | 更新 <br/> 配置 | 备份 |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| 活动 |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| 已禁用 |  |  |  |  |  | ✔ | |  |  | 
| 已挂起 |  |  |  |  | ✔ | ✔ | |  | ✔ |
| 已停止 | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| 服务更新失败 |  |  |  |  |  | ✔ | |  |  | 
| 禁用失败 |  |  |  |  |  | ✔ | |  |  | 
| 启用失败 |  |  |  |  |  | ✔ | |  |  | 
| 启动失败 <br/> 停止失败 <br/> 重新启动失败 | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| 挂起失败 <br/> 恢复失败|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| 创建失败 <br/> 还原失败 |  |  |  |  |  | ✔ | |  |  | 
| 配置更新失败  |  |  | ✔ |  |  | ✔ | |✔ | |
| 缩放失败 |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>另请参阅
* [使用 Azure 经典门户创建 BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [可在 BizTalk 服务中的“仪表板”、“监视”和“缩放”选项卡中执行的操作](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [使用 BizTalk 服务中的开发人员版、基本版、标准版和高级版可获取的内容](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [如何备份和还原 BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk 服务中所述的限制](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [为 BizTalk 服务检索服务总线和访问控制颁发者名称以及颁发者密钥值](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [如何开始使用 Azure BizTalk 服务 SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)




<!--HONumber=Nov16_HO3-->


