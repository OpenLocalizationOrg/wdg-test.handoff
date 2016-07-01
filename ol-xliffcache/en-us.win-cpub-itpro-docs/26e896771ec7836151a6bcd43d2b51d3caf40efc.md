---
description: Use the \[ConnectionSettings\] .INS file setting to specify the network connection settings needed to install your custom package.
ms.assetid: 41410300-6ddd-43b2-b9e2-0108a2221355
author: eross-msft
ms.prod: ie11
ms.mktglfcycl: plan
ms.sitesec: library
title: Use the ConnectionSettings .INS file to review the network connections for install (Internet Explorer Administration Kit 11 for IT Pros)
translationtype: Human Translation
ms.sourcegitcommit: 48c519535edfc52c5fb29b4e3da49d4057ed6354
ms.openlocfilehash: 26e896771ec7836151a6bcd43d2b51d3caf40efc

---

# Use the ConnectionSettings .INS file to review the network connections for install
Info about the network connection settings used to install your custom package. This section creates a common configuration on all of your employee’s computers.

|Name       |Value                      |Description  |
|-----------|---------------------------|-------------|
|ConnectName0 |`<connection_name>` |Name for the connection. |
|ConnectName1 |`<connection_name>` |Secondary name for the connection. |
|DeleteConnectionSettings |<ul><li>
            **0.** Don’t remove the connection settings during installation.</li><li>
            **1.** Remove the connection settings during installation.<p>**Note**<br>This only appears for the **Internal** version of the IEAK 11.</li></ul> |Determines whether to remove the existing connection settings during installation of your custom package. |
|Option |<ul><li>
            **0.** Don’t let employees import connection settings.</li><li>
            **1.** Let employees import connection settings.</li></ul> |Determines whether an employee can import connection settings into the Internet Explorer Customization Wizard. |


<!--HONumber=Jun16_HO4-->


