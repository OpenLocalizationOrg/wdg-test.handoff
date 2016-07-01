---
title: Audit Directory Service Replication (Windows 10)
description: This topic for the IT professional describes the advanced security audit policy setting, Audit Directory Service Replication, which determines whether the operating system generates audit events when replication between two domain controllers begins and ends.
ms.assetid: b95d296c-7993-4e8d-8064-a8bbe284bd56
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
author: Mir0sh
translationtype: Human Translation
ms.sourcegitcommit: fa5ddfcf9d394c41415d2ed3e305b8068e8be0b8
ms.openlocfilehash: 7a17e1f1af4afe3b26e330ce263d0bf7f4dcb557

---

# Audit Directory Service Replication

**Applies to**
-   Windows 10
-   Windows Server 2016


Audit Directory Service Replication determines whether the operating system generates audit events when replication between two domain controllers begins and ends.


            **Event volume**: Medium on domain controllers.

| Computer Type     | General Success | General Failure | Stronger Success | Stronger Failure | Comments                                                                                                                                                                                                            |
|-------------------|-----------------|-----------------|------------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Domain Controller | No              | No              | IF               | IF               | IF - Events in this subcategory typically have an informational purpose and it is difficult to detect any malicious activity using these events. It’s mainly used for Active Directory replication troubleshooting. |
| Member Server     | No              | No              | No               | No               | This subcategory makes sense only on domain controllers.                                                                                                                                                            |
| Workstation       | No              | No              | No               | No               | This subcategory makes sense only on domain controllers.                                                                                                                                                            |

**Events List:**

-   
            [4932](event-4932.md)(S): Synchronization of a replica of an Active Directory naming context has begun.

-   
            [4933](event-4933.md)(S, F): Synchronization of a replica of an Active Directory naming context has ended.




<!--HONumber=Jun16_HO4-->


