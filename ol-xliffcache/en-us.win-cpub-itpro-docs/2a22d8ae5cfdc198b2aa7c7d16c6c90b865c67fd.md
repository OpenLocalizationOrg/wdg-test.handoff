---
title: 5062(S) A kernel-mode cryptographic self-test was performed. (Windows 10)
description: Describes security event 5062(S) A kernel-mode cryptographic self-test was performed.
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: 2a22d8ae5cfdc198b2aa7c7d16c6c90b865c67fd

---

# 5062(S): A kernel-mode cryptographic self-test was performed.

**Applies to**
-   Windows 10
-   Windows Server 2016


This event occurs rarely, and in some situations may be difficult to reproduce.


            ***Subcategory:***            &nbsp;            [Audit System Integrity](audit-system-integrity.md)
          

***Event Schema:***

*A kernel-mode cryptographic self test was performed.*

*Module:%1*

*Return Code:%2*


            ***Required Server Roles:*** None.


            ***Minimum OS Version:*** Windows Server 2008, Windows Vista.


            ***Event Versions:*** 0.

## Security Monitoring Recommendations

-   Typically this event is required for detailed monitoring of CNG-related actions with cryptographic keys. If you need to monitor or troubleshoot actions related to specific cryptographic keys and operations, review this event to see if it provides the information you need.




<!--HONumber=Jun16_HO4-->


