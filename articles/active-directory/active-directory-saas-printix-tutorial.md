---
title: "教程：Azure Active Directory 与 Printix 集成 | Microsoft 文档"
description: "了解如何在 Azure Active Directory 和 Printix 之间配置单一登录。"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 400793331aa2d56358a83a51ce64c67f59bbf3b7
ms.openlocfilehash: 877b7b98757bed9fe9123c8fb5e17f891ef7cbda
ms.lasthandoff: 02/16/2017


---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a>教程：Azure Active Directory 与 Printix 的集成
本教材介绍如何将 Printix 与 Azure Active Directory (Azure AD) 集成。

将 Printix 与 Azure AD 集成具有以下优势：

* 可在 Azure AD 中控制谁有权访问 Printix
* 可以让用户使用其 Azure AD 帐户自动登录到 Printix（单一登录）
* 可以在一个中心位置（即 Azure 经典门户）管理帐户

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先决条件
若要配置 Azure AD 与 Printix 的集成，需备齐以下项目：

* Azure AD 订阅
* 启用 Printix 单一登录的订阅

> [!NOTE]
> 测试本教程中的步骤时，建议不要使用生产环境。
> 
> 

测试本教程中的步骤应遵循以下建议：

* 不应使用生产环境，除非有此必要。
* 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。

本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Printix
2. 配置并测试 Azure AD 单一登录

## <a name="adding-printix-from-the-gallery"></a>从库中添加 Printix
若要配置 Printix 与 Azure AD 的集成，需要从库中将 Printix 添加到托管 SaaS 应用列表。

**若要从库中添加 Printix，请执行以下步骤：**

1. 在 **Azure 经典门户**中，在左侧导航窗格上，单击“Active Directory”。
   
    ![Active Directory][1]

2. 从“目录”列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![应用程序][2]

4. 在页面底部单击“添加”。
   
    ![应用程序][3]

5. 在“要执行什么操作”对话框中，单击“从库中添加应用程序”。
   
    ![应用程序][4]

6. 在搜索框中，键入“Printix”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/tutorial_printix_01.png)

7. 在结果窗格中，选择“Printix”，然后单击“完成”添加该应用程序。

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置并测试 Azure AD 单一登录
本部分需根据名为“Britta Simon”的测试用户的情况，配置和测试 Printix 的 Azure AD 单一登录。

若要使用单一登录，Azure AD 需要了解与 Azure AD 中的用户相对应的 Printix 中的用户是谁。 换句话说，需要建立 Azure AD 用户与 Printix 中相关用户之间的关联关系。

将 Azure AD 中“用户名”的值指定为 Printix 中“用户名”的值，即可建立此关联关系。

若要使用 Printix 配置和测试 Azure AD 单一登录，需完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Printix 测试用户](#creating-a-printix-test-user)** - 目的是在 Printix 中有一个与 Azure AD 中的 Britta Simon 相对应的关联用户。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 能够使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录
在本部分中，在经典门户中启用 Azure AD 单一登录，并在 Printix 应用程序中配置单一登录。

**若要使用 Printix 配置 Azure AD 单一登录，请执行以下步骤：**

1. 在经典门户中的“Printix”应用程序集成页上，单击“配置单一登录”，打开“配置单一登录”对话框。
   
    ![配置单一登录][6] 

2. 在“你希望用户如何登录 Printix”页上，选择“Azure AD 单一登录”，然后单击“下一步”。
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_03.png) 

3. 在“配置应用设置”对话框页上，执行以下步骤：
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_04.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“回复 URL”文本框中，键入 `https://auth.printix.net/saml/SSO`。
   
    b. 单击“下一步”

4. 在“在 Printix 处配置单一登录”页上，执行以下步骤：
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_05.png)
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 单击“下载元数据”，然后在计算机上保存该文件。
   
    b. 单击“下一步”。

5. 以管理员身份登录到 Printix 租户。

