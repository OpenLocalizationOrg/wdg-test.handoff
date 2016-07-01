---
title: How to Change the Size of the FileSystem Cache
description: How to Change the Size of the FileSystem Cache
author: jamiejdt
ms.assetid: 6ed17ba3-293b-4482-b3fa-31e5f606dad6
ms.pagetype: mdop, appcompat, virtualization
ms.mktglfcycl: deploy
ms.sitesec: library
ms.prod: w8
translationtype: Human Translation
ms.sourcegitcommit: 2d1f98a24d9330d6b3bce488b2cac6ac11b5e4bf
ms.openlocfilehash: 92c576e90b04abbff885b5aa281f03494d2cd6e1

---


# How to Change the Size of the FileSystem Cache


You can change the size of the FileSystem cache by using the command line. This action requires a complete reset of the cache, and it requires administrative rights.

**To change the size of the FileSystem cache**

1.  Set the following registry value to 0 (zero):

    HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SoftGrid\\4.5\\Client\\AppFS\\State

2.  Set the following registry value to the maximum cache size, in MB, that is necessary to hold the packages—for example, 8192 MB:

    HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SoftGrid\\4.5\\Client\\AppFS\\FileSize

3.  Restart the computer.

## Related topics


[How to Configure the App-V Client Registry Settings by Using the Command Line](how-to-configure-the-app-v-client-registry-settings-by-using-the-command-line.md)

 

 








<!--HONumber=Jun16_HO4-->


