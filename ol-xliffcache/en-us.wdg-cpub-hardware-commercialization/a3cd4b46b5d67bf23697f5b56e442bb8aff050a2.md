---
author: Justinha
Description: 'Push-button reset'
ms.assetid: f3d01e46-2288-42b3-a662-1f9346aeb50f
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: 'Push-button reset'
---

# Push-button reset

This topic is intended for original equipment manufacturers (OEMs) who want to add push-button reset features to their Windows 10 desktop computer manufacturing processes. If you are a user who wants to reset a computer that runs Windows 10, see [Recovery options in Windows 10](http://windows.microsoft.com/en-us/windows-10/windows-10-recovery-options).

Push-button reset is a recovery tool that repairs the OS while preserving data and important customizations. It reduces the need for custom recovery applications by providing users with more recovery options and the ability to fix their own PCs with confidence.

Push-button reset is included in Windows 10 for desktop editions (Home, Pro, Enterprise, and Education), and was introduced in Windows 8.

In Windows 10, the Push-button reset features have been updated to include the following improvements:

-   **Image-less recovery**: Push-button reset no longer require or support a separate recovery image on a local partition or on media. This significantly reduces the disk space needed to support the features, and makes recovery possible even on devices with limited storage capacity.
-   **Recovers to an updated state**: Push-button reset features now recover the Operating System (OS) and drivers (including device applets that are installed as part of INF-based driver packages) to an updated state. This reduces the amount of time users have to spend reinstalling the OS updates and drivers after performing a recovery.

The Push-button reset user experience continues to offer customization opportunities. Manufacturers can insert custom scripts, install applications or preserve additional data at available extensibility points.

The following Push-button reset features are available to users with Windows 10 PCs and devices:

-   **Refresh your PC** Fixes software problems by reinstalling the OS while preserving the user data, user accounts, and important settings. All other preinstalled customizations are restored to their factory state. In Windows 10, this feature no longer preserves user-acquired Windows apps.
-   **Reset your PC** Prepares the PC for recycling or for transfer of ownership by reinstalling the OS, removing all user accounts and contents (e.g. data, Classic Windows applications, and Universal Windows apps), and restoring preinstalled customizations to their factory state.
-   **Bare metal recovery** Restores the default or preconfigured partition layout on the system disk, and reinstalls the OS and preinstalled customizations from external media.

![](images/dep-winre-pbr.png)

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Content type</th>
<th align="left">References</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Overview</strong></p></td>
<td align="left"><p>[How push-button reset features work](how-push-button-reset-features-work.md) | [Recovery strategy for common customizations](recovery-strategy-for-common-customizations.md)</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Hard drive setup</strong></p></td>
<td align="left"><p>[Hard Drives and Partitions](hard-drives-and-partitions.md) | [UEFI/GPT-based hard drive partitions](configure-uefigpt-based-hard-drive-partitions.md) | [BIOS/MBR-based hard drive partitions](configure-biosmbr-based-hard-drive-partitions.md)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>Operations</strong></p></td>
<td align="left"><p>[Deploy push-button reset features using ScanState](deploy-push-button-reset-features.md) | [Bare metal reset/recovery: enable your users to create recovery media](bare-metal-resetrecovery-enable-your-users-to-create-media-and-to-recover-hard-drive-space.md) | [Bare metal reset/recovery: create recovery media while deploying new devices](create-media-to-run-push-button-reset-features-s14.md) | [Add a script to push-button reset features](add-a-script-to-push-button-reset-features.md)</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Configuration files</strong></p></td>
<td align="left"><p>[ResetConfig XML reference](resetconfig-xml-reference-s14.md)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>Technologies used by push-button reset</strong></p></td>
<td align="left"><p>[Windows Recovery Environment](windows-recovery-environment--windows-re--technical-reference.md) | [Windows PE (WinPE)](winpe-intro.md) | [ScanState](deploy-push-button-reset-features.md)</p></td>
</tr>
</tbody>
</table>

 

 

 