6. 在顶部菜单中，单击右上角的图标，然后选择“身份验证”。
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

7. 在“安装”选项卡上，选择“启用 Azure/Office 365 身份验证”
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

8. 在“Azure”选项卡上，将联合元数据 URL 输入到“联合元数据文档”文本框中。 
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 通过“**support@printix.net**”将步骤 4 中下载的元数据 xml 文件以附件方式提供给 Printix 支持团队。 然后，他们会上载该 xml 文件，并为用户提供联合元数据 URL。

9. 单击“测试”按钮，如果测试成功，则单击“确定”按钮。
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 单击“测试”按钮后，将显示 Azure Active Directory 页。 这里的“测试成功”是指在成功输入 Azure 测试帐户的凭据以后，会弹出“设置测试成功”消息。此时请单击“确定”按钮。
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

10. 单击“身份验证”页上的“保存”按钮。

11. 在经典门户中，选择“单一登录配置确认”，然后单击“下一步”。
    
    ![Azure AD 单一登录][10]

12. 在“单一登录确认”页上，单击“完成”。  
    
    ![Azure AD 单一登录][11]

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
在本部分中，在经典门户中创建名为“Britta Simon”的测试用户。

![创建 Azure AD 用户][20]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 经典门户**中，在左侧导航窗格上，单击“Active Directory”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/create_aaduser_09.png) 

2. 在“目录”列表中，选择要启用目录集成的目录。

3. 若要显示用户列表，请在顶部菜单中，单击“用户”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. 若要打开“添加用户”对话框，请在底部工具栏中单击“添加用户”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

5. 在“告诉我们有关此用户的信息”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/create_aaduser_05.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 对于“用户类型”，选择“组织中的新用户”。
   
    b. 在“用户名”文本框中，键入“BrittaSimon”。
   
    c. 单击“下一步”。

6. 在“用户配置文件”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/create_aaduser_06.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“名字”文本框中，键入“Britta”。  
   
    b. 在“姓氏”文本框中，键入“Simon”。
   
    c. 在“显示名称”文本框中，键入“Britta Simon”。
   
    d.单击“下一步”。 在“角色”列表中，选择“用户”。
   
    e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，然后单击“确定”。 单击“下一步”。

7. 在“获取临时密码”对话框页上，单击“创建”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/create_aaduser_07.png) 

8. 在“获取临时密码”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-printix-tutorial/create_aaduser_08.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 写下“新密码”的值。
   
    b. 单击“完成”。   

### <a name="creating-an-printix-test-user"></a>创建 Printix 测试用户
此部分的目的是在 Printix 中创建名为 Britta Simon 的用户。 Printix 支持在默认情况下启用的实时预配。

本部分不存在任何操作项。 尝试访问 Printix 期间，如果该用户尚不存在，则将创建一个新用户。 [配置 Azure AD 单一登录](#configuring-azure-ad-single-single-sign-on)。

> [!NOTE]
> 如果需要手动创建用户，则需要联系 Printix 支持团队。
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户
本部分需授予 Britta Simon 访问 Printix 的权限，使之能够使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Printix，请执行以下步骤：**

1. 在经典门户中，若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![分配用户][201] 

2. 在应用程序列表中，选择“Printix”。
   
    ![配置单一登录](./media/active-directory-saas-printix-tutorial/tutorial_printix_50.png) 

3. 在顶部菜单中，单击“用户”。
   
    ![分配用户][203]

4. 在“用户”列表中，选择“Britta Simon”。

5. 在底部工具栏中，单击“分配”。
   
    ![分配用户][205]

### <a name="testing-single-sign-on"></a>测试单一登录
在本部分中，将使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Printix”磁贴时，用户就会自动登录到 Printix 应用程序。

## <a name="additional-resources"></a>其他资源
* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](active-directory-saas-tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-printix-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-printix-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-printix-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-printix-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-printix-tutorial/tutorial_general_205.png

