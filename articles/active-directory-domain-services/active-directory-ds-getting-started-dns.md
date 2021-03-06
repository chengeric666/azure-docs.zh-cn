---
title: "Azure AD 域服务：更新 Azure 虚拟网络的 DNS 设置 | Microsoft Docs"
description: "Azure Active Directory 域服务入门"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 16a0019990d4839a15acb4dcac747147a6d55e1d
ms.lasthandoff: 03/07/2017


---
# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure AD 域服务 - 更新 Azure 虚拟网络的 DNS 设置
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>任务 4：更新 Azure 虚拟网络的 DNS 设置
在前面的配置任务中，已成功为你的目录启用 Azure AD 域服务。 下一个任务是确保虚拟网络中的计算机可以连接和使用这些服务。 更新虚拟网络的 DNS 服务器设置，使其指向 Azure AD 域服务在虚拟网络上可用的两个 IP 地址。

> [!NOTE]
> 为目录启用 Azure AD 域服务后，记下目录的“ **配置** ”选项卡上显示的 Azure AD 域服务的 IP 地址。
>
>

若要更新已启用 Azure AD 域服务的虚拟网络的 DNS 服务器设置，请执行下列配置步骤。

1. 导航到 **Azure 经典门户** ([https://manage.windowsazure.com](https://manage.windowsazure.com))。
2. 在左窗格上选择“ **网络** ”节点。

    ![虚拟网络节点](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. 在“ **虚拟网络** ”选项卡上，选择其中启用了 Azure AD 域服务的虚拟网络，以查看其属性。
4. 单击“配置”  选项卡。

    ![虚拟网络节点](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. 在“ **DNS 服务器** ”部分，输入 Azure AD 域服务的 IP 地址。
6. 确保输入目录的“配置”选项卡上“域服务”部分显示的两个 IP 地址。
7. 若要保存此虚拟网络的 DNS 服务器设置，请单击页面底部任务窗格上的“保存”。

   ![更新虚拟网络的 DNS 服务器设置。](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
> 更新虚拟网络的 DNS 服务器设置后，网络上的虚拟机可能需要一些时间才能获取更新的 DNS 配置。 如果虚拟机无法连接到域，可以刷新虚拟机上的 DNS 缓存（例如， “ipconfig /flushdns”）。 此命令会强制刷新虚拟机上的 DNS 设置。
>
>

## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>任务 5 - 启用 Azure AD 域服务密码同步
下一个配置任务是任务“ [启用 Azure AD 域服务密码同步](active-directory-ds-getting-started-password-sync.md)”。

