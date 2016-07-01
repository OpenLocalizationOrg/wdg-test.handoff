---
title: App-V Package WMI Class
description: App-V Package WMI Class
author: jamiejdt
ms.assetid: 0fc26c3b-9706-4804-be2d-645771dc33ae
ms.pagetype: mdop, appcompat, virtualization
ms.mktglfcycl: deploy
ms.sitesec: library
ms.prod: w8
translationtype: Human Translation
ms.sourcegitcommit: 2d1f98a24d9330d6b3bce488b2cac6ac11b5e4bf
ms.openlocfilehash: abd3e4cbc7673b8bbb69d91ec2ebd0473bfb4e9c

---


# App-V Package WMI Class


In the Application Virtualization (App-V) Client, the **Package** class is a Windows Management Instrumentation (WMI) class that represents all the virtual packages on the client. The virtual packages can contain many virtual applications.

## Syntax


``` syntax
class Package
{
      string Name;
      string Version;
      string PackageGUID;
      string SftPath;
      uint64 TotalSize;
      uint64 CachedSize;
      uint64 LaunchSize;
      uint64 CachedLaunchSize;
      boolean InUse;
      boolean Locked;
      uint16 CachedPercentage;
      string VersionGUID;
 };
```

## Properties


<a href="" id="name"></a>**Name**  
Data type: **String**

Access type: Read-only

Qualifiers: None

The user-friendly name of the virtual package.

<a href="" id="version"></a>**Version**  
Data type: **String**

Access type: Read-only

Qualifiers: None

The version of the virtual package.

<a href="" id="packageguid"></a>**PackageGUID**  
Data type: **String**

Access type: Read-only

Qualifiers: Key

The GUID identifier of the package configuration and source files.

<a href="" id="sftpath"></a>**SftPath**  
Data type: **String**

Access type: Read-only

Qualifiers: None

The file path of the SFT file.

<a href="" id="totalsize"></a>**TotalSize**  
Data type: **UInt64**

Access type: Read-only

Qualifiers: None

The total size of the virtual package, in kilobytes.

<a href="" id="cachedsize"></a>**CachedSize**  
Data type: **UInt64**

Access type: Read-only

Qualifiers: None

The total size of the cache for the virtual package, in kilobytes.

<a href="" id="launchsize"></a>**LaunchSize**  
Data type: **UInt64**

Access type: Read-only

Qualifiers: None

The total size of the virtual package’s primary feature block, in kilobytes.

<a href="" id="cachedlaunchsize"></a>**CachedLaunchSize**  
Data type: **UInt64**

Access type: Read-only

Qualifiers: None

Total size of the virtual package’s primary feature block that has been cached, in kilobytes.

<a href="" id="inuse"></a>**InUse**  
Data type: **Boolean**

Access type: Read-only

Qualifiers: None


            **true** if any virtual application in the virtual package is running; otherwise **false**.

<a href="" id="locked"></a>**Locked**  
Data type: **Boolean**

Access type: Read-only

Qualifiers: None


            **true** if the virtual package is locked; otherwise **false**.

<a href="" id="cachedpercentage"></a>**CachedPercentage**  
Data type: **UInt16**

Access type: Read-only

Qualifiers: None

The percentage of the cache files. Based on the following formula: CachedSize / TotalSize × 100.

<a href="" id="versionguid"></a>**VersionGUID**  
Data type: **String**

Access type: Read-only

Qualifiers: None

The GUID identifier of the package version.

 

 








<!--HONumber=Jun16_HO4-->


