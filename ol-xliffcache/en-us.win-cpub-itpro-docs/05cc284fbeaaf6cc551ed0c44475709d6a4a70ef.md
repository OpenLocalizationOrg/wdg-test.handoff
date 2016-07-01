---
title: Firewall GPOs (Windows 10)
description: Firewall GPOs
ms.assetid: 720645fb-a01f-491e-8d05-c9c6d5e28033
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
author: brianlic-msft
translationtype: Human Translation
ms.sourcegitcommit: 8863c8459e010a6b73e4d623e950bb9313751f36
ms.openlocfilehash: 05cc284fbeaaf6cc551ed0c44475709d6a4a70ef

---

# Firewall GPOs

**Applies to**
-   Windows 10
-   Windows Server 2016 Technical Preview

All the devices on Woodgrove Bank's network that run Windows are part of the isolated domain, except domain controllers. To configure firewall rules, the GPO described in this section is linked to the domain container in the Active Directory OU hierarchy, and then filtered by using security group filters and WMI filters.

The GPO created for the example Woodgrove Bank scenario include the following:

-   [GPO\_DOMISO\_Firewall](gpo-domiso-firewall.md)



<!--HONumber=Jun16_HO4-->


