---
description: How to install the Internet Explorer 11 update using your network
ms.assetid: 85f6429d-947a-4031-8f93-e26110a35828
author: eross-msft
ms.prod: ie11
ms.mktglfcycl: deploy
ms.sitesec: library
title: Install Internet Explorer 11 (IE11) using your network (Internet Explorer 11 for IT Pros)
translationtype: Human Translation
ms.sourcegitcommit: 0164c7a3150be29ce2e1077c4746dec0ab6463b6
ms.openlocfilehash: f0dc41cb409be5ce62b87dfff3183624364949fa

---

# Install Internet Explorer 11 (IE11) using your network
You can install Internet Explorer 11 (IE11) over your network by putting your custom IE11 installation package in a shared network folder and letting your employees run the Setup program on their own computers. You can create the network folder structure manually, or you can run Internet Explorer Administration Kit 11 (IEAK 11).

**Note**<br>If you support multiple architectures and operating systems, create a subfolder for each combination. If you support multiple languages, create a subfolder for each localized installation file.

 ![](images/wedge.gif) **To manually create the folder structure**

-   Copy your custom IE11 installation file into a folder on your network, making sure it's available to your employees.

 ![](images/wedge.gif) **To create the folder structure using IEAK 11**

-   Run the Internet Explorer Customization Wizard 11 in IEAK 11, using the **Full Installation Package** option.<p>
The wizard automatically puts your custom installation files in your `\<build_directory>\Flat` folder. Where the `<build_directory>` is the location of your other build files.

**Note**<br>Use the localized versions of the IE Customization Wizard 11 to create localized IE11 installation packages.

## Related topics
- [Internet Explorer Administration Kit 11 (IEAK 11) - Administration Guide for IT Pros](../ie11-ieak/index.md)
     

 

 






<!--HONumber=Jun16_HO4-->


