---
title: Encryption Zone GPOs (Windows 10)
description: Encryption Zone GPOs
ms.assetid: eeb973dd-83a5-4381-9af9-65c43c98c29b
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
author: brianlic-msft
translationtype: Human Translation
ms.sourcegitcommit: 8863c8459e010a6b73e4d623e950bb9313751f36
ms.openlocfilehash: fdb1f159e718b5835c0cac866647d2ea904c84f8

---

# Encryption Zone GPOs

**Applies to**
-   Windows 10
-   Windows Server 2016 Technical Preview

Handle encryption zones in a similar manner to the boundary zones. A device is added to an encryption zone by adding the device account to the encryption zone group. Woodgrove Bank has a single service that must be protected, and the devices that are running that service are added to the group CG\_DOMISO\_Encryption. This group is granted Read and Apply Group Policy permissions in on the GPO described in this section.

The GPO is only for server versions of Windows. Client devices are not expected to participate in the encryption zone. If the need for one occurs, either create a new GPO for that version of Windows, or expand the WMI filter attached to one of the existing encryption zone GPOs to make it apply to the client version of Windows.

-   [GPO\_DOMISO\_Encryption](gpo-domiso-encryption.md)



<!--HONumber=Jun16_HO4-->


