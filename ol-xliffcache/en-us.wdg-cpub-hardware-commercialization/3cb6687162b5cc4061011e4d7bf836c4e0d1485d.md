---
author: kpacquer
Description: 'Windows 10 IoT Core (IoT Core) is a version of Windows 10 that is optimized for smaller devices with or without a display. IoT Core uses the rich, extensible Universal Windows Platform (UWP) API for building great solutions.'
MSHAttr: 'PreferredLib:/library'
title: What's new in IoT Core manufacturing
---

# What's new in IoT Core manufacturing

## <span id="IoT_Core_AddOns"></span>IoT Core Add-Ons

**June 20, 2016**
*  New BSP tools added:
   -  New folder structure: BSPs are now stored in separate folders, \iot-adk-addonkit\Source-<arch>\BSP. Multiple projects can now more easily share the same BSP folder.
   -  Updated tool: newproduct: Now allows you to link to a custom BSP. By default, arm builds default to RPi2, x86 builds default to MBM.
   -  Updated lab: [Lab 2a: Replace a driver in an existing board support package](replace-a-driver-in-an-existing-bsp.md)

**June 9, 2016:** 
Several updates of [command-line tools](iot-core-adk-addons-command-line-options.md):
*  New tool: BuildImage.cmd. Similar to CreateImage.cmd, this tool can build multiple images at a time, which can be useful for automated testing.  

   To troubleshoot, see log files at \Build\<arch>\pkgs\logs.   

*  Updated: [Update apps on your IoT Core devices](..\service\updating-iot-core-apps.md). You can use the same procedures to build app packages and app update packages. For Windows 10, version 1607, you can also update your apps through the Windows Store. 

**May 18, 2016:** 
Several updates of [command-line tools](iot-core-adk-addons-command-line-options.md):
*  Added new tools: 
   -  NewAppxPkg: Creates and prepares working folders for creating app packages based on .appx files, certificates, and dependencies.
   
   -  NewDrvPkg: Creates and prepares working folders for creating driver packages based on .inf files.
   
   -  NewCommonPkg: Creates and prepares working folders for files, folders, registry keys, and other items that aren't architecture-specific.  
   
   -  Inf2Cab: Converts a .inf file to a .cab file.
   
   -  BuildPkg: Builds .cab packages using working folders.

*  Deprecated tools: 
   
   -  BuildAllPackages. Use BuildPkg instead.
   
   -  NewPkg. Use newappxpkg, newdrvpkg, and newcommonpkg instead.

*  Troubleshooting and logging: When building packages, you'll get a cleaner display just showing the packages that have been processed, and any errors. To troubleshoot, you can now see the full log files at \Build\<arch>\pkgs\logs.

*  Updated: CreatePkg now supports adding just the Component.Subcomponent names. Example:

    ``` syntax
    createpkg Registry.SystemSettings
    ```

*  Manufacturing labs: Updated labs to use new NewAppPkg, NewDriverPkg, NewCommonPkg tools.