---
description: "How to create your folder structure on the computer that you’ll use to build your custom browser package."
ms.assetid: e0d05a4c-099f-4f79-a069-4aa1c28a1080
author: eross-msft
ms.prod: ie11
ms.mktglfcycl: plan
ms.sitesec: library
title: Create the build computer folder structure using IEAK 11  (Internet Explorer Administration Kit 11 for IT Pros)
translationtype: Human Translation
ms.sourcegitcommit: 48c519535edfc52c5fb29b4e3da49d4057ed6354
ms.openlocfilehash: 58afeb3e3c835ccbe49b3178be0b947f46a3fb13

---

# Create the build computer folder structure using IEAK 11
Create your build environment on the computer that you’ll use to build your custom browser package. Your license agreement determines your folder structure and which version of Internet Explorer Administration Kit 11 (IEAK 11) you’ll use: **Internal** or **External**.

|Name             |Version               |Description                                              |
|-----------------|----------------------|---------------------------------------------------------|
|`\<root_folder>` |Internal and External |The main, placeholder folder used for all files built by IEAK or that you referenced in your custom package.|
|`\<root_folder>\Dist` |Internal only |Destination directory for your files. You’ll only need this folder if you’re creating your browser package on a network drive. |


<!--HONumber=Jun16_HO4-->


