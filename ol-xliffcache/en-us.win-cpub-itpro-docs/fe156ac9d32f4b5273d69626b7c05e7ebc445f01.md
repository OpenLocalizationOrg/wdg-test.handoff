---
title: Audit Other Privilege Use Events (Windows 10)
description: This security policy setting is not used.
ms.assetid: 5f7f5b25-42a6-499f-8aa2-01ac79a2a63c
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: fe156ac9d32f4b5273d69626b7c05e7ebc445f01

---

# Audit Other Privilege Use Events

**Applies to**
-   Windows 10
-   Windows Server 2016


This auditing subcategory should not have any events in it, but for some reason Success auditing will enable generation of event 4985(S): The state of a transaction has changed.

| Computer Type     | General Success | General Failure | Stronger Success | Stronger Failure | Comments                                                              |
|-------------------|-----------------|-----------------|------------------|------------------|-----------------------------------------------------------------------|
| Domain Controller | No              | No              | No               | No               | This auditing subcategory doesn’t have any informative events inside. |
| Member Server     | No              | No              | No               | No               | This auditing subcategory doesn’t have any informative events inside. |
| Workstation       | No              | No              | No               | No               | This auditing subcategory doesn’t have any informative events inside. |

**Events List:**

-   
            [4985](event-4674.md)(S): The state of a transaction has changed.






<!--HONumber=Jun16_HO4-->


