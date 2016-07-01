---
title: How to Modify Client Configuration by Using PowerShell
description: How to Modify Client Configuration by Using PowerShell
author: jamiejdt
ms.assetid: 53ccb2cf-ef81-4310-a853-efcb395f006e
ms.pagetype: mdop, appcompat, virtualization
ms.mktglfcycl: deploy
ms.sitesec: library
ms.prod: w10
translationtype: Human Translation
ms.sourcegitcommit: 2d1f98a24d9330d6b3bce488b2cac6ac11b5e4bf
ms.openlocfilehash: 2c159217640df42c0de8cc3b4c1a8618e90b02d7

---


# How to Modify Client Configuration by Using PowerShell


Use the following procedure to configure the App-V 5.0 client configuration.

**To modify App-V 5.0 client configuration using PowerShell**

1.  To configure the client settings using PowerShell, use the **Set-AppvClientConfiguration** cmdlet.

2.  To modify the client configuration, open a PowerShell Command prompt and run the following cmdlet **Set-AppvClientConfiguration** with any required parameters. For example:

    `$config = Get-AppvClientConfiguration`

    `Set-AppcClientConfiguration $config`

    `Set-AppcClientConfiguration –Name1 MyConfig –Name2 “xyz”`

    
            **Got a suggestion for App-V**? Add or vote on suggestions [here](http://appv.uservoice.com/forums/280448-microsoft-application-virtualization). 
            **Got an App-V issu**e? Use the [App-V TechNet Forum](https://social.technet.microsoft.com/Forums/home?forum=mdopappv).

## Related topics


[Operations for App-V 5.0](operations-for-app-v-50.md)

 

 








<!--HONumber=Jun16_HO4-->


