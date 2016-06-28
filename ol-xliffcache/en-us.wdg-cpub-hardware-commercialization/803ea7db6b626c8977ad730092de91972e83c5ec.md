---
author: kpacquer
Description: 'We''ll create a provisioning package that contains some sample Wi-Fi settings.'
ms.assetid: d9a50f87-e8c0-48da-89e7-0cdd542ce053
MSHAttr: 'PreferredLib:/library'
title: 'Lab 1d: Add a provisioning package to an image'
---

# Lab 1d: Add a provisioning package to an image

\[This content has been tested on Windows 10 IoT Core Build 10586. Some of these procedures do not yet work on newer preview builds, including Windows 10, version 1607.\]

We'll create a provisioning package that contains some sample Wi-Fi settings. You can use provisioning package in Windows Imaging and Configuration Designer (ICD) to add apps, drivers, features, or to modify many common settings, such as IT device management and policy settings.

Note, to test Wi-Fi, your board will need Wi-Fi support. You can use a Wi-Fi adapter/dongle, or use a board like the Raspberry Pi 3 that has Wi-Fi built-in. 

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites


We'll start with the ProjectA image we created from [Lab 1a: Create a basic image](create-a-basic-image.md), and updated in [Lab 1b: Add an app to your image](deploy-your-app-with-a-standard-board.md) and [Lab 1c: Add a file and a registry setting to an image](add-a-registry-setting-to-an-image.md).

## <span id="Create_your_provisioning_package_in_Windows_ICD"></span><span id="create_your_provisioning_package_in_windows_icd"></span><span id="CREATE_YOUR_PROVISIONING_PACKAGE_IN_WINDOWS_ICD"></span>Create your provisioning package in Windows ICD


1.  Start **Windows Imaging and Configuration Designer**.

2.  Click **File &gt; New project**.

3.  For this example, use the name "**ProductAprov**" for the product name, accept the defaults, and click **Next**.

4.  Select **Provisioning package &gt; Windows 10 IoT Core**.

5.  At the **Import a provisioning package (optional)** page, click **Finish**.

6.  Add a sample setting:

    1.  Expand **Runtime settings &gt; Connectivity Profiles &gt; WLAN &gt; WLANSetting &gt; SSID**.

    2.  Type in the name of a Wi-Fi network name, for example, ContosoWiFi, and click **Add**.

    3.  Expand the **SSID > WLANXmlSettings > SecurityType** and choose a setting such as **Open**.

    4.  Expand the **SSID > WLANXmlSettings > AutoConnect** and choose a setting such as **TRUE**.

    5.  Optional: to add more than one WLAN network, go back to WLANSetting, and repeat the process.

7.  Optional: add other apps, drivers, and settings through the UI. To learn more, see [Configure customizations using Windows ICD](https://msdn.microsoft.com/library/windows/hardware/dn916109).

8.  Export the provisioning package. For example, click **Export &gt; Provisioning Package &gt; Next &gt; Next &gt; Build**. (To learn more, see [Export a provisioning package](https://msdn.microsoft.com/library/windows/hardware/dn916110). )

9.  At the **All done!** page, click the link to the **Output location**.

**Create a folder for the provisioning package in your test product**

1.  In File Explorer, create a new folder, C:\\IoT-ADK-AddonKit\\Products\\ProductA\\prov.

    This folder is structure is used by the script: Provisioning.Auto.pkg.xml file in Provisioning.Auto folder. No changes are required.

2.  Copy the .ppkg, .cat, and customizations.xml files into this folder.
    
    If necessary, rename the files to (your project name)prov.cat and (your project name)prov.ppkg, for example, ProductAprov.cat and ProductAprov.ppkg.
	
3.  Optional: update customizations.xml with any desired changes. See [Windows provisioning answer file](https://msdn.microsoft.com/library/windows/hardware/dn916153) for more info.

**Check the auto-provisioning scripts to make sure they'll include your new provisioning package (optional)**

1.  Review the package definition file: Provisioning.Auto.pkg.xml: C:\\IoT-ADK-AddonKit\\Common\\Packages\\Provisioning.Auto\\Provisioning.Auto.pkg.xml. No changes should required.

    ``` syntax
    <?xml version="1.0" encoding="utf-8"?>
     <Package xmlns="urn:Microsoft.WindowsPhone/PackageSchema.v8.00"
        Owner="$(OEMNAME)"
        OwnerType="OEM"
        ReleaseType="Production"
        Platform="Common"
        Component="Provisioning"
        SubComponent="Auto">
        <Components>
           <OSComponent>
           <Files>
            <!--
            Source : Provisioning is product specific, so copied from the Product directory     
            Destination : Auto provisioning folder which is C:\Windows\Provisioning\Packages
            -->
            <File Source="$(PRJDIR)\Products\$(PROD)\prov\$(PROD)Prov.ppkg"
                    DestinationDir="$(runtime.windows)\Provisioning\Packages" Name="ProvAuto.ppkg"/>
            </Files>
           </OSComponent>
        </Components>
     </Package>
        
    ```

2.  Make sure that the package definition file is referenced in the feature manifest. For example, check C:\\IoT-ADK-AddonKit\\Common\\Packages\\OEMCommonFM.xml:
    ```
          <PackageFile Path="%PKGBLD_DIR%" Name="%OEM_NAME%.Provisioning.Auto.cab">
            <FeatureIDs>
              <FeatureID>OEM_ProvAuto</FeatureID>
            </FeatureIDs>
          </PackageFile>    
          
        </OEM>
    ...
            <FeatureIDs>
              <FeatureID>OEM_ProvAuto</FeatureID>
              <FeatureID>OEM_ProvManual</FeatureID>        
            </FeatureIDs>
    ```

3.  Add the FeatureID to your test configuration file C:\\IoT-ADK-AddonKit\\Source-<arch>\\Products\\ProductA\\TestOEMInput.xml:
    ```
    <Feature>OEM_ProvAuto</Feature>
    ```

### <span id="Build_and_test_the_image"></span><span id="build_and_test_the_image"></span><span id="BUILD_AND_TEST_THE_IMAGE"></span>Build and test the image

**Build the image**

1.  From the IoT Core Shell, create the image:

    ``` syntax
    createimage ProductA Test
    ```

    This creates the product binaries at C:\\IoT-ADK-AddonKit\\Build\\&lt;arch&gt;\\ProductA\\Test\\Flash.FFU.

2.  Start **Windows IoT Core Dashboard** &gt; **Setup a new device** &gt; **Custom**, and browse to your image. Put the Micro SD card in the device, select it, accept the license terms, and click **Install**. This replaces the previous image with our new image.

3.  Put the card into the IoT device and start it up.

    After a short while, the device should start automatically, and you should see your app.

**Check to see if your provisioning settings were applied**

1.  Connect a network cable.

2.  On your technician PC, open Internet Explorer, and type in the address with an http:// prefix and :8080 suffix:

    ``` syntax
    http://100.100.0.100:8080
    ```

    This opens the [Windows Device Portal](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.md). From here, you can upload app packages, see what apps are installed, and switch between them.

3.  Use the default username (Administrator) and password (p@ssw0rd) to log on.

4.  Click **Networking.**

5.  Check the **Profiles** drop-down, you should see the Wi-Fi profile you created. 
    
	If the device is able to automatically connect to the WiFi network, then under **Available Networks**, you should see a checkmark next to the network you configured.

    If your network requires steps such as accepting license terms, the device may not auto-connect.

## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>Next steps

[Lab 1e: Add a driver to an image](add-a-driver-to-an-image.md) 
