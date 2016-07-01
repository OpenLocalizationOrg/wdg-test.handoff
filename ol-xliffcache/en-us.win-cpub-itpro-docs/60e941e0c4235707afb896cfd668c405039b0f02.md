---
title: 5056(S) A cryptographic self-test was performed. (Windows 10)
description: Describes security event 5056(S) A cryptographic self-test was performed.
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: 60e941e0c4235707afb896cfd668c405039b0f02

---

# 5056(S): A cryptographic self-test was performed.

**Applies to**
-   Windows 10
-   Windows Server 2016


This event generates in CNG Self-Test function. This is a Cryptographic Next Generation (CNG) function.

For more information about Cryptographic Next Generation (CNG) visit these pages:

-   <https://msdn.microsoft.com/en-us/library/windows/desktop/aa376214(v=vs.85).aspx>

-   <https://msdn.microsoft.com/en-us/library/windows/desktop/bb204775(v=vs.85).aspx>

-   <http://www.microsoft.com/en-us/download/details.aspx?id=1251>

-   <http://www.microsoft.com/en-us/download/details.aspx?id=30688>

This event is mainly used for Cryptographic Next Generation (CNG) troubleshooting.

There is no example of this event in this document.


            ***Subcategory:***            &nbsp;            [Audit System Integrity](audit-system-integrity.md)
          

***Event Schema:***

*A cryptographic self test was performed.*

*Subject:*

> *Security ID%1*
>
> *Account Name:%2*
>
> *Account Domain:%3*
>
> *Logon ID:%4*

*Module:%5*

*Return Code:%6*


            ***Required Server Roles:*** None.


            ***Minimum OS Version:*** Windows Server 2008, Windows Vista.


            ***Event Versions:*** 0.

## Security Monitoring Recommendations

-   Typically this event is required for detailed monitoring of CNG-related actions with cryptographic keys. If you need to monitor or troubleshoot actions related to specific cryptographic keys and operations, review this event to see if it provides the information you need.




<!--HONumber=Jun16_HO4-->


