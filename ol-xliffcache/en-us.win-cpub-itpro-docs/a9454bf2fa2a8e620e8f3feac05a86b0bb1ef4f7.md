---
title: 4816(S) RPC detected an integrity violation while decrypting an incoming message. (Windows 10)
description: Describes security event 4816(S) RPC detected an integrity violation while decrypting an incoming message.
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: a9454bf2fa2a8e620e8f3feac05a86b0bb1ef4f7

---

# 4816(S): RPC detected an integrity violation while decrypting an incoming message.

**Applies to**
-   Windows 10
-   Windows Server 2016


This message generates if RPC detected an integrity violation while decrypting an incoming message.

There is no example of this event in this document.


            ***Subcategory:***            &nbsp;            [Audit System Integrity](audit-system-integrity.md)
          

***Event Schema:***

*RPC detected an integrity violation while decrypting an incoming message.*

*Peer Name: %1*

*Protocol Sequence: %2*

*Security Error: %3*


            ***Required Server Roles:*** None.


            ***Minimum OS Version:*** Windows Server 2008, Windows Vista.


            ***Event Versions:*** 0.

## Security Monitoring Recommendations

-   We recommend monitoring for this event, especially on high value assets or computers, because it can be a sign of a software or configuration issue, or a malicious action.




<!--HONumber=Jun16_HO4-->


