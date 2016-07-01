---
title: Script rules in AppLocker (Windows 10)
description: This topic describes the file formats and available default rules for the script rule collection.
ms.assetid: fee24ca4-935a-4c5e-8a92-8cf1d134d35f
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
author: brianlic-msft
translationtype: Human Translation
ms.sourcegitcommit: 8e6dba25e9dbe4f0c138a416b6de2fb4abc6f94e
ms.openlocfilehash: c1c4482d5b44f60b160413d496e96e5586334d2e

---

# Script rules in AppLocker

**Applies to**
-   Windows 10

This topic describes the file formats and available default rules for the script rule collection.

AppLocker defines script rules to include only the following file formats:
-   .ps1
-   .bat
-   .cmd
-   .vbs
-   .js

The following table lists the default rules that are available for the script rule collection.

| Purpose | Name | User | Rule condition type |
| - | - | - | - |
| Allows members of the local Administrators group to run all scripts| (Default Rule) All scripts| BUILTIN\Administrators | Path: *|
| Allow all users to run scripts in the Windows folder| (Default Rule) All scripts located in the Windows folder| Everyone | Path: %windir%\*| 
| Allow all users to run scripts in the Program Files folder| (Default Rule) All scripts located in the Program Files folder|Everyone | Path: %programfiles%\*| 
 
## Related topics

- [Understanding AppLocker default rules](understanding-applocker-default-rules.md)



<!--HONumber=Jun16_HO4-->


