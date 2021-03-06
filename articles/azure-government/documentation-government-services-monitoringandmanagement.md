---
title: "Azure 政府版监视 + 管理 | Microsoft Docs"
description: "这里提供了针对 Azure Government 的应用程序开发使用到的功能和指南对比。"
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: 4b7720c1-699e-432b-9246-6e49fb77f497
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 3/13/2017
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 4d7de786dc902cb1c32e70a1f69bc74282de44f1
ms.lasthandoff: 03/14/2017


---
# <a name="azure-government-monitoring--management"></a>Azure 政府版监视 + 管理
本文概述了对 Azure 政府版环境的监视和管理服务的变体和注意事项。

## <a name="automation"></a>自动化
自动化在 Azure 政府版中已正式可用。

### <a name="variations"></a>变体
当前，以下自动化功能在 Azure 政府版中尚不可用。

* 创建用于身份验证的服务主体凭据

有关详细信息，请参阅[自动化公共文档](../automation/automation-intro.md)。

## <a name="backup"></a>备份
备份在 Azure 政府版中已正式发布。

有关详细信息，请参阅 [Azure 政府版备份](documentation-government-services-backup.md)。

## <a name="resource-policy"></a>资源策略

[Azure 资源策略](../azure-resource-manager/resource-manager-policy.md)在 Azure 政府中不可用。

## <a name="site-recovery"></a>Site Recovery
Azure Site Recovery 已在 Azure 政府中正式发布。

有关详细信息，请参阅[站点恢复公共文档](../site-recovery/site-recovery-overview.md)。

### <a name="variations"></a>变体
当前，以下站点恢复功能在 Azure 政府版中尚不可用：

* Azure Resource Manager Site Recovery 保管库
* 电子邮件通知

| Site Recovery | 经典 | 资源管理器 |
| --- | --- | --- |
| VMWare/物理  | GA | GA |
| Hyper-V | GA | GA |
| 站点到站点 | GA | GA |

>[!NOTE]
>适用于美国弗吉尼亚州政府和美国爱荷华州政府的表格

Site Recovery 的以下 URL 在 Azure 政府中是不同的：

| Azure Public | Azure Government  | 说明 |
| --- | --- | --- |
| \*.hypervrecoverymanager.windowsazure.com | \*.hypervrecoverymanager.windowsazure.us | 访问 Site Recovery 服务 |
| \*.backup.windowsazure.com  | \*.backup.windowsazure.us | 访问保护服务 |
| \*.blob.core.windows.net | \*.blob.core.usgovcloudapi.net | 用于存储 VM 快照 |
| http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi | http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi | 下载 MySQL |

## <a name="log-analytics"></a>Log Analytics
Log Analytics 已在 Azure Government 中正式发布。

### <a name="variations"></a>变体
以下 Log Analytics 功能和解决方案当前在 Azure Government 中不可用。

* Microsoft Azure 中预览的解决方案包括：
  * 网络监视解决方案
  * 服务映射
  * Office 365 解决方案
  * Windows 10 升级分析解决方案
  * Application Insights 解决方案
  * Azure 网络分析解决方案
  * Azure 自动化分析解决方案
  * 密钥保管库分析解决方案
* 需要更新到本地软件的解决方案和功能包括：
  * Surface Hub 解决方案
* 公共 Azure 中预览的功能包括：
  * 将数据导出到 Power BI
* Azure 指标和 Azure 诊断
* Operations Management Suite 移动应用程序

Log Analytics 的 URL 在 Azure 政府版中是不同的：

| Azure Public | Azure Government  | 说明 |
| --- | --- | --- |
| mms.microsoft.com |oms.microsoft.us |Log Analytics 门户 |
| *workspaceId*.ods.opinsights.azure.com |*workspaceId*.ods.opinsights.azure.us |[数据收集器 API](../log-analytics/log-analytics-data-collector-api.md) |
| \*.ods.opinsights.azure.com |\*.ods.opinsights.azure.us |代理通信 - [配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |
| \*.oms.opinsights.azure.com |\*.oms.opinsights.azure.us |代理通信 - [配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |
| \*.blob.core.windows.net |\*.blob.core.usgovcloudapi.net |代理通信 - [配置防火墙设置](../log-analytics/log-analytics-proxy-firewall.md) |

以下 Log Analytics 功能在 Azure 政府版中的表现不同：

* 若要将 System Center Operations Manager 管理服务器连接到 Log Analytics，需要下载并导入更新后的管理包。
  + System Center Operations Manager 2016
    1. 安装 [System Center Operations Manager 2016 更新汇总 2](https://support.microsoft.com/help/3209591)。
    2. 将更新汇总 2 中附带的管理包导入 Operations Manager。 有关如何从磁盘导入管理包的信息，请参阅[如何导入 Operations Manager 管理包](http://technet.microsoft.com/library/hh212691.aspx)。
    3. 若要将 Operations Manager 连接到 Log Analytics，请按照[将 Operations Manager 连接到 Log Analytics](../log-analytics/log-analytics-om-agents.md) 中的步骤进行操作。
  + System Center Operations Manager 2012 R2 UR3（或更高版本）/Operations Manager 2012 SP1 UR7（或更高版本）
    1. 下载并保存[更新管理包](http://go.microsoft.com/fwlink/?LinkId=828749)。
    2. 解压缩已下载的文件。
    3. 将管理包导入 Operations Manager。 有关如何从磁盘导入管理包的信息，请参阅[如何导入 Operations Manager 管理包](http://technet.microsoft.com/library/hh212691.aspx)。
    4. 若要将 Operations Manager 连接到 Log Analytics，请按照[将 Operations Manager 连接到 Log Analytics](../log-analytics/log-analytics-om-agents.md) 中的步骤进行操作。
  
* 若要使用 [System Center Configuration Manager 2016 中的计算机组](../log-analytics/log-analytics-sccm.md)，需要使用 [Technical Preview 1701](https://docs.microsoft.com/en-us/sccm/core/get-started/technical-preview) 或更高版本。

### <a name="frequently-asked-questions"></a>常见问题
* 能否将数据从 Microsoft Azure 中的 Log Analytics 迁移到 Azure 政府版？
  * 不能。 无法将数据或工作区从 Microsoft Azure 迁移到 Azure 政府版。
* 能否从 Operations Management Suite Log Analytics 门户切换 Microsoft Azure 工作区和 Azure 政府版工作区？
  * 不能。 适用于 Microsoft Azure 和 Azure 政府版的门户是独立的，并且不共享信息。

有关详细信息，请参阅 [Log Analytics 公共文档](../log-analytics/log-analytics-overview.md)。

## <a name="next-steps"></a>后续步骤
有关补充信息和更新，请订阅 <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure 政府博客</a>。

