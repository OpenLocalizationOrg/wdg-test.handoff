---
title: About Publishing
description: About Publishing
author: jamiejdt
ms.assetid: 295074d7-123f-4740-b938-e4a371ee72fd
ms.pagetype: mdop, appcompat, virtualization
ms.mktglfcycl: deploy
ms.sitesec: library
ms.prod: w8
translationtype: Human Translation
ms.sourcegitcommit: 2d1f98a24d9330d6b3bce488b2cac6ac11b5e4bf
ms.openlocfilehash: 1f960bf4ffbd9675a10b6f3bc8cf4075cd391dec

---


# About Publishing


You can centrally manage publishing applications to the Application Virtualization Client from the Application Virtualization Server Management Console. For example, you can assign access to applications and define when and how often the Application Virtualization Desktop Client and Client for Remote Desktop Services (formerly Terminal Services) need to refresh that information. You can set the clients to refresh this information on a set schedule or every time the user logs in to the client. Also, you can use the console's application publishing functionality to enable users to see which applications are published (or available) to the client.

**Note**  
Before the client can refresh the publishing information, the client must know about the Application Virtualization Management Server. You configure the client with the necessary information about the server when you install the client.

 

When a client contacts the server for application publishing information, the server provides the client with the list of applications that the user has permission to access and the location of the corresponding Open Software Descriptor (OSD) files. The server also provides the relevant information about icons, file type associations, and shortcuts.

## Related topics


[About Application Licensing](about-application-licensing.md)

[About Application Virtualization Applications](about-application-virtualization-applications.md)

 

 








<!--HONumber=Jun16_HO4-->


