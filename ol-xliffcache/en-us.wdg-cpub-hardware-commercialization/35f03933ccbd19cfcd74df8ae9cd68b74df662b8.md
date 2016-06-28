---
author: kpacquer
Description: 'OEM and enterprise customers can utilize a set of scripts and tools to deliver app updates for Windows 10 IoT Core (IoT Core) devices.'
ms.assetid: 010c4836-a8ad-4ab9-b5a8-45babcc8a3f3
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: Update apps on your IoT Core devices
---

# Update apps on your IoT Core devices

OEMs and enterprise customers can deliver app updates to Windows 10 IoT Core (IoT Core) devices by running ApplyUpdate.exe from the device, or by submitting updates to the Windows Store.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites

-  Create an app and add it to your device, as shown in [Lab 1b: Add an app to your image](deploy-your-app-with-a-standard-board.md).
 
-  For store updates, sign and submit the app package to the Windows Store.

## <span id="Create_an_update_package"></span><span id="create_an_update_package"></span><span id="CREATE_AN_UPDATE_PACKAGE"></span>Create an update package

Create a new version of your app, and package it into a .cab file using the same method in [Lab 1b: Add an app to your image](deploy-your-app-with-a-standard-board.md). 

It's OK to reuse the existing package names and even write over the old folder names and locations (though we recommend backing up your files first, in case you'd like to revert the change later.)

Use a higher version number each time. (Example, 1.0.0.0 --> 1.0.1.0.)

If you're adding the image to a retail build or to the Windows Store, sign the app package.

### <span id="Apply_update_directly_to_the_device"></span><span id="apply_update_directly_to_the_device"></span><span id="APPLY_UPDATE_DIRECTLY_TO_THE_DEVICE"></span>Apply update directly to the device

Log into the device and run ApplyUpdate.exe to trigger the update process.

1.  Copy the .cab file to the device. You can do this using an FTP tool such as [WinSCP](http://winscp.net), or by copying the file directly to the device's drive (such as a micro SD card.)

2.  On your technician PC, connect to your device using an SSH client, such as [PuTTY](http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe). For example, use the IP address and port 22 to connect to the device, then log in using the Administrator account and password. (To learn more, see [Using SSH to connect and configure a device running Windows IoT Core](http://ms-iot.github.io/content/en-US/win10/samples/SSH.md).)

3.  From the device command line, stage the update. You can repeat this step to stage multiple updates.
    ``` syntax
    ApplyUpdate.exe -stage <DownloadedPackageName.cab>
    ```

4.  Commit the changes. This command also triggers the Windows Update process, installing any applicable updates. 
    ``` syntax
    ApplyUpdate.exe -commit
    ```
	
	(To reject all updates, use: `ApplyUpdate.exe -clear` instead.)
	
	Troubleshooting:
	-  **Error: A certificate chain could not be built to a trusted root authority. (0x800B010A). Signature verification failed.** 
	   Possible cause: Adding test signed images to a retail image. Sign the image and try again.
	   
    -  **Error: 0x80188302.**
       Possible cause: Package version incorrect. Make sure the new version number is higher than the old version and try again. 
	
	-  **Error: Could not start update (0x8024A10F)**
       Possible cause: Windows Update is in progress. Try again after the current Windows Update is completed.
	   

### <span id="Apply_update_from_the_Windows_Store"></span><span id="apply_update_from_the_windows_store"></span><span id="APPLY_UPDATE_FROM_THE_WINDOWS_STORE"></span>Apply update from the Windows Store

Submit your updated signed package to the Windows Store. When your devices are connected to the internet, they'll periodically check for updates from the Windows Store, and install the updates automatically. 






 





