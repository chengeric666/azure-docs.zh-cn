---
title: "VM 重新启动或大小调整问题 | Microsoft Docs"
description: "排查在 Azure 中重新启动或调整现有 Linux 虚拟机时遇到的经典部署问题"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 466db1525d3f1a9bdde86643089493133f32bad1
ms.lasthandoff: 03/27/2017


---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>排查在 Azure 中重新启动或调整现有 Linux 虚拟机时遇到的经典部署问题
> [!div class="op_single_selector"]
> * [经典](restart-resize-error-troubleshooting.md)
> * [Resource Manager](../../virtual-machines-linux-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

当你尝试启动已停止的 Azure 虚拟机 (VM)，或调整现有 Azure VM 的大小时，经常遇到的错误是分配失败。 当群集或区域没有可用的资源或无法支持所请求的 VM 大小时，将发生此错误。

> [!IMPORTANT] 
> Azure 提供两个不同的部署模型用于创建和处理资源：[Resource Manager 和经典模型](../../../resource-manager-deployment-model.md)。 本文介绍如何使用经典部署模型。 Microsoft 建议大多数新部署使用资源管理器模型。 有关 Resource Manager 版本，请参阅[此处](../../virtual-machines-linux-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>收集审核日志
若要开始故障排除，请收集审核日志，以识别与问题相关的错误。

在 Azure 门户中，单击“浏览” > “虚拟机” >  *你的 Linux 虚拟机*  > “设置” > “审核日志”。

## <a name="issue-error-when-starting-a-stopped-vm"></a>问题：启动已停止的 VM 时发生错误
你尝试启动已停止的 VM，但出现分配失败。

### <a name="cause"></a>原因
必须在托管云服务的原始群集上尝试发出启动已停止 VM 的请求。 但是，群集没有足够的空间可完成该请求。

### <a name="resolution"></a>解决方法
* 创建新的云服务，并将它与区域或基于区域的虚拟网络（而不是地缘组）关联。
* 删除已停止的 VM。
* 使用磁盘在新的云服务中重新创建 VM。
* 启动重新创建的 VM。

如果在尝试创建新的云服务时收到错误，请稍后再试一次，或更改云服务的区域。

> [!IMPORTANT]
> 新的云服务将有新的名称和 VIP，因此需要为所有将信息用于现有云服务的依赖性更改该信息。
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>问题：调整现有 VM 的大小时发生错误
你尝试调整现有 VM 的大小，但出现分配失败。

### <a name="cause"></a>原因
必须在托管云服务的原始群集上尝试发出调整 VM 大小的请求。 但是，群集不支持请求的 VM 大小。

### <a name="resolution"></a>解决方法
减少请求的 VM 大小，然后重试调整大小请求。

* 单击“浏览全部” > “虚拟机(经典)” >  *你的虚拟机*  > “设置” > “大小”。 有关详细步骤，请参阅[调整虚拟机的大小](https://msdn.microsoft.com/library/dn168976.aspx)。

如果无法减少 VM 大小，请遵循以下步骤：

* 创建新的云服务，确保它不链接到地缘组，并且未与链接到地缘组的虚拟网络相关联。
* 在其中创建更大的新 VM。

可以在同一个云服务中合并所有 VM。 如果现有的云服务和基于区域的虚拟网络相关联，你可以将新云服务连接到现有虚拟网络。

如果现有的云服务未与基于区域的虚拟网络相关联，则必须删除现有云服务中的 VM，并在新云服务中从其磁盘重新创建 VM。 然而，请务必记得新的云服务将有新的名称和 VIP，因此需要为所有目前将此信息用于现有云服务的依赖性更新该信息。

## <a name="next-steps"></a>后续步骤
如果在 Azure 中创建新的 Linux VM 时遇到问题，请参阅[排查在 Azure 中新建 Linux 虚拟机时遇到的部署问题](../../virtual-machines-linux-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。


