---
title: "在经典门户中将 Hyper-V VM 复制到 Azure | Microsoft 文档"
description: "本文介绍当 Hyper-V 虚拟机不在 VMM 云中托管时，如何将 Hyper-V 虚拟机复制到 Azure。"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/21/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 2b5e2c2d5cc572ba951a61fd1eb78613c43ec453
ms.lasthandoff: 03/25/2017


---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>使用 Azure Site Recovery 在本地 Hyper-V 虚拟机与 Azure 之间复制（不使用 VMM）
> [!div class="op_single_selector"]
> * [Azure 门户](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell - 资源管理器](site-recovery-deploy-with-powershell-resource-manager.md)
> * [经典门户](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

本文介绍如何在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务，将本地 Hyper-V 虚拟机复制到 Azure。 在本方案中，Hyper-V 服务器不在 VMM 云中管理。

阅读本文后，欢迎在页面底部发表任何看法，或者在 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)上咨询技术问题。


## <a name="site-recovery-in-the-azure-portal"></a>Azure 门户中的 Site Recovery

Azure 提供了两个不同的[部署模型](../resource-manager-deployment-model.md)来创建和处理资源 – Azure Resource Manager 模型和经典模型。 Azure 还有两个门户 – Azure 经典门户和 Azure 门户。

本文介绍如何在经典门户中部署。 可以使用经典门户维护现有的保管库。 无法使用经典门户创建新的保管库。

## <a name="site-recovery-in-your-business"></a>业务中的站点恢复

组织需要制定 BCDR 策略来确定应用和数据如何在计划和非计划停机期间保持运行和可用，并尽快恢复正常运行情况。 以下是站点恢复可以提供的帮助：

* 为 Hyper-V VM 上运行的企业应用提供场外保护。
* 提供单个位置用于设置、管理和监视复制、故障转移及恢复。
* 可轻松故障转移到 Azure，并从 Azure 故障回复（还原）到本地站点中的 Hyper-V 主机服务器。
* 提供包含多个 VM 的恢复计划，以便分层应用程序工作负荷一起故障转移。

