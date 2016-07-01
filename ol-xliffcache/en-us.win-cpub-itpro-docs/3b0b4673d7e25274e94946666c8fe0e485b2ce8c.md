---
description: Enhanced Protected Mode problems with Internet Explorer
ms.assetid: 15890ad1-733d-4f7e-a318-10399b389f45
author: eross-msft
ms.prod: ie11
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
title: Enhanced Protected Mode problems with Internet Explorer (Internet Explorer 11 for IT Pros)
translationtype: Human Translation
ms.sourcegitcommit: 0164c7a3150be29ce2e1077c4746dec0ab6463b6
ms.openlocfilehash: 3b0b4673d7e25274e94946666c8fe0e485b2ce8c

---

# Enhanced Protected Mode problems with Internet Explorer
Enhanced Protected Mode further restricts Protected Mode to deny potential attackers access to sensitive or personal information. If this feature is turned on, users might start to see errors asking them to turn it off, like **This webpage wants to run "npctrl.dll. If you trust this site, you can disable Enhanced Protected Mode for this site to run the control**. If your users click the **Disable** box, Enhanced Protected Mode is turned off for only the single visit to that specific site. After the user leaves the site, Enhanced Protected Mode is automatically turned back on.

You can use your company’s Group Policy to turn Enhanced Protected Mode on or off for all users. For more information, see the [Group policy objects and Internet Explorer 11 (IE11)](group-policy-objects-and-ie11.md) information in this guide.

For more information about Enhanced Protected Mode, see the [Enhanced Protected Mode](http://go.microsoft.com/fwlink/p/?LinkId=267512) post on IEBlog, and both the [Understanding Enhanced Protected Mode](http://go.microsoft.com/fwlink/p/?LinkId=282662) and the [Enhanced Protected Mode and Local Files](http://go.microsoft.com/fwlink/p/?LinkId=282663) blog posts on IEInternals.

 

 






<!--HONumber=Jun16_HO4-->


