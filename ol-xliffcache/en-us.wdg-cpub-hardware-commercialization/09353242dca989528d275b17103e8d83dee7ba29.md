---
author: kpacquer
Description: 'We''re now going to take an app (like the sample Hello, World! app), and package it up so that it can be serviced after it reaches your customers.'
ms.assetid: a801d768-0397-4f85-b68f-bd85ddcc3f1f
MSHAttr: 'PreferredLib:/library'
title: 'Lab 1b: Add an app to your image'
---

# Lab 1b: Add an app to your image

\[This content has been tested on Windows 10 IoT Core Build 10586. Some of these procedures do not yet work on newer preview builds, including Windows 10 Anniversary SDK Preview Build 14295.\]

We're now going to take an app (like the sample [Hello, World!](http://go.microsoft.com/fwlink/?LinkID=532945) app), package it up, and create a new image you can load onto new devices. 

**Note**  As we go through this manufacturing guide, ProjectA will start to resemble the SampleA image that's in C:\\IoT-ADK-AddonKit\\Source-arm\\Products\\SampleA.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites

We'll use the ProjectA image we created from [Lab 1a: Create a basic image](create-a-basic-image.md).

## <span id="Create_and_test_an_Windows_app"></span><span id="create_and_test_an_windows_app"></span><span id="CREATE_AND_TEST_AN_WINDOWS_APP"></span>Create and test an Windows app
You can skip these steps if you've already created and tested your app.

