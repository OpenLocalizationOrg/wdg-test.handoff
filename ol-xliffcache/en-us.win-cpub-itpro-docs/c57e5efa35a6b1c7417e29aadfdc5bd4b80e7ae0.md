---
description: Learn how to use the Resultant Set of Policy (RSoP) snap-in to view your policy settings.
ms.assetid: 0f21b320-e879-4a06-8589-aae6fc264666
author: eross-msft
ms.prod: ie11
ms.mktglfcycl: manage
ms.sitesec: library
title: Use the RSoP snap-in to review policy settings (Internet Explorer Administration Kit 11 for IT Pros)
translationtype: Human Translation
ms.sourcegitcommit: 48c519535edfc52c5fb29b4e3da49d4057ed6354
ms.openlocfilehash: c57e5efa35a6b1c7417e29aadfdc5bd4b80e7ae0

---

# Using the Resultant Set of Policy (RSoP) snap-in to review policy settings
After you’ve deployed your custom Internet Explorer package to your employees, you can use the Resultant Set of Policy (RSoP) snap-in to view your created policy settings. The RSoP snap-in is a two-step process. First, you run the RSoP wizard to determine what information should be viewed. Second, you open the specific items in the console window to view the settings. For complete instructions about how to use RSoP, see [Resultant Set of Policy](http://go.microsoft.com/fwlink/p/?LinkId=259479).

![](images/wedge.gif) **To add the RSoP snap-in**

1.  On the **Start** screen, type *MMC*.<p>
The Microsoft Management Console opens.

2.  Click **File**, and then click **Add/Remove Snap-in**.

3.  In the **Available snap-ins** window, go down to the **Resultant Set of Policy** snap-in option, click **Add**, and then click **OK**.<p>
You’re now ready to use the RSoP snap-in from the console.

![](images/wedge.gif) **To use the RSoP snap-in**

1.  Right-click **Resultant Set of Policy** and then click **Generate RSoP Data**.<p>
You’ll only need to go through the resulting RSoP Wizard first time you run the snap-in.

2.  Click **Next** on the **Welcome** screen.

3.  Under **Computer Configuration**, click **Administrative Templates**, click **Windows Components**, click **IE**, and then click the feature you want to review the policy settings for.

 

 








<!--HONumber=Jun16_HO4-->


