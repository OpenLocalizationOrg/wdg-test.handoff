---
title: 5148(F) The Windows Filtering Platform has detected a DoS attack and entered a defensive mode; packets associated with this attack will be discarded. (Windows 10)
description: Describes security event 5148(F) The Windows Filtering Platform has detected a DoS attack and entered a defensive mode; packets associated with this attack will be discarded.
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: e14bf0850f3d9dbe4a32f421bece1b8e5667d960

---

# 5148(F): The Windows Filtering Platform has detected a DoS attack and entered a defensive mode; packets associated with this attack will be discarded.

**Applies to**
-   Windows 10
-   Windows Server 2016


In most circumstances, this event occurs very rarely. It is designed to be generated when an ICPM DoS attack starts or was detected.

There is no example of this event in this document.


            ***Subcategory:***            &nbsp;            [Audit Other Object Access Events](audit-other-object-access-events.md)
          

***Event Schema:***

*The Windows Filtering Platform has detected a DoS attack and entered a defensive mode; packets associated with this attack will be discarded.*

*Network Information:*

> *Type:%1*


            ***Required Server Roles:*** None.


            ***Minimum OS Version:*** Windows Server 2008 R2, Windows 7.


            ***Event Versions:*** 0.

## Security Monitoring Recommendations

-   This event can be a sign of ICMP DoS attack or, among other things, hardware or network device related problems. In both cases, we recommend triggering an alert and investigating the reason the event was generated.




<!--HONumber=Jun16_HO4-->