1.  Create an app. This can be any app designed for IoT Core, saved as an Appx Package. For our example, we use the [Hello, World](https://developer.microsoft.com/en-us/windows/iot/win10/samples/HelloWorld.htm) app.

2.  In Visual Studio, to save the Hello, World app as an Appx package, click **Project > Store > Create App Packages** > **Next**. 

3.  Select: 
    - **Output location: C:\HelloWorld** (or any other path that doesn't include spaces.)
    
    - **Generate app bundle: Never**
    
    Then click **Create**.

    Visual Studio creates the Appx file into C:\HelloWorld\HelloWorld_1.0.0.0_Debug_Test 

4.  Optional: (Test the app)[test-the-app.md]. Note, you may have already tested the app as part of building the project. 


## <span id="Package_the_app"></span><span id="package_the_app"></span><span id="PACKAGE_THE_APP"></span>Package the app

**Create a package for an app**

1.  Open **C:\\IoT-ADK-AddonKit\\Tools\\IoTCoreShell** as an administrator.


2.  Create a working folder for the app, for example:

    ``` syntax
    newAppxPkg "C:\Users\<UserName>\Documents\Visual Studio 2015\Projects\HelloWorld\AppPackages\HelloWorld_1.0.0.0_ARM_Debug_Test\HelloWorld_1.0.0.0_ARM_Debug.appx" Appx.HelloWorld
    ```

    This creates a new working folder at C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Packages\\Appx.HelloWorld that includes files that you'll use to help build the package.

	Troubleshooting: If you get the message: "The system cannot find the file specified", it may be because the package has no defined dependencies.
	
3.  From the IoT Core Shell, build the package.

    ``` syntax
    buildpkg Appx.HelloWorld
    ```

    The package is built, appearing as **C:\\IoT-ADK-AddonKit\\Build\\&lt;arch&gt;\\pkgs\\&lt;your OEM name&gt;.Appx.HelloWorld.cab**.

    Watch for any build errors, and correct them - often these are a result of filenames in the package definition file being different from the actual filenames.

## <span id="Update_the_feature_manifest"></span><span id="update_the_feature_manifest"></span><span id="UPDATE_THE_FEATURE_MANIFEST"></span>Update the feature manifest


**Add your app package to the feature manifest**

1.  Open your feature manifest file, **C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Packages\\OEMFM.xml**

2.  Create a new PackageFile section in the XML, with your package file listed, and give it a new FeatureID, such as "Appx\_HelloWorld".

    ``` syntax
    <?xml version="1.0" encoding="utf-8"?>
    <FeatureManifest xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/embedded/2004/10/ImageUpdate">
      <BasePackages/>
      <Features>
        <OEM>
          <!-- Feature definitions below -->
          <PackageFile Path="%PKGBLD_DIR%" Name="%OEM_NAME%.Appx.Main.cab">
            <FeatureIDs>
              <FeatureID>OEM_AppxMain</FeatureID>
            </FeatureIDs>
          </PackageFile>
        <PackageFile Path="%PKGBLD_DIR%" Name="%OEM_NAME%.Appx.HelloWorld.cab">
            <FeatureIDs>
              <FeatureID>OEM_AppxHelloWorld</FeatureID>
            </FeatureIDs>
          </PackageFile>
        </OEM>
        <OEMFeatureGroups/>
      </Features>
    </FeatureManifest>
    ```

    You'll now be able to add your app to any of your products by adding a reference to this feature manifest and Feature ID.

## <span id="Update_the_project_s_configuration_files"></span><span id="update_the_project_s_configuration_files"></span><span id="UPDATE_THE_PROJECT_S_CONFIGURATION_FILES"></span>Update the project's configuration files


**Replace your product's default app with your own**

1.  Open your product's test configuration file: **C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Products\\ProductA\\TestOEMInput.xml**.

2.  Make sure both your feature manifest, OEMFM.xml, and the feature manifest: OEMCommonFM.xml, are both listed in the AdditionalFMs section.

    ``` syntax
    <AdditionalFMs>
       <!-- Including BSP feature manifest -->
       <AdditionalFM>%BSPSRC_DIR%\RPi2\Packages\RPi2FM.xml</AdditionalFM>
       <!-- Including OEM feature manifest -->
       <AdditionalFM>%COMMON_DIR%\Packages\OEMCommonFM.xml</AdditionalFM>
       <AdditionalFM>%SRC_DIR%\Packages\OEMFM.xml</AdditionalFM>
       <!-- Including the test features -->
       <AdditionalFM>%AKROOT%\FMFiles\arm\IoTUAPNonProductionPartnerShareFM.xml</AdditionalFM>
    </AdditionalFMs>
    ```

3.  Add the FeatureIDs for both your app package, as well as the **OEM\_CustomCmd** package, which is used to launch your app. Remove the FeatureID for the OEM_AppxMain.

    ``` syntax
   <OEM>
      <!-- Include BSP Features -->
          <Feature>RPI2_DRIVERS</Feature>
      <!-- Include OEM features -->
      <Feature>OEM_AppxHelloWorld</Feature>
      <Feature>OEM_CustomCmd</Feature>
      <Feature>OEM_ProvAuto</Feature>
   </OEM>
    ```

4.  Remove the FeatureID for the IoT Core default app (IOT\_BERTHA) and the IOT_ALLJOYN_APP either by using &lt;!-- ... --&gt; to comment it out, or delete it from the list:

    ``` syntax
     <Microsoft>
       <Feature>IOT_EFIESP</Feature>
       <Feature>IOT_DMAP_DRIVER</Feature>
       <Feature>IOT_CP210x_MAKERDRIVER</Feature>
       <Feature>IOT_FTSER2K_MAKERDRIVER</Feature>
       <!-- Following two required for Appx Installation -->
       <Feature>IOT_UAP_OOBE</Feature>
       <Feature>IOT_APP_TOOLKIT</Feature>
       <!-- for Connectivity -->
       <Feature>IOT_WEBB_EXTN</Feature>
       <Feature>IOT_POWERSHELL</Feature>
       <Feature>IOT_NETCMD</Feature>
       <Feature>IOT_SSH</Feature>
       <Feature>IOT_SIREP</Feature>
       <!-- Enabling Test images -->
       <Feature>IOT_DISABLE_UMCI</Feature>
       <Feature>IOT_ENABLE_TESTSIGNING</Feature>
       <Feature>IOT_TOOLKIT</Feature>
       <!-- Debug Features -->
       <Feature>IOT_KDSERIAL_SETTINGS</Feature>
       <Feature>IOT_UMDFDBG_SETTINGS</Feature>
       <Feature>IOT_WDTF</Feature>
       <Feature>IOT_CRT140</Feature>
       <Feature>IOT_DIRECTX_TOOLS</Feature>
       <Feature>PRODUCTION_CORE</Feature>
       <!-- 
		  Sample Apps, remove this when you introduce OEM Apps
          <Feature>IOT_BERTHA</Feature>
		  <Feature>IOT_ALLJOYN_APP</Feature>
	   -->
       </Microsoft>

    ```

**Set the app to automatically install and set itself as the default app**

1.  Open **C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Products\\ProductA\\OEMCustomization.cmd**

2.  Recommended: Change the device's default username and password.

3.  Remove the first set of REM statements from the code block starting with "REM if exist..", this allows the AppInstall.cmd command to automatically install your app.

    ``` syntax
    @echo off
    REM OEM Customization Script file
    REM This script if included in the image, is called everytime the system boots.

    REM Enable Administrator User
    net user Administrator p@ssw0rd /active:yes

    if exist C:\AppInstall\AppInstall.cmd (
    REM Enable Application Installation for onetime only, after this the files are deleted.
    call C:\AppInstall\AppInstall.cmd > %temp%\AppInstallLog.txt
    if %errorlevel%== 0 (
    REM Cleanup Application Installation Files. Change dir to root so that the dirs can be deleted
    cd \
    rmdir /S /Q C:\AppInstall
    )
    )
    ```

## <span id="Build_and_test_the_image"></span><span id="build_and_test_the_image"></span><span id="BUILD_AND_TEST_THE_IMAGE"></span>Build and test the image


****

1.  From the IoT Core Shell, create the image:

    ``` syntax
    buildimage ProductA Test
    ```

    This creates the product binaries at C:\\IoT-ADK-AddonKit\\Build\\&lt;arch&gt;\\ProductA\\Flash.FFU.

	Troubleshooting: Check the logfiles at C:\\iot-adk-addonkit\\Build\\&lt;arch&gt;\\HelloWorld_Test.log, and search for "fatal error".
	
2.  Install the image onto the Micro SD card.
    -  Start **Windows IoT Core Dashboard**
    -  **Setup a new device** &gt; **Custom**, click Browse, and select your image. 
	-  Put the Micro SD card in the device, select it, accept the license terms, and click **Install**. 
	This replaces the previous image with your new image.

3.  Put the card into the IoT device and start it up.

    After a short while, the device should start automatically, and you should see your app.

## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>Next steps

[Lab 1c: Add a file and a registry setting to an image](add-a-registry-setting-to-an-image.md)

 ## <span id="Related_topics"></span><span id="related_topics"></span><span id="RELATED_TOPICS"></span>Related topics

[Update apps on your IoT Core devices](updating-iot-core-apps.md)