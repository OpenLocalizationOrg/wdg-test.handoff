---
title: 4780(S) The ACL was set on accounts which are members of administrators groups. (Windows 10)
description: Describes security event 4780(S) The ACL was set on accounts which are members of administrators groups.
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: 0df42696c89ac74f7c939fe9091480d6b9e6db81

---

# 4780(S): The ACL was set on accounts which are members of administrators groups.

**Applies to**
-   Windows 10
-   Windows Server 2016


Every hour, the domain controller that holds the primary domain controller (PDC) Flexible Single Master Operation (FSMO) role compares the ACL on all security principal accounts (users, groups, and machine accounts) present for its domain in Active Directory and that are in administrative or security-sensitive groups and which have AdminCount attribute = 1 against the ACL on the [AdminSDHolder](https://technet.microsoft.com/en-us/magazine/2009.09.sdadminholder.aspx) object. If the ACL on the principal account differs from the ACL on the AdminSDHolder object, then the ACL on the principal account is reset to match the ACL on the AdminSDHolder object and this event is generated.

For some reason, this event doesn’t generate on some OS versions.


            ***Subcategory:***            &nbsp;            [Audit User Account Management](audit-user-account-management.md)
          

***Event Schema:***

*The ACL was set on accounts which are members of administrators groups.*

*Subject:*

> *Security ID:%4*
>
> *Account Name:%5*
>
> *Account Domain:%6*
>
> *Logon ID:%7*

*Target Account:*

> *Security ID:%3*
>
> *Account Name:%1*
>
> *Account Domain:%2*

*Additional Information:*

> *Privileges:%8*


            ***Required Server Roles:*** Active Directory domain controller.


            ***Minimum OS Version:*** Windows Server 2008.


            ***Event Versions:*** 0.

## Security Monitoring Recommendations

-   Monitor for this event and investigate why the object’s ACL was changed.




<!--HONumber=Jun16_HO4-->


