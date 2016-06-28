---
author: kpacquer
Description: 'Here''s the features you can add to Windows 10 IoT Core (IoT Core) images.'
ms.assetid: cbae6949-ccfe-4015-a9b0-a269f6f30d5a
MSHAttr: 'PreferredLib:/library'
title: IoT Core feature list
---

# IoT Core feature list


Here's the features you can add to Windows 10 IoT Core (IoT Core) images.

Add features using the OEMInput XML file. To learn more, see the [IoT Core manufacturing guide](iot-core-manufacturing-guide.md).

## <span id="Retail_features_defined_by_Microsoft"></span><span id="retail_features_defined_by_microsoft"></span><span id="RETAIL_FEATURES_DEFINED_BY_MICROSOFT"></span>Retail features defined by Microsoft


The following table describes the Microsoft-defined features that can be used by OEMs in the Features element in the **OEMInput** file for **Retail** build.

When creating images for your device, determine which features are required and which features are optional. Some features are required only for certain boards (Raspberry Pi 2=**RPi2**, Qualcomm Dragonboard=**DB**, Minnowboard Max =**MBM**.

**Required features**

The following features must be included in the image, though they may be customized.

| Features              | Description                                                                                             |
|-----------------------|---------------------------------------------------------------------------------------------------------|
| **IOT\_APP\_TOOLKIT** | Adds tools required for Appx installation and management                                                |
| **IOT\_EFIESP**       | Boots the device using UEFI                                                                             |
| **IOT\_UAP\_OOBE**    | Includes the inbox OOBE app that is launched during the first boot and also during installation of apps |

 

**Optional features**

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Visual Studio debug tools</th>
<th align="left"></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><strong>IOT_CRT140</strong></td>
<td align="left">Adds CRT binaries</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_DIRECTX_TOOLS</strong></td>
<td align="left">Adds DirectX tools</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_UMDFDBG_SETTINGS</strong></td>
<td align="left">Includes user-mode debug settings</td>
</tr>
<tr class="even">
<td align="left">Developer tools</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_NETCMD</strong></td>
<td align="left">Adds the command-line tool: net.exe, used for configuring network connectivity</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_POWERSHELL</strong></td>
<td align="left">Adds PowerShell</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_SIREP</strong></td>
<td align="left">Enables SIREP service for TShell connectivity</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_SSH</strong></td>
<td align="left">Enables Secure Shell (SSH) connectivity</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_TOOLKIT</strong></td>
<td align="left">Includes developer tools such as Kernel Debug components, FTP, Network Diagnostics, basic device portal, and XPerf.
<p>This also relaxes the firewall rules and enables various ports.</p>
<div class="alert">
<strong>Note</strong>  We don't recommend including this feature in the Retail image.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_WEBB_EXTN</strong></td>
<td align="left">Enables IOTCore-specific extensions to the Windows Device Portal. The basic device portal is included in the IoT Toolkit.</td>
</tr>
<tr class="odd">
<td align="left">Apps</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_ALLJOYN_APP</strong></td>
<td align="left">Adds the AllJoyn application, used for Headless ZwaveAdapterAppx.
<p>Requires <strong>IOT_APP_TOOLKIT</strong>, for instalilng this appx on first boot.</p></td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_BERTHA</strong></td>
<td align="left">Adds a sample app: Bertha. This app provides basic version info and connectivity status.
<p>Requires <strong>IOT_APP_TOOLKIT</strong>, for instalilng this appx on first boot.</p></td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_UAP_DEFAULTAPP</strong></td>
<td align="left">Adds a sample app, referred to as Chucky. This is similar to Bertha app, providing information such as installed apps on the screen</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_CP210x_MAKERDRIVER</strong></td>
<td align="left">Adds the SiliconLabs USB to Serial driver</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_FTSER2K_MAKERDRIVER</strong></td>
<td align="left">Adds the FTDI USB-to-Serial driver</td>
</tr>
<tr class="odd">
<td align="left">Speech data</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_SPEECHDATA_DE_DE</strong></td>
<td align="left">Adds speech data for German</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_SPEECHDATA_EN_GB</strong></td>
<td align="left">Adds speech data for UK English</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_SPEECHDATA_EN_US</strong></td>
<td align="left">Adds speech data for US English</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_SPEECHDATA_ES_ES</strong></td>
<td align="left">Adds speech data for Spanish</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_SPEECHDATA_FR_FR</strong></td>
<td align="left">Adds speech data for French</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_SPEECHDATA_IT_IT</strong></td>
<td align="left">Adds speech data for Italian</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_SPEECHDATA_ZH_CN</strong></td>
<td align="left">Adds speech data for Chinese</td>
</tr>
<tr class="odd">
<td align="left">Features in the IoT Core Add-Ons</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><strong>OEM_CustomCmd</strong></td>
<td align="left">Adds scripts which support adding OEM Apps using the ADK Add-Ons</td>
</tr>
</tbody>
</table>

 

## <span id="Test_features_defined_by_Microsoft"></span><span id="test_features_defined_by_microsoft"></span><span id="TEST_FEATURES_DEFINED_BY_MICROSOFT"></span>Test features defined by Microsoft


The following table describes the Microsoft-defined test features that can be used by OEMs in the Features element in the **OEMInput** file for **Test** build ONLY.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Feature</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><strong>IOT_DISABLE_TESTSIGNING</strong></td>
<td align="left">Disables runtime-installation of test-signed packages</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_DISABLE_UMCI</strong></td>
<td align="left">Disables the code integrity check</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_EFIESP_TEST</strong></td>
<td align="left">UEFI packages required for booting test images. Should not be used with IOT_EFIESP</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_ENABLE_TESTSIGNING</strong></td>
<td align="left">Enables run-time installation of test-signed packages. Allows test-signed drivers and (.appx) apps to run</td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_KD_ON</strong></td>
<td align="left">Enables Kernel Debugger</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_KDNETUSB_SETTINGS</strong></td>
<td align="left">Includes all kernel debugger transports and enables KDNET over USB.
<p>The default debug transport settings for this feature are an IP address of &quot;1.2.3.4&quot;, a port address of &quot;50000&quot;, and a debugger key of &quot;4.3.2.1&quot;.</p>
<p>To use the default IP address of 1.2.3.4, run VirtEth.exe with the /autodebug flag.</p>
<p>For example, to establish a kernel debugger connection to the phone, use the following command:<code>Windbg -k net:port=50000,key=4.3.2.1</code></p>
<div class="alert">
<strong>Note</strong>  Do not include either <strong>IOT_KDUSB_SETTINGS</strong> or <strong>IOT_KDNETUSB_SETTINGS</strong> if you need to enable MTP or IP over USB in the image. If the kernel debugger is enabled in the image and the debug transports are used to connect to the device, the kernel debugger has exclusive use of the USB port and prevents MTP and IP over USB from working.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_KDSERIAL_SETTINGS</strong></td>
<td align="left">Includes all kernel debugger transports and enables KDSERIAL with the following settings: 115200 Baud, 8 bit, no parity</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_KDUSB_SETTINGS</strong></td>
<td align="left">Includes all kernel debugger transports and enables KDUSB.
<p>The default debug transport target name for this feature is <strong>WOATARGET</strong>. To establish a kernel debugger connection to the phone, use the following command. <code>Windbg -k usb:targetname=WOATARGET</code></p>
<div class="alert">
<strong>Note</strong>  Do not include either <strong>IOT_KDUSB_SETTINGS</strong> or <strong>IOT_KDNETUSB_SETTINGS</strong> if you need to enable MTP or IP over USB in the image. If the kernel debugger is enabled in the image and the debug transports are used to connect to the device, the kernel debugger has exclusive use of the USB port and prevents MTP and IP over USB from working.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_WDTF</strong></td>
<td align="left">Includes components for Windows Driver Test Framework, required for HLK validation</td>
</tr>
</tbody>
</table>

 

## <span id="Device-specific_features"></span><span id="device-specific_features"></span><span id="DEVICE-SPECIFIC_FEATURES"></span>Device-specific features


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Features</th>
<th align="left">Description</th>
<th align="left">Device</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><strong>IOT_DISABLEBASICDISPLAYFALLBACK</strong></td>
<td align="left">Disables the inbox basic render driver.
<p>This feature should only be used with the Qualcomm DragonBoard.</p></td>
<td align="left">Required for DB.</td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_DMAP_DRIVER</strong></td>
<td align="left">Adds DMAP drivers.</td>
<td align="left">Required for MBM and RPi2.
<p>Not supported for DB.</p></td>
</tr>
<tr class="odd">
<td align="left"><strong>IOT_EFIESP_BCD</strong></td>
<td align="left">Sets boot configuration data (BCD) for GPT-based drives used by MBM.
<div class="alert">
<strong>Note</strong>  RPi2 includes BCD settings for MBR devices through Microsoft-IoTUAP-RPi2-BCD-Package.cab.
</div>
<div>
 
</div></td>
<td align="left">Required for MBM.
<p>Not supported for RPi2.</p></td>
</tr>
<tr class="even">
<td align="left"><strong>IOT_POWER_SETTINGS (Windows 10, version 1607) or IOT_POWER_SETIINGS (Windows 10 IoT Core Build 10586).</strong></td>
<td align="left">Prevents the device from going to sleep due to inactivity.
<p>Note, the name of this package changed in Windows 10, version 1607.</p></td>
<td align="left">Required for MBM and other x86/amd64 platforms.</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Related topics


[What's in the Windows ADK IoT Core Add-ons](iot-core-adk-addons.md)

[IoT Core manufacturing guides](iot-core-manufacturing-guide.md)

 

 



