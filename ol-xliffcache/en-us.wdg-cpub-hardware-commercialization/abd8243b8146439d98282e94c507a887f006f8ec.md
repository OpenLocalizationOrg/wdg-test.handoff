---
title: Stack Tags
description: In the Windows® Performance Analyzer (WPA), stack tags is a feature that lets you create labels (tags) to help you better identify which parts of the call stack(s) are affected.
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 07FACEE4-E906-4267-BF48-5C3D590F67C3
ms.prod: W10
ms.mktglfcycl: operate
ms.sitesec: msdn
---

# Stack Tags


In the Windows® Performance Analyzer (WPA), stack tags is a feature that lets you create labels (tags) to help you better identify which parts of the call stack(s) are affected.

## Understanding differences between stack tags and stack frame tags


You can think of **stack (frame tags)** and **stack** tags as two views of the same data available in the **Stack** column. You can configure a stack column to be viewed as a stack tag or stack column (frame tag) in the View Editor.

A call stack consists of a list of frames. If a call stack is in the form of A -&gt; B -&gt; C, then there are three frames: A, B, and C. Stack columns (frame tags) map each and every call stack frame to a tag or defaults to module!method if no tag is present.

For example, call stack A -&gt; B -&gt; C-&gt; D, in **Stack (Frame Tags)** view can become A -&gt; FrameTagB -&gt; FrameTagC -&gt; D. Each of the frame tags can have a hierarchy based on the hierarchy of definition of the tags in the \*.stacktags file (for example, FrameTagB's actual value can be "HTML\\Script\\OM").

A stack tag summarizes an entire call stack by using a single tag name. For example, the bottom most mapped frame tag is typically made the stack tag unless there is priority specified for tags. Using the same A -&gt; B -&gt; C -&gt; D example, where frame tag view is A -&gt; FrameTagB -&gt; FrameTagC -&gt; D, the stack tag view is just: FrameTagC.

Besides normal Tag for exactly matching module and method, you can also define HintTag with HintOperator as Callee or Caller. For example, a HintTag with HintOperator as Callee is defined for B. The call stack A -&gt; B -&gt; C -&gt; D in **Stack (FrameTags)** view can become A -&gt; FrameTagB -&gt; ModuleOfC -&gt; D and its StackTag view is FrameTagB -&gt; ModuleOfC. The module of C is dynamically created as a new stack tag. Explicitly setting the *OnlyShowModule* attribute of HintTag as false would make C as a new stack tag rather than ModuleOfC. *OnlyShowModule* attribute is true by default. The typical use case is to automatically attribute RPC server functions. Their direct caller function is rpcrt4.dll!Invoke\_epilog1\_start. You can define a HintTag for this common caller function to achieve this.

## Adding stack tags to the Stack Tags Definition file


To add a stack tag definition to the Stack Tags Definition file, do the following:

1.  In the menu, choose **Trace**, then select **Trace Properties**. The **Trace Properties** tab opens.

2.  In the Stack Tags Definition area, click **Add** to the desired location.

3.  Navigate to the area that contains the stack tags file, select it, and then click **Open**.

## Removing a stack tag from the stack tags definition file


To remove a stack tag definition from the Stack Tags Definition file, do the following:

1.  In the menu, choose **Trace**, then select **Trace Properties**. The **Trace Properties** tab opens.

2.  In the Stack Tags Definition area, select the stack tag definitions you want to remove then click **Remove**.

    **Warning**  Make sure you want to remove the selected stack tag definition(s), as you will not have the option to cancel once you click **Remove**.

     

## Reloading the stack tags definition file


To reload a stack tag definition to the Stack Tags Definition file, do the following:

1.  In the menu, choose **Trace**, then select **Trace Properties**. The **Trace Properties** tab opens.

2.  In the Stack Tags Definition area, click **Reload**. You can load multiple stack tags by pressing and holding down the Shift key and left-clicking each stack tags definition.

## Troubleshooting your stack tags file


To investigate issues within your stack tags file in WPA, do the following:

-   In the menu, click **Window**, then select **Diagnostic Console**. The WPA display splits into two - with the **Graph Explorer** and **Analysis** in the top half of the screen and the **Diagnostic Console** on the bottom half of the screen.

    **Tip**  You can also access the Diagnostic Console in the lower left corner of WPA by clicking **Diagnostic Console**. Once open, you can also drag it out to a separate window or dock it at the top or side.

     

    The Diagnostic Console lists information about exceptions that occur during analysis workflow. You can diagnose symbol decoding issues from this console

## Related topics


[Introduction to the WPA User Interface](introduction-to-the-wpa-user-interface.md)

[Diagnostic Console](diagnostic-console.md)

 

 







