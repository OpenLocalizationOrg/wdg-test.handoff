---
author: kpacquer
Description: 'We''ll take our test image and convert it to a retail build.'
MS-HAID: 'p\_iot\_core.build\_retail\_image'
MSHAttr: 'PreferredLib:/library'
title: 'Lab 1f: Build a retail image'
---

# Lab 1f: Build a retail image


\[This content has been tested on Windows 10 IoT Core Build 10586. Some of these procedures do not yet work on newer preview builds, including Windows 10, version 1607.\]

We''ll take our test image and convert it to a retail build. 

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites


We'll start with the ProjectA image we created from [Lab 1a: Create a basic image](create-a-basic-image.md).
You can include any of the updates from [Lab 1b: Add an app to your image](deploy-your-app-with-a-standard-board.md), [Lab 1c:  Add a file and a registry setting to an image](add-a-registry-setting-to-an-image.md), [Lab 1d: Add a provisioning package to an image](add-a-provisioning-package-to-an-image.md), and [Lab 1e: Add a driver to an image](add-a-driver-to-an-image.md).

## <span id="Add_your_app_to_the_retail_configuration_file"></span><span id="add_your_app_to_the_retail_configuration_file"></span><span id="ADD_YOUR_APP_TO_THE_RETAIL_CONFIGURATION_FILE"></span>Add your app to the retail configuration file


1.  Open your product's retail configuration file: **C:\\IoT-ADK-AddonKit\\Source-&lt;arch&gt;\\Products\\ProductA\\RetailOEMInput.xml**..

2.  Add your feature manifest, OEMFM.xml, into the list of AdditionalFMs. At the same time, add the feature manifest: OEMCommonFM.xml, which contains the OEM\_CustomCmd package that configures your app on the first boot:

    ``` syntax
    <AdditionalFMs>
        <AdditionalFM>%AKROOT%\FMFiles\arm\IoTUAPRPi2FM.xml</AdditionalFM>
        <AdditionalFM>%AKROOT%\FMFiles\arm\RPi2FM.xml</AdditionalFM>
        <AdditionalFM>%SRC_DIR%\Packages\OEMFM.xml</AdditionalFM>
        <AdditionalFM>%COMMON_DIR%\Packages\OEMCommonFM.xml</AdditionalFM>
      </AdditionalFMs>
    ```

3.  Add the FeatureIDs for the your app package, the  and the OEM\_CustomCmd package.

    ``` syntax
    <OEM> 
    <Feature>RPI2_DRIVERS</Feature> 
    <Feature>RPI2_DEVICE_TARGETINGINFO</Feature> 
    <Feature>OEM_AppxHelloWorld</Feature> 
    <Feature>OEM_FileAndRegKey</Feature> 
    <Feature>OEM_CustomCmd</Feature> 
    <Feature>OEM_ProvAuto</Feature>
	<Feature>OEM_DriverHelloBlinky</Feature> 
    </OEM>
    ```
    
    OEM_CustomCmd is required to trigger the app installation.
    
    OEM_ProvAuto is required to pull in the provisioning package.
	
	OEM_FileAndRegKey, OEM_AppxHelloWorld, and OEM_DriverHelloBlinky were sample packages added in previous labs.

4.  To support your apps, add the FeatureIDs for IOT_UAP_OOBE and IOT_APP_TOOLKIT from the main list of Microsoft features:

    ```
      <Features>
        <Microsoft> 
         <Feature>IOT_EFIESP</Feature> 
         <Feature>IOT_DMAP_DRIVER</Feature> 
         <Feature>IOT_UAP_OOBE</Feature> 
         <Feature>IOT_APP_TOOLKIT</Feature> 
        </Microsoft>
       </Features>
    ```

### <span id="Build_and_create_the_image"></span><span id="build_and_create_the_image"></span><span id="BUILD_AND_CREATE_THE_IMAGE"></span>Build and create the image

**Build the image**

1.  From the IoT Core Shell, create the image:

    ``` syntax
    createimage ProductA Retail
    ```

    This creates the product binaries at C:\\IoT-ADK-AddonKit\\Build\\&lt;arch&gt;\\ProductA\\Retail\\Flash.FFU.

2.  Start **Windows IoT Core Dashboard** &gt; **Setup a new device** &gt; **Custom**, and browse to your image. Put the Micro SD card in the device, select it, accept the license terms, and click **Install**. This replaces the previous image with our new image.
3.  Put the card into the IoT device and start it up.

    After a short while, the device should start automatically, and you should see your app.

**Check to see if everything is working**

With retail builds, you won't be able to log into the device using the SSH clients or by using the web interface. However, any files and reg keys that your app relies on should still work.




## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>Next steps
**Congratulations!**
That's it for the first lab. 

From here, you can continue on to:
-  [Lab 2a: Replace a driver in an existing board support package](replace-a-driver-in-an-existing-bsp.md)



 