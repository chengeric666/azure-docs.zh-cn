---
title: Subscribe to an offer and then provision a VM in Azure Stack (tenant) | Microsoft Docs
description: As a tenant, learn how to subscribe to an offer and then provision a VM in Azure Stack.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/1/2017
ms.author: erikje
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: e662baf863082b0d99430a79e5bd0546cc4b8bd3
ms.lasthandoff: 03/24/2017


---
# <a name="subscribe-to-an-offer"></a>Subscribe to an offer
Now that you've [created an offer](azure-stack-create-offer.md), test that your tenants can create a subscription.

1. On the Azure Stack POC computer, log in to `https://publicportal.local.azurestack.external` as [a tenant](azure-stack-connect-azure-stack.md) and click **Get a Subscription**.

   ![](media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. In the **Display Name** field, type a name for your subscription, click **Offer**, click one of the offers in the **Choose an offer** blade, and then click **Create**.

   ![](media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. To view the subscription you created, click **More services**, click **Subscriptions**, then click your new subscription.  

After you subscribe to an offer, refresh the portal to see which services are part of the new subscription.

# <a name="subscribe-to-an-add-on-plan"></a>Subscribe to an add-on plan
If the offer has an add-on plan, tenants can add them to their subscription at any time.  

1. In the tenant portal, select More services > Subscriptions .

2. Click on the subscription > Add Plan button, and select the add-on plan.



## <a name="next-steps"></a>Next steps
[Provision a virtual machine](azure-stack-provision-vm.md)

