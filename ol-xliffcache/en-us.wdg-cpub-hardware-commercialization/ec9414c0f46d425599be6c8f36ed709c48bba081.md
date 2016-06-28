---
Description: 'Replacing a driver in an existing BSP in a Windows 10 IOT Core image.'
title: 'Lab 2a: Replace a driver in an existing board support package (BSP)'
---

# Lab 2a: Replace a driver in an existing board support package (BSP)

If the driver in your pre-built Board Support Package (BSP) doesn't support what you need it to do, you can replace it.

Note, when you modify the BSP, you become the owner for this new, modified BSP. If the original BSP hardware manufacturer provides any updates to the board, you'll need to choose whether to pass the updates on to your own boards.

In our lab, we'll again use the sample GPIO driver: [Hello, Blinky!](https://ms-iot.github.io/content/en-US/win10/samples/DriverLab.htm). We'll also remove the existing GPIO driver from the device.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites

-  Complete [Lab 1e: Add a driver to an image](add-a-driver-to-an-image.md).

## <span id="Create_a_new_BSP_working_folder"></span><span id="create_a_new_bsp_working_folder"></span><span id="CREATE_A_NEW_BSP_FILE"></span>Create a new BSP working folder

1.  Create a duplicate copy of the BSP working folder that you'd like to modify.

	For example, copy C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\BSP\\Rpi2 to C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\BSP\\CustomRpi2.

2.  Remove/comment out any driver in the BSP feature manifest file that no longer apply.

   For example, update: **C:\\IoT-ADK-AddonKit\\Source-arm\\Products\\ProductB\\Blinky_RPi2FM.xml**.

    ``` syntax
    <!---
    <PackageFile Path="$(mspackageroot)\Retail\$(cputype)\$(buildtype)" Name="RASPBERRYPI.RPi2.GPIO.cab">
        <FeatureIDs>
          <FeatureID>RPI2_DRIVERS</FeatureID>
        </FeatureIDs>
    </PackageFile>
        -->
    ```  

3.  Add new drivers. 

    Optional: Use the same FeatureID as the rest of the files in the BSP (example, RPI2_DRIVERS) gets more easily picked up whenever you use this new BSP.  
    
    ``` syntax
          <PackageFile Path="%PKGBLD_DIR%" Name="%OEM_NAME%.Drivers.HelloBlinky.cab">
            <FeatureIDs>
              <FeatureID>RPI2_DRIVERS</FeatureID>
            </FeatureIDs>
          </PackageFile>
    ```  

4.  Comment out the Device Info file. This prevents your device from receiving updates from the original BSP manufacturer that could wipe out your changes.

    ``` syntax
    <!---
     <DeviceSpecificPackages>
     <PackageFile Device="RPi2" Path="$(mspackageroot)\Retail\$(cputype)\$(buildtype)" Name="RASPBERRYPI.RPi2.DeviceInfo.cab"/> 
     </DeviceSpecificPackages>
     -->
    ```
	
## <span id="Create_a_new_product_folder"></span><span id="create_a_new_product_and_folder"></span><span id="CREATE_A_NEW_PRODUCT_FOLDER"></span>Create a new product folder

1.  Create a new working product folder, adding your BSP name to the end.

    ``` syntax
    newproduct ProductB CustomRpi2
    ```

    This creates the folder: C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Products\\ProductB, which is linked to the new custom BSP.

## <span id="Update_the_project_s_configuration_files"></span><span id="update_the_project_s_configuration_files"></span><span id="UPDATE_THE_PROJECT_S_CONFIGURATION_FILES"></span>Update the project's configuration files

1.  Open your product's test configuration file: **C:\\IoT-ADK-AddonKit\\Source-arm\\Products\\ProductB\\TestOEMInput.xml**.

2.  Add FeatureIDs:
	  -  Add the FeatureIDs: IOT_DISABLE_UMCI and IOT_ENABLE_TESTSIGNING to enable test binaries and packages to work.
	  
	  -  Optional: add the FeatureID for the other apps and test packages: OEM_AppxHelloWorld, OEM_CustomCmd, OEM_FileAndRegKey, that you created in Lab 1.

    ``` syntax
    <OEM> 
    <Feature>RPI2_DRIVERS</Feature> 
    <Feature>RPI2_DEVICE_TARGETINGINFO</Feature> 
    <Feature>PRODUCTION</Feature> 
    <Feature>IOT_DISABLE_UMCI</Feature> 
    <Feature>IOT_ENABLE_TESTSIGNING</Feature> 
    <Feature>OEM_CustomCmd</Feature> 
    <Feature>OEM_AppxHelloWorld</Feature> 
    <Feature>OEM_FileAndRegKey</Feature> 
    </OEM>
    ```

## <span id="Build_and_test_the_image"></span><span id="build_and_test_the_image"></span><span id="BUILD_AND_TEST_THE_IMAGE"></span>Build and test the image

**Build the image**

1.  From the IoT Core Shell, create the image:

    ``` syntax
    createimage ProductB Test
    ```

    This creates the product binaries at C:\\IoT-ADK-AddonKit\\Build\\&lt;arch&gt;\\ProductB\\Flash.FFU.

2.  Start **Windows IoT Core Dashboard** &gt; **Setup a new device** &gt; **Custom**, and browse to your image. 

    Put the Micro SD card in the device, select it, accept the license terms, and click **Install**. This replaces the previous image with our new image.

3.  Put the card into the IoT device and start it up.

    After a short while, the device should start automatically, and you should see your app.

**Check to see if your driver works**

1.  Use the [testing procedures in the Hello, Blinky! lab](https://ms-iot.github.io/content/en-US/win10/samples/DriverLab3.htm) to test your driver.

<!--
## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>Next steps
Congratulations, you've completed Lab 2. 

[Lab 2b: Add a provisioning package to an image](add-a-provisioning-package-to-an-image.md)
-->
