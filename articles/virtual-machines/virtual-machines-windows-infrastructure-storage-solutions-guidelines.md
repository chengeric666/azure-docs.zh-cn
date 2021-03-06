---
title: "Azure 中 Windows VM 的存储解决方案 | Microsoft 文档"
description: "了解用于在 Azure 基础结构服务中部署存储解决方案的关键设计和实施准则。"
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: f692f98beaee16bef24bb7fbf716a9b4b8edeb6c
ms.lasthandoff: 03/18/2017


---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>适用于 Windows VM 的 Azure 存储基础结构准则

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

本文重点介绍实现最佳虚拟机 (VM) 性能的存储需求和设计注意事项。

## <a name="implementation-guidelines-for-storage"></a>存储的实施准则
决策：

* 要使用 Azure 托管磁盘还是非托管磁盘？
* 需要为工作负荷使用标准存储还是高级存储？
* 是否需要进行磁盘条带化以创建大于 1023 GB 的磁盘？
* 是否需要进行磁盘条带化以获得工作负荷的最佳 I/O 性能？
* 你需要使用哪一组存储帐户来托管你的 IT 工作负荷或基础结构？

任务：

* 查看要部署的应用程序的 I/O 需求，并计划适当的存储帐户数目和类型。
* 使用命名约定创建存储帐户集。 你可以使用 Azure PowerShell 或门户。

## <a name="storage"></a>存储
Azure 存储空间是部署与管理虚拟机 (VM) 和应用程序的重要部分。 Azure 存储空间提供的服务可用于存储文件数据、非结构化数据和消息，该存储空间也是为 VM 提供支持的基础结构的一部分。

[Azure 托管磁盘](../storage/storage-managed-disks-overview.md)在幕后为你处理存储。 使用非托管磁盘时，需创建存储帐户来存储你的 Azure VM 的磁盘（VHD 文件）。 进行扩展时，必须确保创建了额外的存储帐户，以便任何磁盘都不会超出对存储的 IOPS 限制。 使用托管磁盘处理存储时，不再受限于存储帐户限制（例如 20,000 IOPS / 帐户）。 也不再需要将自定义映像（VHD 文件）复制到多个存储帐户。 可以在一个中心位置管理自定义映像（每个 Azure 区域一个存储帐户），并使用它们在一个订阅中创建数百台 VM。 我们建议你使用托管磁盘进行新部署。

有两种可为 VM 提供支持的存储帐户：

* 标准存储帐户可以访问 Blob 存储（用于存储 Azure VM 磁盘）、表存储、队列存储和文件存储。
* [高级存储](../storage/storage-premium-storage.md)帐户可为 I/O 密集型工作负荷（例如 AlwaysOn 群集中的 SQL Server）提供高性能且低延迟的磁盘支持。 高级存储目前仅支持 Azure VM 磁盘。

Azure 使用一个操作系统磁盘、一个临时磁盘和零个或更多可选数据磁盘创建 VM。 操作系统磁盘和数据磁盘是 Azure 页 blob，而临时磁盘则通过本地方式存储在计算机所在的节点上。 请注意，在设计应用程序时，务必仅将此临时磁盘用于非持久性数据，因为 VM 可能会在维护事件期间在主机之间迁移。 任何存储在临时磁盘上的数据都会丢失。

持久性和高可用性将由基础 Azure 存储环境提供，确保数据在发生非计划维护或硬件故障时受到保护。 设计 Azure 存储环境时，可以选择复制 VM 存储：

* 在给定的 Azure 数据中心内本地复制
* 在给定区域内的 Azure 数据中心之间复制
* 在不同区域中的 Azure 数据中心之间复制

了解[有关高可用性复制选项的详细信息](../storage/storage-introduction.md#replication-for-durability-and-high-availability)。

操作系统磁盘和数据磁盘的最大大小为 1023 千兆字节 (GB)。 Blob 的最大大小为 1024 GB，并且必须包含 VHD 文件的元数据（脚注）（1 GB 是 1024<sup>3</sup> 字节）。 你可以使用 Windows Server 2012 中的存储空间来超越此限制，方法是将数据磁盘整合在一起，以向 VM 提供大于 1023GB 的逻辑卷。

设计 Azure 存储部署时有几个可伸缩性限制，请参阅 [Microsoft Azure 订阅和服务限制、配额和约束条件](../azure-subscription-service-limits.md#storage-limits)，了解更多详细信息。 另请参阅 [Azure 存储可缩放性和性能目标](../storage/storage-scalability-targets.md)。

针对应用程序存储，可以使用 Blob 存储来存储非结构化对象数据，例如文档、映像、备份、配置数据、 日志等。 应用程序可以直接写入 Azure Blob 存储，而不是写入附加到 VM 的虚拟磁盘。 根据可用性需求和成本限制，Blob 存储还提供[热存储层和冷存储层](../storage/storage-blob-storage-tiers.md)选项。

## <a name="striped-disks"></a>条带化的磁盘
除了允许创建大于 1023 GB 的磁盘外，在许多情况下，对数据磁盘使用条带化还可增强性能，因为允许多个 blob 支持单个卷的存储。 使用条带化时，将会并行处理针对单个逻辑磁盘写入和读取数据所需的 I/O。

Azure 将对可用的数据磁盘数和带宽加以限制，具体取决于 VM 大小。 有关详细信息，请参阅[虚拟机大小](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

如果要对 Azure 数据磁盘使用磁盘条带化，请考虑以下准则：

* 数据磁盘应始终为最大大小 (1023 GB)。
* 附加 VM 大小所允许的最大数据磁盘量。
* 使用存储空间。
* 避免使用 Azure 数据磁盘缓存选项（缓存策略 =“无”）。

有关详细信息，请参阅[存储空间 - 专为提高性能设计](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx)。

## <a name="multiple-storage-accounts"></a>多个存储帐户
本节不适用于 [Azure 托管磁盘](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)，因为无需创建单独的存储帐户。 

为非托管磁盘设计 Azure 存储环境时，随着部署的 VM 数目增加，可以使用多个存储帐户。 此方法有助于将 I/O 分散到底层 Azure 存储基础结构上，以维持 VM 和应用程序的最佳性能。 设计要部署的应用程序时，请考虑每个 VM 的 I/O 需求，并使这些 VM 在 Azure 存储帐户之间取得均衡。 尽量避免将所有高 I/O 需求的 VM 分组在仅仅一个或两个存储帐户中。

有关不同 Azure 存储选项的 I/O 功能及一些建议最大值的详细信息，请参阅 [Azure 存储可缩放性和性能目标](../storage/storage-scalability-targets.md)。

## <a name="next-steps"></a>后续步骤
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


