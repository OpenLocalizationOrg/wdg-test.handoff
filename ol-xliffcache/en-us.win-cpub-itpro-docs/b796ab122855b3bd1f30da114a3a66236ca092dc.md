---
title: 4766(F) An attempt to add SID History to an account failed. (Windows 10)
description: Describes security event 4766(F) An attempt to add SID History to an account failed.
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: b796ab122855b3bd1f30da114a3a66236ca092dc

---

# 4766(F): An attempt to add SID History to an account failed.

**Applies to**
-   Windows 10
-   Windows Server 2016


This event generates when an attempt to add [SID History](https://msdn.microsoft.com/en-us/library/ms679833(v=vs.85).aspx) to an account failed.

See more information about SID History here: <https://technet.microsoft.com/en-us/library/cc779590(v=ws.10).aspx>.

There is no example of this event in this document.


            ***Subcategory:***            &nbsp;            [Audit User Account Management](audit-user-account-management.md)
          

***Event Schema:***

*An attempt to add SID History to an account failed.*

*Subject:*

> *Security ID:-*
>
> *Account Name:%5*
>
> *Account Domain:%6*
>
> *Logon ID:%7*

*Target Account:*

> *Security ID:%4*
>
> *Account Name:%2*
>
> *Account Domain:%3*

*Source Account:*

> *Account Name:%1*

*Additional Information:*

> *Privileges:%8*


            ***Required Server Roles:*** Active Directory domain controller.


            ***Minimum OS Version:*** Windows Server 2008.


            ***Event Versions:*** 0.

## Security Monitoring Recommendations

-   There is no recommendation for this event in this document.




<!--HONumber=Jun16_HO4-->


