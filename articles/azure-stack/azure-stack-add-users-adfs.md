---
title: Add users for Azure Stack ADFS | Microsoft Docs
description: Learn how to add users for ADFS deployments of Azure Stack
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 5ff45235cffead8c665d5c8610edca3c787ac267
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="add-users-in-the-azure-stack-development-kit"></a>Add users in the Azure Stack Development Kit

To add additional users to the Development Kit deployment, you must add them to the Azure Stack Development Kit directory using Microsoft Management Console from the Azure Stack host computer.
1.  On the Azure Stack host computer, open Microsoft Management Console.
2.  Click **File > Add or remove snap-in**.
3.  Select **Active Directory Users and Computers** > **AzureStack.local** > **Users**.
4.  Click **Action** > **New** > **User**.
5.  In the New Object – User window, provide and confirm a password
6.  Click **Next** to finalize the values and click Finish to create the user.



