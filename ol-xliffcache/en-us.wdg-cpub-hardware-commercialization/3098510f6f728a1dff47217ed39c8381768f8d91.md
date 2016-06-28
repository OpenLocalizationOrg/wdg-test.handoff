---
author: kpacquer
Description: 'The Windows 10 IoT Core ADK Add-Ons include tools to help you customize and create new images for your devices with the apps, board support packages (BSPs), drivers, and Windows features that you choose, and a sample structure you can use to quickly create new images.'
ms.assetid: 26cfbad0-9528-4f89-a174-f198ece325d4
MSHAttr: 'PreferredLib:/library'
title: 'Windows ADK IoT Core Add-ons'
---

# Windows ADK IoT Core Add-ons



The [Windows 10 IoT Core ADK Add-Ons](http://go.microsoft.com/fwlink/?LinkId=735028) include OEM-specific tools to create images for your IoT Core devices with your apps, board support packages (BSPs), settings, drivers, and features.

The [IoT Core manufacturing guide](iot-core-adk-addons.md) walks you through building images with these tools.

## <span id="Folder_structure_"></span><span id="folder_structure_"></span><span id="FOLDER_STRUCTURE_"></span>Folder structure:


-   **IoTCoreShell**: Shortcut to launch the IoT Core Shell.
-   **Tools** (\\Tools)
-   **Common files** (\\Common), includes packages common to all architectures
-   **Source files** (\\Source-x86, \\Source-arm), includes packages specific to x86 or arm devices.
-   **Templates** (\\Templates), a directory with product template files.
-   **Build folder** (\\Build). You can use this folder to store your built binaries. It starts out empty.

## <span id="Tools"></span><span id="tools"></span><span id="TOOLS"></span>Tools


-   **createimage**: Combines product-specific packages into a new image
-   **createpkg**: Creates packages
-   **createprovpkg**: Creates provisioning packages
-   **createupdatepkgs**: Creates update packages
-   **newproduct**: Creates a new product directory based on the template file.
-   **newupdate**: Creates a new update directory based on the template file.
-   **setenv**: Sets environment variables for your architecture (x86 or arm).
-   **setversion**: Sets the version number of the packages you’re creating
<!--- -   **updateimage**: Updates an existing package with new updates -->

This folder also contains scripts used to generate packages/images.

For details, see [IoT Core Add-ons command-line options](iot-core-adk-addons-command-line-options.md).

## <span id="Common_files__packages_common_to_all_architectures_"></span><span id="common_files__packages_common_to_all_architectures_"></span><span id="COMMON_FILES__PACKAGES_COMMON_TO_ALL_ARCHITECTURES_"></span>Common files (packages common to all architectures)


-   **Custom.Cmd**: Package to include the oemcustomization cmd. This is product-specific and picks up the input file from product directory. This also makes an registry entry with the product name.
-   **Provisioning.Auto**: Package for auto provisioning. This is product specific and picks up the input ppkg file from the product directory. To learn more, see [Add a provisioning package to an image](add-a-provisioning-package-to-an-image.md).
-   **Provisioning.Manual**: Package for manual provisioning. This depends on Custom.Cmd for triggering the provisioning.
-   **Registry.Version**: Package containing registry settings with Product and version information.
-   **Registry.CrashSettings**: Package containing registry settings for the crash settings.
-   **Registry.TestSettings**: Sample package for OEM specific registry settings. To learn more, see [Add a registry setting to an image](add-a-registry-setting-to-an-image.md).
-   **OEMCommonFM.xml**: Feature manifest for OEM Common packages.

## <span id="Source_files__packages_that_are_specific_to_an_architecture_"></span><span id="source_files__packages_that_are_specific_to_an_architecture_"></span><span id="SOURCE_FILES__PACKAGES_THAT_ARE_SPECIFIC_TO_AN_ARCHITECTURE_"></span>Source files (packages that are specific to an architecture)


**\\Packages**: Architecture-specific sample packages:

-   **Appx.Main**: Sample package for Appx installation. This depends on Custom.Cmd to trigger the appx installation.
-   **Drivers.GPIO**: Sample package for a driver.
-   **OEMFM.xml:** Feature manifest for OEM packages

**\\Products**: Directory containing named product configurations. It starts with sample update folders, SampleA, SampleB, and SampleC, each of which contains some of the following samples files:

-   OEM input files for retail and test builds:
    -   **RetailOEMInput.xml**
    -   **TestOEMInput.xml**
-   Image customization files:
    -   A sample file that represents a customization, for example, **SampleAProv.cat**.
    -   **Customizations.xml**: A sample customization file created from Windows ICD.
    -   ***&lt;ProductName&gt;*Prov.ppkg**: A sample provisioning package created from the sample customization.xml files.
-   BSP feature manifest files:
    -   **OEM\_RPi2FM.xml**: used for Raspberry Pi 2
    -   **OEM\_MBMFM.xml**: used for Minnowboard MAX

**\\Updates**: Directory containing named update packages. It starts with two sample update folders, Update1 and Update2, each of which contains the following files:

-   **Versioninfo.txt**: The text file with the version number corresponding to this update.
-   **UpdateInput.xml**: Input file for updating the FFU, lists the packages that need to be updated in the existing FFU
-   Packages with updated contents (For example, Appx.Main with version 1.0.2.0).

## OK, let's build an image!
[Start with the IoT Core manufacturing guides](iot-core-manufacturing-guide.md)


## <span id="related_topics"></span>Related topics


[IoT Core feature list](iot-core-feature-list.md)


 

 



