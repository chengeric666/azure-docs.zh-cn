---
title: "预配 Azure DocumentDB 的吞吐量 | Microsoft 文档"
description: "了解如何为 DocumentDB 集合设置预配吞吐量。"
services: documentdb
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: abdf0af8a85db19c68a0d74c0477d798c0fd03fc
ms.openlocfilehash: 8ff2fc6106438e35b93112a6dc97814ba79b06fc
ms.lasthandoff: 02/22/2017


---

# <a name="set-throughput-for-azure-documentdb-collections"></a>为 Azure DocumentDB 集合设置吞吐量

可以在 Azure 门户中或使用客户端 SDK 为 DocumentDB 集合设置吞吐量。 

下表列出了集合可用的吞吐量：

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>单分区集合</strong></p></td>
            <td valign="top"><p><strong>分区集合</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>最小吞吐量</p></td>
            <td valign="top"><p>400 个请求单位/秒</p></td>
            <td valign="top"><p>2,500 个请求单位/秒</p></td>
        </tr>
        <tr>
            <td valign="top"><p>最大吞吐量</p></td>
            <td valign="top"><p>10,000 个请求单位/秒</p></td>
            <td valign="top"><p>不受限制</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE] 
> 将分区集合设置为介于 2,500 RU/秒和 10,000 RU/秒之间的吞吐量值时，必须暂时使用 Azure 门户。 此功能目前在 SDK 中不可用。

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>使用 Azure 门户设置吞吐量

1. 在新窗口中，打开 [Azure 门户](https://portal.azure.com)。
2. 在左侧栏中单击“NoSQL (DocumentDB)”，或者单击底部的“更多服务”，滚动到“数据库”，然后单击“NoSQL (DocumentDB)”。
3. 选择 DocumentDB 帐户。
4. 在新窗口中，在“集合”下单击“缩放”，如以下屏幕截图中所示。
5. 在新窗口中，从下拉列表中选择你的集合，更改**吞吐量**值，然后单击“保存”。

    ![显示如何在 Azure 门户中通过导航到帐户，然后单击“缩放”来更改集合的吞吐量的屏幕截图](./media/documentdb-set-throughput/azure-documentdb-change-throughput-value.png)

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-net-sdk"></a>使用 .NET SDK 设置吞吐量

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>吞吐量常见问题

**是否可以将吞吐量设置为小于 400 RU/s？**

400 RU/s 是 DocumentDB 单个分区集合上可用的最小吞吐量（2500 RU/s 是分区集合的最小值）。 请求单位按 100 RU/s 间隔进行设置，但吞吐量不能设置为 100 RU/s 或小于 400 RU/s 的任何值。 如果在寻找一种经济高效的方法来开发和测试 DocumentDB，则可以使用免费的 [DocumentDB 模拟器](documentdb-nosql-local-emulator.md)（可以在本地免费部署）。 

## <a name="next-steps"></a>后续步骤

若要了解有关使用 DocumentDB 进行预配和全球扩展的详细信息，请参阅[使用 DocumentDB 进行分区和缩放](documentdb-partition-data.md)。

