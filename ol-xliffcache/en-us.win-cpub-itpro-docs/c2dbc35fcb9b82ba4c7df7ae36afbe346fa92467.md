---
title: Using the Sdbinst.exe Command-Line Tool (Windows 10)
description: You must deploy your customized database (.sdb) files to other computers in your organization before your compatibility fixes, compatibility modes, and AppHelp messages are applied.
ms.assetid: c1945425-3f8d-4de8-9d2d-59f801f07034
ms.prod: w10
ms.mktglfcycl: plan
ms.pagetype: appcompat
ms.sitesec: library
author: TrudyHa
translationtype: Human Translation
ms.sourcegitcommit: ae8220499efdd2bafe460fcae530d62dc13a2634
ms.openlocfilehash: c2dbc35fcb9b82ba4c7df7ae36afbe346fa92467

---

# Using the Sdbinst.exe Command-Line Tool


**Applies to**

-   Windows 10
-   Windows 8.1
-   Windows 8
-   Windows 7
-   Windows Server 2012
-   Windows Server 2008 R2

You must deploy your customized database (.sdb) files to other computers in your organization before your compatibility fixes, compatibility modes, and AppHelp messages are applied. You can deploy your customized database files in several ways, including by using a logon script, by using Group Policy, or by performing file copy operations.

After you deploy and store the customized databases on each of your local computers, you must register the database files. Until you register the database files, the operating system is unable to identify the available compatibility fixes when starting an application.

## Command-Line Options for Deploying Customized Database Files


The command-line options use the following conventions.

Sdbinst.exe \[-q\]\[-u filepath\]\[-g *GUID*\]\[-n *"name"*\]\[-?\]

The following table describes the available command-line options.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Option</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>-q</p></td>
<td align="left"><p>Performs a silent installation with no visible window, status, or warning information. Fatal errors appear only in Event Viewer (Eventvwr.exe).</p>
<p>For example,</p>
<p><code>sdbinst.exe -q</code></p></td>
</tr>
<tr class="even">
<td align="left"><p>-u <em>filepath</em></p></td>
<td align="left"><p>Performs an uninstallation of the specified database.</p>
<p>For example,</p>
<p><code>sdbinst.exe -u C:\example.sdb</code></p></td>
</tr>
<tr class="odd">
<td align="left"><p>-g <em>GUID</em></p></td>
<td align="left"><p>Specifies the customized database to uninstall by a globally unique identifier (GUID).</p>
<p>For example,</p>
<p><code>sdbinst.exe -g 6586cd8f-edc9-4ea8-ad94-afabea7f62e3</code></p></td>
</tr>
<tr class="even">
<td align="left"><p>-n <em>&quot;name&quot;</em></p></td>
<td align="left"><p>Specifies the customized database to uninstall by file name.</p>
<p>For example,</p>
<p><code>sdbinst.exe -n &quot;My_Database&quot;</code></p></td>
</tr>
<tr class="odd">
<td align="left"><p>-?</p></td>
<td align="left"><p>Displays the Help for the Sdbinst.exe tool.</p>
<p>For example,</p>
<p><code>sdbinst.exe -?</code></p></td>
</tr>
</tbody>
</table>

 

## Related topics


[Compatibility Administrator User's Guide](compatibility-administrator-users-guide.md)

 

 








<!--HONumber=Jun16_HO4-->


