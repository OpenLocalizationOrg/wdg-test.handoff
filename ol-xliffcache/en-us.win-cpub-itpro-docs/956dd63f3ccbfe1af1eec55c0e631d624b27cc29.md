---
title: Open the Group Policy Management Console to Windows Firewall with Advanced Security (Windows 10)
description: Open the Group Policy Management Console to Windows Firewall with Advanced Security
ms.assetid: 28afab36-8768-4938-9ff2-9d6dab702e98
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
author: brianlic-msft
translationtype: Human Translation
ms.sourcegitcommit: ec65ca848bf7efadab2e50f96d2b50f4064313cc
ms.openlocfilehash: 956dd63f3ccbfe1af1eec55c0e631d624b27cc29

---

# Open the Group Policy Management Console to Windows Firewall with Advanced Security

**Applies to**
-   Windows 10
-   Windows Server 2016 Technical Preview

Most of the procedures in this guide instruct you to use Group Policy settings for Windows Firewall with Advanced Security.

To open a GPO to Windows Firewall with Advanced Security

1.  Open the Group Policy Management console.

2.  In the navigation pane, expand **Forest:***YourForestName*, expand **Domains**, expand *YourDomainName*, expand **Group Policy Objects**, right-click the GPO you want to modify, and then click **Edit**.

3.  In the navigation pane of the Group Policy Management Editor, navigate to **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Windows Firewall with Advanced Security** > **Windows Firewall with Advanced Security - LDAP://cn={***GUID***},cn=…**.



<!--HONumber=Jun16_HO4-->