## <a name="azure-prerequisites"></a>Azure 先决条件
* 需要一个 [Microsoft Azure](https://azure.microsoft.com/) 帐户。 你可以从 [免费试用版](https://azure.microsoft.com/pricing/free-trial/)开始。
* 需要使用一个 Azure 存储帐户来存储复制的数据。 需要为帐户启用地域复制。 该帐户应位于 Azure Site Recovery 保管库所在的同一区域，并与同一订阅相关联。 [了解有关 Azure 存储的详细信息](../storage/storage-introduction.md)。 请注意，我们不支持跨资源组移动使用[新 Azure 门户](../storage/storage-create-storage-account.md)创建的存储帐户。
* 你需要一个 Azure 虚拟网络，以便在从主站点故障转移时，Azure 虚拟机可以连接到网络。

## <a name="hyper-v-prerequisites"></a>Hyper-V 先决条件
* 在源本地站点中，需要一台或多台运行 **Windows Server 2012 R2** 且装有 Hyper-V 角色的服务器，或运行 **Microsoft Hyper-V Server 2012 R2** 的服务器。 此服务器应该：
* 包含一个或多个虚拟机。
* 直接或通过代理连接到 Internet。
* 正在运行知识库文章 [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") 中所述的修复程序。

## <a name="virtual-machine-prerequisites"></a>虚拟机先决条件
要保护的虚拟机应符合 [Azure 虚拟机要求](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)。

## <a name="provider-and-agent-prerequisites"></a>提供程序和代理先决条件
在部署 Azure Site Recovery 的过程中，你将在每个 Hyper-V 服务器上安装 Azure Site Recovery 提供程序和 Azure 恢复服务代理。 请注意：

* 建议你始终运行最新版本的提供程序和代理。 可以在站点恢复门户中获取这些程序。
* 保管库中的所有 Hyper-V 服务器应有相同版本的提供程序和代理。
* 服务器上运行的提供程序通过 Internet 连接到站点恢复。 你可以使用 Hyper-V 服务器上当前配置的代理设置或者使用你在安装提供程序期间配置的自定义代理设置，在不使用代理的情况下完成部署。 需确保你要使用的代理服务器可以访问这些 URL 以连接到 Azure：

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* 此外，允许 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=41653)中所述的 IP 地址，以及 HTTPS (443) 协议。 必须将打算使用的 Azure 区域以及美国西部的 IP 范围加入允许列表。

此图显示了站点恢复用来完成业务流程和复制的不同通信通道和端口

![B2A 拓扑](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>步骤 1：创建保管库
1. 登录到[管理门户](https://portal.azure.com)。
2. 展开“**数据服务**” > “**恢复服务**”，并单击“**Site Recovery 保管库**”。
3. 单击“**新建**” > “**快速创建**”。
4. 在“名称”中，输入一个友好名称以标识此保管库。
5. 在“区域” 中，为保管库选择地理区域。 若要查看受支持的区域，请参阅 [Azure Site Recovery 价格详细信息中的](https://azure.microsoft.com/pricing/details/site-recovery/) “地域可用性”。
6. 单击“创建保管库” 。

    ![新保管库](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

检查状态栏以确认保管库已成功创建。 保管库将以“活动”形式列在主要的“恢复服务”页上  。

## <a name="step-2-create-a-hyper-v-site"></a>步骤 2：创建 Hyper-V 站点
1. 在“恢复服务”页中，单击保管库以打开“快速启动”页。 也可随时使用该图标打开“快速启动”。

    ![快速启动](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. 在下拉列表中，选择“**本地 Hyper-V 站点与 Azure 之间**”。

    ![Hyper-V 站点方案](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. 在“**创建 Hyper-V 站点**”中，单击“**创建 Hyper-V 站点**”。 指定站点名称并保存。

    ![Hyper-V 站点](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-the-provider-and-agent"></a>步骤 3：安装提供程序和代理
在你要保护的 VM 所在的每个 Hyper-V 服务器上安装提供程序和代理。

如果要在 Hyper-V 群集上进行安装，请在故障转移群集中的每个节点上执行步骤 5-11。 在注册所有节点并启用保护后，即使跨群集中的节点迁移虚拟机，这些虚拟机也会受到保护。

1. 在“**准备 Hyper-V 服务器**”中，单击“**下载注册密钥**”文件。
2. 在“**下载注册密钥**”页上，单击站点旁边的“**下载**”。 将密钥下载到可由 Hyper-V 服务器方便访问的安全位置。 该密钥生成后，有效期为 5 天。

    ![注册密钥](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. 单击“**下载该提供程序**”以获取最新版本。
4. 在想要注册到保管库中的每个 Hyper-V 服务器上运行该文件。 该文件将安装两个组件：
   * **Azure Site Recovery 提供程序**—处理 Hyper-V 服务器与 Azure Site Recovery 门户之间的通信和协调。
   * **Azure 恢复服务代理**—处理源 Hyper-V 服务器上运行的虚拟机与 Azure 存储之间的数据传输。
5. 在“Microsoft 更新”中，你可以选择获取更新  。 启用此设置后，将会根据你的 Microsoft Update 策略安装提供程序和代理更新。

    ![Microsoft 更新](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. 在“**安装**”中，指定要将提供程序和代理安装在 Hyper-V 服务器上的哪个位置。

    ![安装位置](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. 完成安装后，请继续设置以在保管库中注册服务器。
8. 在“**保管库设置**”页上，单击“**浏览**”以选择密钥文件。 指定 Azure Site Recovery 订阅、保管库名称和 Hyper-V 服务器属于的 Hyper-V 站点。

    ![服务器注册](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. 在“**Internet 连接**”页上，指定提供程序如何连接到 Azure Site Recovery。 选择“使用默认系统代理设置”以使用服务器上配置的默认 Internet 连接设置。 如果你未指定值，系统将使用默认设置。

   ![Internet 设置](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. 注册过程将开始在保管库中注册服务器。

    ![服务器注册](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. 完成注册后，Azure Site Recovery 将检索 Hyper-V 服务器中的元数据，该服务器将显示在保管库中“**服务器**”页上的“**Hyper-V 站点**”选项卡上。

### <a name="install-the-provider-from-the-command-line"></a>从命令行安装提供程序
或者，你可以从命令行安装 Azure Site Recovery 提供程序。 如果要在运行 Windows Server Core 2012 R2 的计算机上安装提供程序，则应该使用此方法。 从命令行运行如下所示的命令：

1. 将提供程序安装文件和注册密钥下载到某个文件夹中。 例如 C:\ASR。
2. 以管理员身份打开命令提示符并键入：

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. 然后运行以下命令以安装提供程序：

        C:\ASR> setupdr.exe /i
4. 运行以下命令以完成注册：

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

其中的参数包括：

* **/Credentials**：指定下载的注册密钥所在的位置。
* **/FriendlyName**：指定用于标识 Hyper-V 主机服务器的名称。 此名称将显示在门户中
* **/EncryptionEnabled**：可选。 指定是否要在 Azure 中加密副本虚拟机（静态加密）。
* **/proxyAddress**；**/proxyport**；**/proxyUsername**；**/proxyPassword**：可选。 如果想要使用自定义代理，或现有的代理需要身份验证，请指定代理参数。

## <a name="step-4-create-an-azure-storage-account"></a>步骤 4：创建 Azure 存储帐户
* 在“**准备资源**”中，选择“**创建存储帐户**”以创建一个 Azure 存储帐户（如果你没有的话）。 该帐户应已启用地域复制。 该帐户应位于 Azure Site Recovery 保管库所在的同一区域，并与同一订阅相关联。

    ![创建存储帐户](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. 我们不支持跨资源组移动使用[新 Azure 门户](../storage/storage-create-storage-account.md)创建的存储帐户。
> 2. 用于部署 Site Recovery 的存储帐户不支持跨同一订阅中的资源组或跨订阅[迁移存储帐户](../azure-resource-manager/resource-group-move-resources.md)。
>

## <a name="step-5-create-and-configure-protection-groups"></a>步骤 5：创建并配置保护组
保护组是你想要使用相同的保护设置保护的虚拟机的逻辑分组。 可以将保护设置应用到保护组，这些设置将应用到你在组添加的所有虚拟机。

1. 在“**创建和配置保护组**”中，单击“**创建保护组**”。 如果缺少任何必备组件，系统将发出一条消息，你可以单击“**查看详细信息**”来了解详细信息。
2. 在“**保护组**”选项卡中，添加一个保护组。 指定名称、源 Hyper-V 站点、目标 **Azure**、你的 Azure Site Recovery 订阅名称和 Azure 存储帐户。

    ![保护组](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. 在“**复制设置**”中设置“**复制频率**”，以指定在源和目标之间同步数据差异的频率。 可以设置为 30 秒、5 分钟或 15 分钟。
4. 在“**保留恢复点**”中，指定恢复历史记录应存储的小时数。
5. 在“**与应用程序一致的快照的频率**”中，可以指定是否拍摄使用卷影复制服务 (VSS) 的快照来确保在拍摄快照时应用程序处于一致状态。 默认情况下不拍摄这些快照。 请确保将此值设置为小于配置的附加恢复点数。 仅当虚拟机运行 Windows 操作系统时，才支持此操作。
6. 在“**初始复制开始时间**”中，指定何时应将保护组中虚拟机的初始复制发送到 Azure。

    ![保护组](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>步骤 6：启用虚拟机保护
将虚拟机添加到保护组以启用虚拟机保护。

> [!NOTE]
> 不支持保护运行 Linux 的、使用静态 IP 地址的 VM。
>
>

1. 在保护组的“**计算机**”选项卡上，单击“**将虚拟机添加到保护组以启用保护**”。
2. 在“**启用虚拟机保护**”页上，选择你要保护的虚拟机。

    ![启用虚拟机保护](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    随后将开始运行“启用保护”作业。 你可以在“**作业**”选项卡上跟踪进度。 在“完成保护”作业运行之后，虚拟机就可以进行故障转移了。
3. 在设置保护后，你可以：

   * 在“**受保护的项**” > “**保护组**” > “*protectiongroup_name*” > “**虚拟机**”中查看虚拟机。可以在“**属性**”选项卡中深入查看虚拟机详细信息。
   * 在“**受保护的项**” > “**保护组**” > “*protectiongroup_name*” > “**虚拟机**”>“*virtual_machine_name*” > “**配置**”中配置虚拟机的故障转移属性。 你可以配置：

     * **名称**：虚拟机在 Azure 中的名称。
     * **大小**：故障转移的虚拟机的目标大小。

       ![配置虚拟机属性](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * 在“*受保护的项*”* > *“*保护组*”* > “*protectiongroup_name*” > “**虚拟机**”>“*virtual_machine_name*” > “**配置*”* 中配置其他虚拟机设置，包括：

     * **网络适配器**：网络适配器数目根据你为目标虚拟机指定的大小来确定。 查看[虚拟机大小规格](../virtual-machines/virtual-machines-linux-sizes.md)，了解虚拟机大小所支持的 NIC 数目。

       修改虚拟机的大小并保存设置后，下一次打开“ **配置** ”页时，网络适配器的数量将会改变。 目标虚拟机的网络适配器数目是源虚拟机上网络适配器的最小数目和所选虚拟机大小支持的网络适配器的最大数目。 解释如下：

       * 如果源计算机上的网络适配器数小于或等于目标计算机大小允许的适配器数，则目标的适配器数将与源相同。
       * 如果源虚拟机的适配器数大于目标大小允许的数目，则使用目标大小允许的最大数目。
       * 例如，如果源计算机有两个网络适配器，而目标计算机大小支持四个，则目标计算机将有两个适配器。 如果源计算机有两个适配器，但支持的目标大小只支持一个，则目标计算机只有一个适配器。

     * **Azure 网络**：指定虚拟机应故障转移到的网络。 如果虚拟机有多个网络适配器，所有适配器应连接到同一个 Azure 网络。
     * **子网**：对于虚拟机上的每个网络适配器，请在 Azure 网络中选择故障转移后计算机应连接到的子网。
     * **目标 IP 地址**：如果源虚拟机的网络适配器配置为使用静态 IP 地址，那么你可以指定目标虚拟机的 IP 地址，以确保计算机在故障转移后具有相同的 IP 地址。  如果不指定 IP 地址，将在故障转移时分配任何可用的地址。 如果指定了正在使用的地址，故障转移将会失败。

     > [!NOTE]
     > 用于部署 Site Recovery 的网络不支持跨同一订阅中的资源组或跨订阅[迁移网络](../azure-resource-manager/resource-group-move-resources.md)。
     >

     ![配置虚拟机属性](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>步骤 7：创建恢复计划
为了对部署进行测试，你可以针对单个虚拟机或单个恢复计划（其中包含一个或多个虚拟机）运行测试性故障转移。 [详细了解](site-recovery-create-recovery-plans.md)如何创建恢复计划。

## <a name="step-8-test-the-deployment"></a>步骤 8：测试部署
可通过两种方式运行到 Azure 的测试故障转移。

* **没有 Azure 网络的测试故障转移**- 此类型的测试故障转移检查虚拟机是否可以在 Azure 中正常运行。 在故障转移后，虚拟机不会连接到任何 Azure 网络。
* **使用 Azure 网络的测试故障转移**—此类型的故障转移检查整个复制环境是否可以按预期运行，并且在故障转移后，虚拟机是否连接到指定的目标 Azure 网络。 请注意，对于测试故障转移，将根据副本虚拟机的子网确定测试虚拟机的子网。 这不同于常规复制，在常规复制中，副本虚拟机的子网是根据源虚拟机的子网确定的。

如果你想要运行测试故障转移而未指定 Azure 网络，则不需要做任何准备。

若要运行具有目标 Azure 网络的测试性故障转移，你将需要创建一个与你的 Azure 生产网络相隔离的新 Azure 网络（你在 Azure 中新建网络时的默认行为）。 阅读[运行测试故障转移](site-recovery-failover.md)以获取更多详细信息。

若要完全测试复制和网络部署，你需要设置基础结构，以便复制的虚拟机按预期工作。 一种做法是将虚拟机设置为使用 DNS 的域控制器，并使用站点恢复将其复制到 Azure，以通过运行测试故障转移在测试网络中创建该域控制器。  [阅读](site-recovery-active-directory.md#test-failover-considerations)有关 Active Directory 测试故障转移注意事项的详细信息。

按如下所述运行测试故障转移：

> [!NOTE]
> 在为 Azure 执行故障转移时，若要获得最佳性能，请确保已在受保护计算机中安装 Azure 代理。 这有助于加快启动速度，并且也对出现问题时的诊断有所帮助。 Linux 代理可在[此处](https://github.com/Azure/WALinuxAgent)找到 - Windows 代理可在[此处](http://go.microsoft.com/fwlink/?LinkID=394789)找到
>
>

1. 在“**恢复计划**”选项卡上，选择该计划并单击“**测试故障转移**”。
2. 在“**确认测试故障转移**”页上，选择“**无**”或选择一个特定的 Azure 网络。  请注意，如果你选择了“**无**”，则测试故障转移将检查虚拟机是否可以正确复制到 Azure，但不会检查你的复制网络配置。

    ![测试故障转移](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. 在“作业”选项卡上，你可以跟踪故障转移进度  。 在 Azure 门户中，你应当也能够看到虚拟机测试副本。 如果你已设置为从本地网络访问虚拟机，则可以启动与虚拟机的远程桌面连接。
4. 在故障转移到达**完成测试**阶段时，单击“**完成测试**”以结束测试故障转移。 你可以向下钻取到“作业”选项卡来跟踪故障转移进度和状态以及执行所需的任何操作  。
5. 在故障转移后，你将能够在 Azure 门户中看到虚拟机测试副本。 如果你已设置为从本地网络访问虚拟机，则可以启动与虚拟机的远程桌面连接。

   1. 验证虚拟机成功启动。
   2. 如果想要在故障转移之后使用远程桌面连接到 Azure 中的虚拟机，请在虚拟机上启用远程桌面连接，然后运行测试故障转移。 还需要在虚拟机上添加 RDP 终结点。 你可以利用 [Azure 自动化 Runbook](site-recovery-runbook-automation.md)来执行此操作。
   3. 故障转移后，如果想要在远程桌面中使用公共 IP 地址连接到 Azure 中的虚拟机，请确保没有任何域策略阻止你使用公共地址连接到虚拟机。
6. 完成测试后，执行以下操作：

   * 单击“测试故障转移已完成” 。 清理测试环境以自动关闭电源，并删除测试虚拟机。
   * 单击“说明”以记录并保存与测试故障转移相关联的任何观测结果  。
7. 当故障转移达到“**完成测试**”阶段时，请按如下所述完成验证：
   1. 在 Azure 门户中查看副本虚拟机。 验证虚拟机是否启动成功。
   2. 如果你已设置为从本地网络访问虚拟机，则可以启动与虚拟机的远程桌面连接。
   3. 单击“完成测试”以完成测试  。
   4. 单击“说明”以记录并保存与测试故障转移相关联的任何观测结果  。
   5. 单击“测试故障转移已完成” 。 清理测试环境以自动关闭电源，并删除测试虚拟机。

## <a name="next-steps"></a>后续步骤
设置并运行部署以后，请[详细了解](site-recovery-failover.md)故障转移。

