---
title: Control an Uncontrolled GPO
description: Control an Uncontrolled GPO
author: jamiejdt
ms.assetid: dc81545c-8da5-4b6f-b266-f01a82e27c6b
ms.pagetype: mdop
ms.mktglfcycl: manage
ms.sitesec: library
ms.prod: w10
translationtype: Human Translation
ms.sourcegitcommit: 2d1f98a24d9330d6b3bce488b2cac6ac11b5e4bf
ms.openlocfilehash: 61b2be71a4ddb68f622b9ec070536d38ade27c79

---


# Control an Uncontrolled GPO


To provide change control for a Group Policy Object (GPO), you must first control the GPO.

A user account with the Approver or AGPM Administrator (Full Control) role or necessary permissions in Advanced Group Policy Management (AGPM) is required to complete this procedure. Review the details in "Additional considerations" in this topic.

**To control an uncontrolled GPO**

1.  In the **Group Policy Management Console** tree, click **Change Control** in the forest and domain in which you want to manage GPOs.

2.  On the **Contents** tab in the details pane, click the **Uncontrolled** tab to display the uncontrolled GPOs.

3.  Right-click the GPO to be controlled with AGPM, and then click **Control**.

4.  Type a comment to be displayed in the history of the GPO, and then click **OK**.

5.  When the **Progress** window indicates that overall progress is complete, click **Close**. The GPO is removed from the list on the **Uncontrolled** tab and added to the **Controlled** tab.

### Additional considerations

-   By default, you must be an Approver or an AGPM Administrator (Full Control) to perform this procedure. Specifically, you must have **List Contents** and **Create GPO** permissions for the domain.

### Additional references

-   [Creating or Controlling a GPO](creating-or-controlling-a-gpo-agpm40-app.md)

 

 








<!--HONumber=Jun16_HO4-->


