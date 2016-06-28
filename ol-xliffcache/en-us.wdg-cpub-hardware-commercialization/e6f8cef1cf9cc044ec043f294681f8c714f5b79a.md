---
author: kpacquer
Description: 'We''ll show you one of two ways to add a driver to the image.'
title: 'Lab 1e: Add a driver to an image'
---

# Lab 1e: Add a driver to an image

Now we'll add drivers to a Windows 10 IoT Core image. 

When you're using a pre-built Board Support Package (BSP), you'll want to know whether there's already similar drivers on the board. If not, you can usually just add the new driver. 

If there is, but that driver doesn't meet your needs, then you'll need to replace the driver from the BSP, which we'll cover in [Lab 2a](replace-a-driver-in-an-existing-bsp.md).

In our lab, we'll use the sample driver: [Hello, Blinky!](https://ms-iot.github.io/content/en-US/win10/samples/DriverLab.htm), package it up, and deploy it to the device.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites

-  Complete [Lab 1a: Create a basic image](create-a-basic-image.md).

## <span id="Create_your_test_files"></span><span id="create_your_test_files"></span><span id="CREATE_YOUR_TEST_FILES"></span>Create your test files

-  Complete the exercises in [Installing The Sample Driver](https://ms-iot.github.io/content/en-US/win10/samples/DriverLab.htm) to build the Hello, Blinky app. You'll create three files: ACPITABL.dat, gpiokmdfdemo.inf, and gpiokmdfdemo.sys, which you'll use to install the driver.

   You can also use your own IoT Core driver, so long as it doesn't conflict with the existing Board Support Package (BSP).

-  Copy each of the files: gpiokmdfdemo.sys, gpiokmdfdemo.inf, and ACPITABL.dat into a test folder, for example, C:\gpiokmdfdemo\.

## <span id="Build_a_package_for_your_driver"></span><span id="build_a_package_for_your_driver"></span><span id="BUILD_A_PACKAGE_FOR_YOUR_DRIVER"></span>Build a package for your driver

1.  Run **C:\\IoT-ADK-AddonKit\\Tools\\IoTCoreShell** as an administrator.

2.  Create the driver package using the .inf file as the base:

    ``` syntax
    newdrvpkg C:\gpiokmdfdemo\gpiokmdfdemo.inf Drivers.HelloBlinky
    ```

    The new folder at **C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Packages\\Drivers.HelloBlinky\\**.


**Verify that the sample files are in the package**

1.  Update the driver's package definition file, **C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Packages\\Drivers.HelloBlinky\\Drivers.HelloBlinky.pkg.xml**.

    The default package definition file includes sample XML that you can modify to add your own driver files.

    If necessary, update the value of File Source to point to your .SYS file, and the ACPITABL.dat file. (You don't need to add the .INF file.)  Add the DestinationDir of "$(runtime.drivers)".
    
    ``` syntax
    <Package xmlns="urn:Microsoft.WindowsPhone/PackageSchema.v8.00" 
         Owner="$(OEMNAME)" OwnerType="OEM" ReleaseType="Production" 
         Platform="arm" Component="Drivers" SubComponent="HelloBlinky"> 
      <Components> 
        <Driver InfSource="gpiokmdfdemo.inf"> 
          <Reference Source="gpiokmdfdemo.sys" /> 
          <Files> 
            <File Source="gpiokmdfdemo.sys"  
                  DestinationDir="$(runtime.drivers)"  
                  Name="gpiokmdfdemo.sys" /> 
            <File Source="ACPITABL.dat"  
                  DestinationDir="$(runtime.drivers)"  
                  Name="ACPITABL.dat" /> 
          </Files> 
        </Driver> 
      </Components> 
    </Package> 
    ```

2.  From the IoT Core Shell, build the package.

    ``` syntax
    createpkg Drivers.HelloBlinky
    ```

    The package is built, appearing as **C:\\IoT-ADK-AddonKit\\Build\\&lt;arch&gt;\\pkgs\\&lt;your OEM name&gt;.Drivers.HelloBlinky.cab**.

    
## <span id="Update_your_feature_manifest"></span><span id="update_your_feature_manifest"></span><span id="UPDATE_YOUR_FEATURE_MANIFEST"></span>Update your feature manifest


**Add your driver package to the feature manifest**

1.  Open the architecture-specific feature manifest file, **C:\\IoT-ADK-AddonKit\\Source-<arch>\\Packages\\OEMFM.xml**
2.  Create a new PackageFile section in the XML, with your package file listed, and give it a new FeatureID, such as "OEM\_DriverHelloBlinky".

    ``` syntax      
          <PackageFile Path="%PKGBLD_DIR%" Name="%OEM_NAME%.Drivers.HelloBlinky.cab">
            <FeatureIDs>
              <FeatureID>OEM_DriverHelloBlinky</FeatureID>
            </FeatureIDs>
          </PackageFile>
    ```

    You'll now be able to add your driver to your product by adding a reference to this feature manifest.


## <span id="Update_the_project_s_configuration_files"></span><span id="update_the_project_s_configuration_files"></span><span id="UPDATE_THE_PROJECT_S_CONFIGURATION_FILES"></span>Update the project's configuration files

1.  Open your product's test configuration file: **C:\\IoT-ADK-AddonKit\\Source-arm\\Products\\ProductA\\TestOEMInput.xml**.
2.  Make sure your feature manifest, OEMFM.xml, is in the list of AdditionalFMs. Add it if it isn't there already there:

    ``` syntax
      <AdditionalFMs>
        <AdditionalFM>%AKROOT%\FMFiles\arm\IoTUAPNonProductionPartnerShareFM.xml</AdditionalFM>
        <AdditionalFM>%AKROOT%\FMFiles\arm\IoTUAPRPi2FM.xml</AdditionalFM>
        <AdditionalFM>%AKROOT%\FMFiles\arm\RPi2FM.xml</AdditionalFM>
        <AdditionalFM>%SRC_DIR%\Packages\OEMFM.xml</AdditionalFM>
        <AdditionalFM>%COMMON_DIR%\Packages\OEMCommonFM.xml</AdditionalFM>
      </AdditionalFMs>
    ```

3.  Add the FeatureID for your driver:

    ``` syntax
    <OEM> 
    <Feature>RPI2_DRIVERS</Feature> 
    <Feature>RPI2_DEVICE_TARGETINGINFO</Feature> 
    <Feature>PRODUCTION</Feature> 
    <Feature>OEM_AppxHelloWorld</Feature> 
    <Feature>OEM_FileAndRegKey</Feature> 
    <Feature>OEM_DriverHelloBlinky</Feature> 
    </OEM>
    ```

## <span id="Build_and_test_the_image"></span><span id="build_and_test_the_image"></span><span id="BUILD_AND_TEST_THE_IMAGE"></span>Build and test the image


**Build the image**

1.  From the IoT Core Shell, create the image:

    ``` syntax
    createimage ProductA Test
    ```

    This creates the product binaries at C:\\IoT-ADK-AddonKit\\Build\\&lt;arch&gt;\\ProductA\\Flash.FFU.

2.  Start **Windows IoT Core Dashboard** &gt; **Setup a new device** &gt; **Custom**, and browse to your image. Put the Micro SD card in the device, select it, accept the license terms, and click **Install**. This replaces the previous image with our new image.
3.  Put the card into the IoT device and start it up.

    After a short while, the device should start automatically, and you should see your app.

**Check to see if your driver works**

1.  Use the procedures in the [Hello, Blinky! lab](https://ms-iot.github.io/content/en-US/win10/samples/DriverLab3.htm) to test your driver.



## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>Next steps

[Lab 2a: Replace a driver in an existing board support package](replace-a-driver-in-an-existing-bsp.md)