---
title: 5029(F) The Windows Firewall Service failed to initialize the driver. The service will continue to enforce the current policy. (Windows 10)
description: Describes security event 5029(F) The Windows Firewall Service failed to initialize the driver. The service will continue to enforce the current policy.
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: cf2cd31142e85651cf846fb242235d5d955403e4

---

# 5029(F): The Windows Firewall Service failed to initialize the driver. The service will continue to enforce the current policy.

**Applies to**
-   Windows 10
-   Windows Server 2016


Windows logs an error if either the Windows Firewall service or its driver fails to start, or if they unexpectedly terminate. The error message indicates the cause of the service failure by including an error code in the text of the message.

There is no example of this event in this document.


            ***Subcategory:***            &nbsp;            [Audit Other System Events](audit-other-system-events.md)
          

***Event Schema:***

*The Windows Firewall service failed to initialize the driver. Windows Firewall will continue to enforce the current policy.*

*Error Code:%1*


            ***Required Server Roles:*** None.


            ***Minimum OS Version:*** Windows Server 2008, Windows Vista.


            ***Event Versions:*** 0.

## Security Monitoring Recommendations

-   This event can be a sign of software or operating system issues, or a sign of malicious activity that corrupted Windows Firewall Driver. We recommend monitoring this event and investigating the reason for the condition.




<!--HONumber=Jun16_HO4-->


