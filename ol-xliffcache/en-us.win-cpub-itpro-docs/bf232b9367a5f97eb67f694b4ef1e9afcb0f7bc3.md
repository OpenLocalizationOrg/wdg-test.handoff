---
title: Review GPO Settings
description: Review GPO Settings
author: jamiejdt
ms.assetid: bed956d0-082e-4fa9-bf1e-572d0d3d02ec
ms.pagetype: mdop
ms.mktglfcycl: manage
ms.sitesec: library
ms.prod: w10
translationtype: Human Translation
ms.sourcegitcommit: 2d1f98a24d9330d6b3bce488b2cac6ac11b5e4bf
ms.openlocfilehash: bf232b9367a5f97eb67f694b4ef1e9afcb0f7bc3

---


# Review GPO Settings


You can generate HTML-based and XML-based reports for reviewing settings within any version of a Group Policy Object (GPO).

A user account with the Reviewer, Editor, Approver, or AGPM Administrator (Full Control) role or necessary permissions in Advanced Group Policy Management (AGPM) is required to complete this procedure. Review the details in "Additional considerations" in this topic.

**To review settings in any version of a GPO**

1.  In the **Group Policy Management Console** tree, click **Change Control** in the forest and domain in which you want to manage GPOs.

2.  On the **Contents** tab in the details pane, click a tab to display GPOs.

3.  Double-click the GPO to display its history.

4.  Right-click the GPO version for which to review the settings, click **Settings**, and then click **HTML Report** or **XML Report** to display a summary of the GPO's settings.

### Additional considerations

-   By default, you must be a Reviewer, an Editor, an Approver, or an AGPM Administrator (Full Control) to perform this procedure. Specifically, you must have **List Contents** and **Read Settings** permissions for the GPO. Also, to display the list of GPOs, you must have **List Contents** permission for the domain.

### Additional references

-   [Performing Reviewer Tasks](performing-reviewer-tasks-agpm30ops.md)

 

 








<!--HONumber=Jun16_HO4-->


