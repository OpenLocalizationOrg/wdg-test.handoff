---

title: OEM deployment of Windows 10 for desktop editions

author: Justinha

description: Get step-by-step guidance for OEMs to deploy Windows 10 to desktop computers, laptops, and 2-in-1s. Find information about how to enable imageless, push-button reset recovery and more.  

---

# OEM deployment of Windows 10 for desktop editions

This guide documents a prescriptive method for Windows 10 deployment using the classic deployment tools. Many of the tools and methods used in a Windows 8.1 classic deployment are applicable to Windows 10. The biggest change is the recovery process, where Windows 10 enables image-less recovery.

This guide is intended for OEMs, and applies to Windows 10 for desktop editions (Home, Pro, Enterprise, and Education). IT professionals using this guide should have prior knowledge of Windows basic administration and troubleshooting. For more information about what's new in Windows 10 deployment, see [Windows 10 Deployment and Tools](https://technet.microsoft.com/library/mt297512.aspx).

## About this guide

This guide is organized around three hardware and software configurations.

| Hardware configuration       | 1            | 1b           | 2            |
|------------------------------|--------------|--------------|--------------|
| Form factor                  | Small tablet | 2-in-1       | Notebook     |
| RAM                          | 1 GB         | 2 GB         | 4 GB         |
| Disk capacity and type       | 16 GB eMMC   | 32 GB eMMC   | 500 GB HDD   |
| Display size                 | 8"           | 10"          | 14"          |
| Windows SKU                  | Core         | Pro          | Core         |
| Language(s)                  | EN-US        | EN-US, DE-DE | EN-US, DE-DE |
| Cortana                      | Yes          | Yes          | Yes          |
| Inbox apps (universal)       | Yes          | Yes          | Yes          |
| Pen                          | No           | Yes          | No           |
| Office (Universal)           | Yes          | Yes          | Yes          |
| Classic Windows applications | No           | Yes          | Yes          |
| Office 2016                  | No           | Yes          | Yes          |
| Compact OS                   | Yes          | Yes          | No           |

Many of the tools and deployment techniques are the same as those used for Windows 8.1. Windows 10 has deprecated WIMBoot and replaced it with [Compact OS](compact-os.md). 

### OEM Activation 3.0

For OEMs deploying systems with **OEM Activation 3.0 (OA 3.0)** enabled, please pay special attention to important additional steps and guidance regarding **OA 3.0** considerations.

### Deployment and imaging tools environment

The Windows Assessment and Deployment Kit (Windows ADK) is a collection of tools and documentation that OEMs, ODMs, and IT Professionals use to customize, assess, and deploy Windows operating systems to new computers. Deployment tools enable you to customize, manage, and deploy Windows images. They can also be used to automate Windows deployments, removing the need for user interaction during Windows setup.

You must use the matching version of the Windows ADK for the images that you plan to customize. For example, if you are customizing an image based on Windows 10, version 1511, you must use the Windows ADK from Windows 10, version 1511.  


|  Windows version  | Link to run ADKSetup.exe      |
|-------------------|-------------------------------|
| Windows 10 RTM    | [**Windows ADK**](http://download.microsoft.com/download/8/1/9/8197FEB9-FABE-48FD-A537-7D8709586715/adk/adksetup.exe)  |
| Windows 10, version 1511 | [**Windows ADK**](http://download.microsoft.com/download/3/8/B/38BBCA6A-ADC9-4245-BCD8-DAA136F63C8B/adk/adksetup.exe) |

Tools inside Windows ADK that you will use with this guide:

-   Windows 10 PE

-   Deployment Image Servicing and Management (DISM) tool

-   Windows System Image Manager (SIM)

-   OSCDIMG, BCDBoot, and other tools and interfaces

-   User State Migration Tool (USMT)

-   Windows Overlay Filter (WOF) Driver:

    -   Windows 10 version of WOF driver supports Compact OS

    -   Backward compatible to WIMBoot v1 on downlevel OS

    -   Inbox in Windows 10 and Windows 10 PE

    -   Installed using Deployment Tools category to supported downlevel host (to Windows 7)

### Prerequisites

To complete the steps outlined in this guide, OEMs will require:

-   **A Technician computer**: A PC running Windows 10 or Windows 8.1 on which [Windows ADK](http://go.microsoft.com/fwlink/?LinkId=293840) will be installed. 32-bit PCs require a 32-bit version of Windows to use some tools.

Note: This guide provides sample Windows PowerShell script to automate offline servicing section. In order to use this script, you will need a Technician computer running Windows 10.

-   **A Reference computer**: A PC that represents all of the PCs in a single model line; for example, the Fabrikam Notebook PC Series.
    
    For this PC, choose from something that resembles the Hardware Configuration Table.

    ![What is the ADK?](images/what-is-adk.png)

    Note: We recommend using the 32-bit version of Windows on the technician computer because the 32-bit version supports both 32-bit and 64-bit deployments.

    Follow the on-screen instructions to install **Windows 10 ADK**, including the **Deployment Tools**, **Windows Preinstallation Environment**, and **Windows Assessment Toolkit** features.

    ![Select ADK Features](images/adk-select-features.png)

    If the installation will be successful, click **Install**.

#### Creating my USB-B

-   The deployment steps in this guide depend on the sample configuration files included in **USB-B**. You can download [USB-B.zip](http://download.microsoft.com/download/5/8/4/5844EE21-4EF5-45B7-8D36-31619017B76A/USB-B.zip) from the Microsoft Download Center. 

-   The contents of the configuration files included in **USB-B** are examples that you may change according to your branding and manufacturing choices. However, file names and hierarchy of the folders and files must be the same as demonstrated below in order to align your deployment procedure with this guide.

Format your desired USB Drive and name it as follows:

![Extract USB](images/extract-usb.png) 

### Software downloads

To complete this guide, many OPK downloads are required from <https://www.microsoftoem.com>. The table below shows the required and optional downloads before getting started on the imaging process.

This guide uses Windows 10 RTM images as examples for creating images. You should check for the latest OPK on <https://www.microsoftoem.com> before completing the sections in this guide.

**Important: The version of Windows components, Windows ADK, language packs, FOD, and Language Interface Pack must match the Windows 10 image version.**

#### Windows 10 RTM images and updates

|           |                                  |
|-----------|----------------------------------|
| X20-09658 | Windows 10 Home 32 64 English OPK    |
| X20-09716 | Windows 10 Home SL 32 64 English OPK |
| X20-09737 | Windows 10 Pro 32 64 English OPK     |

#### Optional: Windows 10 language packs, FODs, and Appx bundles

Note: When installing new or additional language packs, the FODs, and Appx packages are required downloads.

|           |                                            |
|-----------|--------------------------------------------|
| X20-20209 | Windows 10 32 64 MultiLang OPK LangPackAll     |
| X20-53652 | Windows 10 for desktop editions OPK Supp Updates Sep15     |
| X20-52949 | Office Mobile MultiLang OPK -2             |
| X19-96440 | Office 2013 Single Image v15.4 English OPK |

#### Windows 10, version 1511 images and updates

|  |       |
|-----------|----------------------------------------|
| X20-74664 | Windows 10 Home, version 1511 32/64 English OPK     |
| X20-74668 | Windows 10 Home SL, version 1511 32/64 English OPK  |
| X20-74669 | Windows 10 Home SL, version 1511 32/64 Eng Intl OPK |
| X20-74672 | Windows 10 Pro, version 1511 32/64 English OPK      |

#### Optional: Windows 10, version 1511 language packs, FODs, and Appx bundles

Note: When installing new or additional language packs, the FODs, and Appx packages are required downloads.

|   |   |
|-----------|-------------------------------------------------|
| X20-74675 | Windows 10, version 1511 32/64 MultiLang OPK LangPackAll/LIP |
| X20-74677 | Windows 10, version 1511 32/64 MultiLang OPK Feat on Demand  |
| X20-87906 | Windows 10, version 1511 32-BIT/X64 MultiLang OPK App Update |
| X20-52949 | Office Mobile MultiLang OPK -2                  |
| X19-96440 | Office 2013 Single Image v15.4 English OPK      |

## Windows 10 deployment procedure

This section walks you through scripts and steps for creating a Windows 10 image.

This guide uses samples of configuration files and scripts, as well as storing a copy of the Windows installation files on a USB key. Before starting this guide, complete the steps in [Creating My USB-B](#creating-my-usb-b).

This flowchart shows the deployment steps:

![Deployment process](images/deployment-process.png)

### Create a WinPE bootable USB

1.  Press the Windows key to display the **Start** menu. Type:
    
        Deployment and Imaging Tools Environment

    Right-click the name of the tool and then click **Run as administrator**.

    Optional: speed up the optimization and image capture processes by setting the power scheme to High performance:

    -   Click **Start**, type **powercfg.cpl**. The **Power Options** control panel appears.

    -   Select the **High Performance** power scheme. (If it's not shown, select **Show additional plans**.)

1.  From the "Deployment Tools Environment command prompt" Copy base WinPE to a new folder on the Technician Computer.
    
    If you use an **x64** Windows 10 image, copy the x64 WinPE folder structure:

        Copype amd64 C:\winpe_amd64

    If you use an **x86** Windows 10 image, copy the x86 WinPE folder structure:

        Copype x86 C:\winpe_x86

1.  **Optional**: Mount WinPE image to add Additional packages and languages.
   
    If you use an **x64** Windows 10 image, mount the x64 WinPE image:

        Dism /mount-image /imagefile:c:\WinPE_amd64\media\sources\boot.wim /index:1 /mountdir:c:\winpe_amd64\mount

    If you use an **x86** Windows 10 image, mount the x86 WinPE image:

        Dism /mount-image /imagefile:c:\WinPE_x86\media\sources\boot.wim /index:1 /mountdir:c:\winpe_x86\mount

#### Add packages, dependencies, and language packs

Use **Dism** command with the **/Add-Package** option. For example, in order to use Windows PowerShell in WinPE, NetFx dependency and the language packs must be installed.

If you use an **x64** Windows 10 image:

    dism /image:C:\winpe_amd64\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-NetFx.cab"
    dism /image:C:\winpe_amd64\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-NetFx_en-us.cab"
    dism /image:C:\winpe_amd64\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-PowerShell.cab"
    dism /image:C:\winpe_amd64\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-Powershell_en-us.cab"

If you use an **x86** Windows 10 image:

    dism /image:C:\winpe_x86\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\WinPE-NetFx.cab"
    dism /image:C:\winpe_x86\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\en-us\WinPE-NetFx_en-us.cab"
    dism /image:C:\winpe_x86\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\WinPE-PowerShell.cab"
    dism /image:C:\winpe_x86\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\en-us\WinPE-Powershell_en-us.cab"

#### Add drivers 

Use the Dism command with the /Add-Driver option. 

Note: This method requires drivers to be INF based. It is recommended to get INF based drivers from the IHV Vendor.

If you use an **x64** Windows 10 image:

    dism /image:C:\winpe_amd64\mount /Add-Driver /driver:"C:\Out-of-Box Drivers\mydriver.inf"

If you use an **x86** Windows 10 image:

    dism /image:C:\winpe_x86\mount /Add-Driver /driver:"C:\Out-of-Box Drivers\mydriver.inf"

Note: To install all of the drivers in a folder and all its subfolders use the /recurse option. 

If you use an **x64** Windows 10 image:

    dism /Image:C:\Winpe_amd64 /Add-Driver /Driver:c:\drivers /Recurse

If you use an **x86** Windows 10 image:

    dism /Image:C:\Winpe_x86 /Add-Driver /Driver:c:\drivers /Recurse

#### Cleanup boot.wim

Run cleanup to reduce the disk and memory footprint of WinPE, which is suited for lower-spec devices (such as devices with 1 GB Ram or 16 GB Storage). This increases compatibility with a wider range of devices. If DISM /Resetbase is a long-running operation, you can specify the /Defer parameter with /Resetbase to defer any long-running cleanup operations to the next automatic maintenance. But we highly recommend you **only** use /Defer as an option in the factory where DISM /Resetbase requires more than 30 minutes to complete.

If you use an **x64** Windows 10 image:

    dism /image:c:\winpe_amd64\mount /Cleanup-image /StartComponentCleanup /ResetBase

If you use an **x86** Windows 10 image:

    dism /image:c:\winpe_x86\mount /Cleanup-image /StartComponentCleanup /ResetBase 

#### Optimize Winpe on boot

Optimize online winpe by setting the power scheme to High performance:

Note: Setting up high performance can impact thermal performnce of the device.

If you use an **x64** Windows 10 image:

    Notepad.exe c:\winpe_amd64\mount\windows\system32\startnet.cmd

Enter into notepad:

    powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c

Close and save notepad.

If you use an **x86** Windows 10 image:

    Notepad.exe c:\winpe_x86\mount\windows\system32\startnet.cmd

Enter into notepad: 

    powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c

Close and save notepad.

![Startnet](images/startnet.png)

#### Commit changes to WinPE 

Run the commands in the following table to commit the changes to WinPE and mark files for deletion.

If you use an **x64** Windows 10 image:

    dism /unmount-image /mountdir:c:\winpe_amd64\mount /commit
    dism /export-image /sourceimagefile:c:\winpe_amd64\media\sources\boot.wim /sourceindex:1 /DestinationImageFile:c:\winpe_amd64\mount\boot2.wim
    Del c:\winpe_amd64\media\sources\boot.wim
    Copy c:\winpe_amd64\mount\boot2.wim c:\winpe_amd64\media\sources\boot.wim
   
If you use an **x86** Windows 10 image:

    dism /unmount-image /mountdir:c:\winpe_x86\mount /commit
    dism /export-image /sourceimagefile:c:\winpe_x86\media\sources\boot.wim /sourceindex:1 /DestinationImageFile:c:\winpe_x86\mount\boot2.wim
    Del c:\winpe_x86\media\sources\boot.wim
    Copy c:\winpe_x86\mount\boot2.wim c:\winpe_x86\media\sources\boot.wim

#### Create a bootable USB key

1.  Connect **USB-A** Drive. Make sure the drive capacity is at least 4 GB.

2.  Make the inserted USB a WinPE bootable USB drive.

    Note: If you have labeled your USB-A stick, the script below will overwrite the label to “WinPE". 

    If you use an **x64** Windows 10 image:

        MakeWinPEMedia /UFD /f C:\winpe_amd64 E:

    If you use an **x86** Windows 10 image:

        MakeWinPEMedia /UFD /f C:\winpe_x86 E:    
    
    (where E: is the drive letter of **USB-A**)

### Install Windows with basic customizations

For a document to help you tailor the customizations defined in your unattend.xml file, see the [Windows 10 Update OEM Policy Document (OPD)](https://myoem.microsoft.com/oem/myoem/en/topics/Licensing/roylicres/ost2016/Pages/COMM-Win10-OPD-RTM-Now-Avail.aspx).

Download Windows 10 Professional from the [Digital Operations Center](http://www.microsoftoem.com/) Software Order Center, and use the Microsoft Media Creation Tool from [SOC Resources](https://moo.microsoftoem.com/okdnet/SOCResources.aspx) to generate the ISO files. OEMs can download the Windows kit that is applicable to them in terms of language and edition.

#### Create an answer file

An "answer file" is an XML-based file that contains setting definitions and values to use during Windows Setup. In an answer file, you specify various setup options, including how to partition disks, the location of the Windows image to install, and the product key to apply. Values that apply to the Windows installation, such as the names of user accounts, display settings, and Internet Explorer Favorites can also be specified. The answer file for Setup is typically called **Unattend.xml**.

Answer files created in Windows System Image Manager (Windows SIM) are associated with a particular Windows image. This enables validating the settings in the answer file to the settings available in the Windows image. However, because any answer file can be used to install any Windows image, if there are settings in the answer file for components that do not exist in the Windows image, those settings are ignored.

This guide uses three different answer files:

![Three answer files](images/three-answer-files.png)

Note: You need to point Windows System Image Manager (SIM) to an install.wim before you can create and customize the answer file.

1.  On the Technician Computer, mount the ISO image of Windows (see below) by double clicking it. Highlight all files on the mounted ISO to E:\MyWindows as shown in the following diagram.

    ![Mounted files](images/mounted-files.png)

1.  Run **Windows System Image Manager** to start creating an answer file from scratch. This tool allows the creation or management of the answer files in an easy and organized manner.
    
    ![Create an answer file](images/create-answer-file.png)  
    
    Reference: Please refer to the [WSIM overview](https://technet.microsoft.com/library/hh825214.aspx) for more information on Windows System Image Manager (SIM) user interface.

1.  Click **File** > **Select Windows Image**. Browse to E:\MyWindows\Sources\*Install.wim*. A Catalog file will be created (.clg file) for that specific wim.

    Troubleshoot: Catalog creation may fail due to several reasons. You may receive the following:

    ![Catalog creation error](images/catalog-creation-fail.png)

1.  Click **Yes**.

    Note: Please make sure install.wim has read/write permissions and the individual who is creating this is the administrator of the local machine. Install.wim image and Windows 10 ADK versions must be the same. If the below error is displayed, make sure the correct architecture (x86 or x64) of Windows 10 is installed on the Technician computer. 
    
    ![Architecture mismatch](images/catalog-fail-arch-mismatch.png)

1.  Open a sample answer file or create a new one. We recommend using the sample answer files provided in **USB-B**.
    
    <table>
    <tr>
    <td>For OA 3.0 systems: 
    </td>
    <td>**USB-B**\ConfigSet\\**OA3.0**\AutoUnattend.xml
    </td>
    </tr>
    <tr>
    <td>For non-OA 3.0 systems: 
    </td>
    <td>**USB-B**\ConfigSet\\**Non_OA3.0**\AutoUnattend.xml
    </td>
    </tr>
    </table>
   
1.  To associate the answer file with the Windows Image, click **OK** when prompted.

#### Customize the answer file

Note: A blank character in specialize | Microsoft-Windows-Shell-Setup | Computer Name section of the answer file will result in Windows installation failure.

1.  For basic customizations, please review the provided sample answer files in **USB-B**. Note that the sample answer files in the **USB-B** have placeholder values that must be changed before its use. We recommend OEMs to build their own answer files and use the following files as a starting point or reference:

    <table>
    <tr>
    <td>For OA 3.0 systems: 
    </td>
    <td>**USB-B**\ConfigSet\\**OA3.0**\AutoUnattend.xml
    </td>
    </tr>
    <tr>
    <td>For non-OA 3.0 systems: 
    </td>
    <td>**USB-B**\ConfigSet\\**Non_OA3.0**\\AutoUnattend.xml
    </td>
    </tr>
    </table>
   
    See [Technet reference Sample Unattend](https://technet.microsoft.com/library/cc732280.aspx) for addition samples.

1.  Specify the product key in the answer file. This customization is included in the sample answer file in **USB-B**. There are two different ways to specify the product key in the answer file:

    -   Use this [ProductKey](http://technet.microsoft.com/library/cc721925.aspx) setting in the Microsoft-Windows-Setup component during the **windowsPE pass** to specify the Windows image to install during Windows Setup. The product key specified by this setting is stored on the computer after installation. When Windows is activated, this product key will be used.

    -   Use this [ProductKey](http://technet.microsoft.com/library/cc749389.aspx) setting in the Microsoft-Windows-Shell-Setup component during the **specialize pass** to specify a different product key to activate Windows. For example, one product key may be specified to install Windows with the ProductKey in Microsoft-Windows-Setup component, and then specify a different product key to activate Windows.

Please refer to the Kit Guide Windows 10 Default Manufacturing Key OEM PDF to find default product keys for **OA3.0** and **Non-OA3.0** keys.

Example: Navigate to OPK X20-74664 Win Home 10 1511 32 64 English OPK\Print Content\X20-09791 Kit Guide Win 10 Default Manufacturing Key OEM\X2009791GDE.pdf.

#### Install Windows

1.  Connect the **USB-A** drive and boot the Reference computer.

    Note: If booting with USB drive fails, make sure USB boot has been prioritized instead of HDD boot. To do so, it may be necessary to go to the Reference Computer’s/Device’s BIOS menu and adjust the boot priority order so that the USB Key is at the top of the list.

1.  After WinPE has been booted, insert **USB-B**.

2.  At the **X:\windows\system32>** prompt, type ***diskpart*** and press the **&lt;Enter&gt;** key to start Diskpart.

3.  At the **\DISKPART&gt;** prompt type ***list volume***.

    Note: Check what letter USB-B has been assigned under the “Ltr" column (Example: E). This is the drive letter that will be used throughout the installation.

1.  Type ***exit*** to quit Diskpart.

    ![Diskpart](images/diskpart.png)
    
1.  Execute ***setup.exe*** with an answer file and install Windows 10 Update with additional OEM customizations. Copy the commands in the following table.

    <table>
    <tr>
    <td>For OA 3.0 systems: 
    </td>
    </tr>
    <tr>
    <td><code><br>Xcopy /herky e:\configset\$oem$ e:\MyWindows\Sources\$OEM$</br>
    <br>E:\MyWindows\Setup.exe /unattend:E:\Configset\OA3.0\AutoUnattend.xml
    </br></code>
    <p>Press D for directory when prompted.
    </p>
    </td>
    </tr>
    </table>
    
    <table>
    <tr>
    <td>For non-OA 3.0 systems: 
    </td>
    </tr>
    <tr>
    <td><code><br>Xcopy /herky e:\configset\$oem$ e:\MyWindows\Sources\$OEM$</br>
    <br>E:\MyWindows\Setup.exe /unattend:E:\Configset\non_oa3.0\AutoUnattend.xml
    </br></code>
    <p>Press D for directory when prompted.
    </p>
    </td>
    </tr>
    </table>
  
1.  After installation files have been copied, disconnect **USB-A**.

2.  Disconnect **USB-B** immediately after the computer reboots.

If you use the AutoUnattend, the system will automatically boot into Audit mode and the System Preparation Tool (sysprep) appears. If the device boots to the Languages or the Hi there screen instead, press Ctrl+Shift+F3 to enter Audit mode. The device reboots to the Desktop and the System Preparation Tool (sysprep) appears. Ignore Sysprep for now.

Note: Many Windows features, including the Start menu and the Settings menu, do not work in this environment.

#### Apps and Store opportunities

Through Windows 10 and the Windows Store, you have tremendous opportunities for brand and device differentiation, revenue creation, and customer access.

Windows Store apps are at the center of the Windows 10 experience. They are Windows universal apps, so you can build apps for desktops, tablets, or phones that run Windows 10. As an OEM, you can provide an engaging customer experience and increase brand loyalty by providing a great set of value-added software and services along with the high-quality hardware that you build.

Important: The key below must be set in Audit mode. If you have completed the [Install Windows](#install-windows) section, you should be in Audit mode.

Change the registry setting and add the OEM ID. OEM Windows Store Program participants, contact <PartnerOps@microsoft.com> to get your OEM ID.

| **Item**  | **Location in Registry**                                                                   |
|-----------|--------------------------------------------------------------------------------------------|
| **OEMID** | HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Store, (REG_SZ) OEMID |

***Regedit.exe***

1.  Run regedit.exe from a command prompt.

2.  Navigate to HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Store.

3.  Right-click under (Default) &gt; click **New**.

4.  Click **String Value**.

5.  Type: 

        OEMID

6.  Double-click **OEMID** and enter the OEM name in the **Value data:** text field.

    Important: The OEMID registry key is not restored automatically during PBR in Windows 10. For more information about how to restore the OEMID registry key during PBR operation, see [Prepare system for recovery Push Buton Reset scenarios](#prepare-system-for-recovery-push-button-reset-scenarios).

#### Verify customizations in Audit mode

Connecting the computer to the internet is not recommended during manufacturing stages. It is not recommended to get the updates from Windows Update in audit mode. This will likely generate an error when you run generalize + syspreping on the machine from audit mode.

1.  After setup has finished, the computer logs into Windows in Audit mode automatically as an Administrator.

2.  Verify the changes which were stated in the answer file (see manufacturer name, support phone number and other customizations).

3.  The image must be generalized before being used as a manufacturing image; select the **Generalize** checkbox.

4.  In the System Cleanup Action box, select **Enter System Out-of-Box Experience**.

5.  In the shutdown options box select **Shutdown**.

    ![Sysprep generalize](images/sysprep.png)

Important: The system must be set to generalize and OOBE in order to further service the image. In the following sections, an unattend file will be used to return to Audit mode on the OOBE-sealed system. There are known issues when resealing to Audit mode in this phase and it is not recommended.

### Capture basic image

1.  Connect "**USB-A**" and boot the Reference computer.

2.  After WinPE has booted, connect to **USB-B**.

    Troubleshoot: At the end of the previous section [Verify customizations in Audit mode](#verify-customizations-in-audit-mode), the reference system was shutdown. While turning on if the system continues to boot from Internal HDD, Windows will enter specialize pass and then OOBE pass. **In order to capture a generalized and stable image none of the Windows passes must be completed. To fix this, we need to generalize the image again. At the OOBE screen, press **&lt;Ctrl&gt;+&lt;Shift&gt;+&lt;F3&gt;. The system restarts in Audit mode. In Audit mode, Sysprep the system by using the OOBE Shutdown and Generalize switches, as shown in the previous diagram. After the system reboots, please make sure to boot from **USB-A** to WinPE.

    If the system still boots with internal HDD, please make sure USB boot is prioritized instead of HDD boot. To do so, it may be neceesary to enter the Reference Computer BIOS menu and adjust the boot priority order so that the USB Key is at the top of the list.

1.  Identify Windows partition drive letter.

    -   At the **X:\windows\system32&gt;** prompt, type ***diskpart*** and press **&lt;Enter&gt;** to start Diskpart.

    -   At the **\DISKPART&gt;** prompt type ***list volume***.

    -   Under the "Label" column, locate the volume that is labeled "Windows".

    -   Note what letter it is has been assigned under the “Ltr” column (Example: C). This is the drive letter that needs to be used.

    -   Type ***exit*** to quit Diskpart.

    ![Diskpart](images/diskpart-v10.png)

1.  Capture the image of the windows partition to **USB-B**. This process takes several minutes.

    Note: It is recommended to run dism operations using a cache directory. For this sample, we create a scratch directory on the USB-B key for temporary files, which is assigned drive letter "E:" in the examples. 

        MD e:\scratchdir

        Dism /Capture-Image /CaptureDir:C:\ /ImageFile:E:\Images\BasicImage.wim /Name:"myWinImage" /scratchdir:e:\scratchdir

### Offline servicing

Modify your images by adding and removing languages, drivers, and packages.

![Create a model specific image](images/create-model-specific-image.png)

#### Mount image

Insert **USB-B** into your technician computer.

1.  Mount Windows image (BasicImage.wim) This process extracts the contents of the image file to a location where the mounted image it can be viewed and modified

        Md C:\mount\windows

        Dism /Mount-Wim /WimFile:E:\Images\BasicImage.wim /index:1 /MountDir:C:\mount\windows

    (where E:\ is the drive letter of **USB-B**)

1.  Mount Windows RE Image file.

        Md c:\mount\winre

        Dism /Mount-Image /ImageFile:C:\mount\windows\Windows\System32\Recovery\winre.wim /index:1 /MountDir:C:\mount\winre

    Troubleshoot: If winre.wim cannot be seen under the specified directory, use the following command to set the file visible:

        attrib -h -a -s C:\mount\windows\Windows\System32\Recovery\winre.wim

Troubleshoot: If the mounting operation fails, make sure the Windows 10 version of DISM is the one installed with the Windows ADK. Make sure that version is being used and not an older version from the Technician Computer. Do not mount images to protected folders, such as the User\Documents folder. If DISM processes are interrupted, consider temporarily disconnecting from the network and disabling antivirus protection.

#### Add drivers

1.  Adding driver packages (.inf files) one by one. SampleDriver\driver.inf is a **sample** driver package that is specific to the computer model. (Type a specific driver path). **If there are multiple driver packages please skip to the next step**.

2.  Multiple drivers can be added on one command line if a folder is specified instead of an .inf file. To install all of the drivers in a folder and all its subfolders, use the **/recurse** option.

        Dism /Image:C:\mount\windows /Add-Driver /Driver:c:\SampleDrivers /Recurse

        Dism /Add-Driver /Image:C:\mount\windows /Driver:"C:\SampleDriver\driver.inf"

1.  Review the contents of the %WINDIR%\Inf\ (C:\mount\windows\Windows\Inf\) directory in the mounted Windows image to ensure that the .inf files were installed. Drivers added to the Windows image are named OEM\*.inf. This is to ensure unique naming for new drivers added to the computer. For example, the files MyDriver1.inf and MyDriver2.inf are renamed Oem0.inf and Oem1.inf.

2.  Verify your driver has been installed for both images.

        Dism /Image:C:\mount\windows /Get-Drivers
        

Important: If the driver contains only the installer package and doesn’t have an .inf file, the driver in AUDIT mode may be installed by double-clicking the corresponding installer package. Some drivers may be incompatible with Sysprep tool; they will be removed after sysprep generalize even if they have been injected offline. 

In these cases, you need to add PersistAllDeviceInstalls and DoNotCleanupNonPresentDevices (sections below) extra parameter to UnattendSysprep.xml.

<table>
<tr>
<td>For OA 3.0 systems: 
</td>
<td>**USB-B**\AnswerFiles\\**OA3.0**\UnattendSysprep.xml
</td>
</tr>
<tr>
<td>For non-OA 3.0 systems: 
</td>
<td>**USB-B**\AnswerFiles\\**Non_OA3.0**\UnattendSysprep.xml
</td>
</tr>
</table>

##### PersistAllDeviceInstalls

To save time during installation and to speed up the out-of-box experience for end users, instruct Windows Setup that the hardware on the reference computer and the destination computers are identical. By doing this, Windows Setup maintains driver configurations during image capture and deployment.

The following XML output specifies that device drivers will not be uninstalled during the generalize pass.

&lt;PersistAllDeviceInstalls&gt;true&lt;/PersistAllDeviceInstalls&gt;

Add the setting in the following table to **USB-B**\Answerfiles\OA3.0\UnattendSysprep.xml.

***x64/x86 Distinction***

OEMs using x64 Windows 10 image, add the following setting to **USB-B**\Answerfiles\OA3.0\UnattendSysprep.xml:

    <settings pass="generalize">
        <component name="Microsoft-Windows-PnpSysprep" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <PersistAllDeviceInstalls>true</PersistAllDeviceInstalls>
        </component>
    </settings>

OEMs using **x86 Windows 10 image**, add the following setting to **USB-B**\AnswerFiles\OA3.0\UnattendSysprep.xml:

    <settings pass="generalize">
        <component name="Microsoft-Windows-PnpSysprep" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <PersistAllDeviceInstalls>true</PersistAllDeviceInstalls>
        </component>
    </settings>

##### DoNotCleanupNonPresentDevices

The **DoNotCleanUpNonPresentDevices** setting specifies whether plug-and-play information for devices that are not detected on the destination computer during the next specialize should remain on the computer.

However, when both **PersistAllDeviceInstalls** and **DoNotCleanUpNonPresentDevices** are set to **true**, the device information remains on the computer. For more information, see [DoNotCleanUpNonPresentDevices](https://msdn.microsoft.com/library/windows/hardware/dn915711.aspx).

The following diagram describes the process Windows Setup uses to determine whether plug-and-play information remains on the computer, or is removed, or is removed and then re-initialized.

![DoNotCleanupNonPresentDevices](images/do-not-cleanup-nonpresent-devices.png)

The following XML output specifies that device drivers will not be uninstalled during the generalize pass.

OEMs using **x64** Windows 10 image, add the following setting to **USB-B**\AnswerFiles\OA3.0\UnattendSysprep.xml:

    <settings pass="generalize">
        <component name="Microsoft-Windows-PnpSysprep" processorArchitecture="amd64"
        publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" 
        xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" 
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <DoNotCleanUpNonPresentDevices>true</DoNotCleanUpNonPresentDevices >
        </component>
    </settings>

OEMs using **x86** Windows 10 image, add the following setting to **USB-B**\AnswerFiles\OA3.0\UnattendSysprep.xml:

    <settings pass="generalize">
        <component name="Microsoft-Windows-PnpSysprep" processorArchitecture="x86" 
        publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" 
        xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" 
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <DoNotCleanUpNonPresentDevices>true</DoNotCleanUpNonPresentDevices >
        </component>
    </settings>

#### Adding LPs / LIPs / FoDs / GDRs

The following table lists language pack components and any dependencies. For more information, see [Language packs](language-packs--lpcab--and-windows-deployment.md) and [Features on Demand (FoD)](features-on-demand-v2--capabilities.md).


<table border="1" cellpadding="0">
    <tbody>
        <tr>
            <td>
                <p align="center">
                    <strong>Component</strong>
                </p>
            </td>
            <td>
                <p align="center">
                    <strong>Sample file name</strong>
                </p>
            </td>
            <td>
                <p align="center">
                    <strong>Dependencies</strong>
                </p>
            </td>
            <td>
                <p align="center">
                    <strong>Description</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Language pack
                </p>
            </td>
            <td>
                <p>
                    <code>lp.cab</code>
                </p>
            </td>
            <td>
                <p>
                    None
                </p>
            </td>
            <td>
                <p>
                    UI text, including basic Cortana capabilities.
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Language interface pack
                </p>
            </td>
            <td>
                <p>
                    <code>lp.cab</code>
                </p>
            </td>
            <td>
                <p>
Requires a specific fully-localized or partially-localized language pack. Example: ca-ES requires es-ES.
                </p>
            </td>
            <td>
                <p>
                    UI text, including basic Cortana capabilities.
                </p>
                <p>
                    Not all of the language resources for the UI are included in a LIP. LIPs require at least one language pack (or parent language). A parent
                    language pack provides support for a LIP. The parts of the UI that are not translated into the LIP language are displayed in the parent
                    language. In countries or regions where two languages are commonly used, you can provide a better user experience by applying a LIP over a
                    language pack.
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Basic
                </p>
            </td>
            <td>
                <p>
                    <code>Microsoft-Windows-LanguageFeatures-Basic-fr-fr-Package</code>
                </p>
            </td>
            <td>
                <p>
                    None
                </p>
            </td>
            <td>
                <p>
                    Spell checking, text prediction, word breaking, and hyphenation if available for the language.
                </p>
                <p>
                    You must add this component before adding any of the following components.
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Fonts
                </p>
            </td>
            <td>
                <p>
                    <code>Microsoft-Windows-LanguageFeatures-Fonts-Thai-Package</code>
                </p>
            </td>
            <td>
                <p>
                    None
                </p>
            </td>
            <td>
                <p>
                    Fonts.
                </p>
                <p>
Required for some regions to render text that appears in documents. Example, th-TH requires the Thai font pack. 
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Optical character recognition
                </p>
            </td>
            <td>
                <p>
                    <code>Microsoft-Windows-LanguageFeatures-OCR-fr-fr-Package</code>
                </p>
            </td>
            <td>
                <p>
                    Basic
                </p>
            </td>
            <td>
                <p>
                    Recognizes and outputs text in an image.
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Handwriting recognition
                </p>
            </td>
            <td>
                <p>
                    <code>Microsoft-Windows-LanguageFeatures-Handwriting-fr-fr-Package</code>
                </p>
            </td>
            <td>
                <p>
                    Basic
                </p>
            </td>
            <td>
                <p>
                    Enables handwriting recognition for devices with pen input.
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Text-to-speech
                </p>
            </td>
            <td>
                <p>
                    <code>Microsoft-Windows-LanguageFeatures-TextToSpeech-fr-fr-Package</code>
                </p>
            </td>
            <td>
                <p>
                    Basic
                </p>
            </td>
            <td>
                <p>
                    Enables text to speech, used by Cortana and Narrator.
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    Speech recognition
                </p>
            </td>
            <td>
                <p>
                    <code>Microsoft-Windows-LanguageFeatures-Speech-fr-fr-Package</code>
                </p>
            </td>
            <td>
                <p>
                    Basic, Text-To-Speech recognition
                </p>
            </td>
            <td>
                <p>
                    Recognizes voice input, used by Cortana and Windows Speech Recognition.
                </p>
            </td>
        </tr>
    </tbody>
</table>


Always use language packs and Features-On-Demand (FOD) packages that match the language and platform of the Windows image.

Features on demand (FODs) are Windows feature packages that can be added at any time. When a user needs a new feature, they can request the feature package from Windows Update. OEMs can preinstall these features to enable them on their devices out-of-the-box.

Common features include language resources like handwriting recognition. Some of these features are required to enable full Cortana functionality.

Obtain the Win 10 32 64 MultiLang OPK LangPackAll and the Win 10 32-BIT x64 MultiLang OPK Feat on Demand from [DOC Center](https://www.microsoftoem.com).

Note: you must use the Microsoft Media Tool from [SOC Resources](https://moo.microsoftoem.com/okdnet/SOCResources.aspx) to merge the DDP folder structure into a mountable image for the Win 10 32-BIT x64 MultiLang OPK Feat on Demand From DOC Center.

1.  copy LP.cab to E:\Languagepacks\x86\de-de.

    ![Copy LP.cab](images/copy-lp-cab.png)

1.  Repeat x64 de-de language pack copying to E:\Languagepacks\x64\de-de for x64-bit devices.

2.  Copy partial language packs to **USB-B**.

3.  Highlight all de-de language packs and copy to E:\LanguageFeaturePacks\x86.

    ![Highlight Language Packs](images/highlight-language-packs.png)

    Where E: is drive letter of **USB-B**

Important: Do not install a language pack after an update. If an update (hotfix, general distribution release [GDR], or service pack [SP]) is installed that contains language-dependent resources before a language pack is installed, the language-specific changes that are contained in the update are not applied the update will need to be reinstalled. Always install language packs before installing updates.

Add languages, and Features On Demand to the Windows image.

For packages with dependencies, make sure you install the packages in order. For example, to enable Cortana, install: lp.cab, then –**Basic**, then –**TextToSpeech**, then –**Speech**, in this order. If you’re not sure of the dependencies, it’s OK to put them all in the same folder, and then add them all using the same DISM /Add-Package command, as shown in the examples in the following tables (where E: is the drive where language pack exists).

If you use an **x64** Windows 10 image:

    Dism /Add-Package /Image:"C:\mount\windows" /PackagePath:"E:\LanguagePacks\x64\de-de\lp.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-Basic-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-OCR-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-Handwriting-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-TextToSpeech-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-Speech-de-de-Package.cab" /packagepath:"e:\LanguageFeaturePacks\x64\Microsoft-Windows-RetailDemo-OfflineContent-Content-de-de-Package.cab"

If you use an **x86** Windows 10 image:

    Dism /Add-Package /Image:"C:\mount\windows" /PackagePath:"E:\LanguagePacks\x86\de-de\lp.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-Basic-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-OCR-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-Handwriting-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-TextToSpeech-de-de-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-Speech-de-de-Package.cab" /packagepath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-RetailDemo-OfflineContent-Content-de-de-Package.cab"

Verify the package has been installed to the Windows image:

    Dism /Get-Packages /Image:C:\mount\windows

##### [Optional] Add a language with additional fonts

As part of the language pack re-factoring for Windows 10, some languages with Fonts were factored out into their own Language Cabs. In this section, ja-JP language is added to the image with the language Font cab in addition to other feature language packs.

If you use an **x64** Windows 10 image:

    Dism /Add-Package /Image:"C:\mount\windows" /PackagePath:"E:\LanguagePacks\x64\ja-jp\lp.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-Basic-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-OCR-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-Handwriting-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-TextToSpeech-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-Speech-ja-jp-Package.cab" **/PackagePath:"E:\LanguageFeaturePacks\x64\Microsoft-Windows-LanguageFeatures-Fonts-Jpan-Package.cab"** /packagepath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-RetailDemo-OfflineContent-Content-ja-jp-Package.cab

If you use an **x86** Windows 10 image:

    Dism /Add-Package /Image:"C:\mount\windows" /PackagePath:"E:\LanguagePacks\x86\ja-jp\lp.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-Basic-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-OCR-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-Handwriting-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-TextToSpeech-ja-jp-Package.cab" /PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-Speech-ja-jp-Package.cab" **/PackagePath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-LanguageFeatures-Fonts-Jpan-Package.cab"** /packagepath:"E:\LanguageFeaturePacks\x86\Microsoft-Windows-RetailDemo-OfflineContent-Content-ja-jp-Package.cab"

#### Add languages to Windows RE

When you add languages to Windows, add them to Windows RE to ensure a consistent language experience in recovery scenarios.

Troubleshoot: If winre.wim cannot be seen under the specified directory, use the following command to make the file visible:

    attrib -h -a -s C:\mount\windows\Windows\System32\Recovery\winre.wim

If you use an **x64** Windows 10 image:

    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\lp.cab"    
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-Rejuv_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-EnhancedStorage_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-Scripting_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-SecureStartup_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-SRT_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-WDS-Tools_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-WMI_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-StorageWMI_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\de-de\WinPE-HTA_de-de.cab"

If you use an **x86** Windows 10 image:

    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\lp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-Rejuv_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-EnhancedStorage_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-Scripting_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-SecureStartup_de-de.cab"   
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-SRT_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-WDS-Tools_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-WMI_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-StorageWMI_de-de.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\de-de\WinPE-HTA_de-de.cab"

1.  Verify that the language packages are part of the image:

        Dism /Get-Packages /Image:"C:\mount\winre"

    where C is the drive letter of the drive that contains the image.

    Review the resulting list of packages and verify that the list contains the package. For example:

        Package Identity : Microsoft-Windows-WinPE-Rejuv_de-de ... de-de~10.0.9926.0 State : Installed

##### [Optional] Add ja-jp language packs

Complete this section only if you added ja-jp language packs from the optional section [Add a language with additional fonts](#add-a-language-with-additional-fonts).

If you use an **x64** Windows 10 image:

    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\lp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-Rejuv_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-EnhancedStorage_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-Scripting_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-SecureStartup_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-SRT_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-WDS-Tools_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-WMI_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-StorageWMI_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\ja-jp\WinPE-HTA_ja-jp.cab"

If you use an **x86** Windows 10 image:

    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\lp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-Rejuv_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-EnhancedStorage_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-Scripting_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-SecureStartup_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-SRT_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-WDS-Tools_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-WMI_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-StorageWMI_ja-jp.cab"
    Dism /image:C:\mount\winre /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs\ja-jp\WinPE-HTA_ja-jp.cab"

#### [Optional] Remove English language to make a single language image

In Windows 10, Microsoft will only make en-US versions available. To make the image a single image after installing de-DE in the previous section, you can remove en-US from the image, making it only de-DE.

OEMs will need to remove all en-US language packages from the system. The following sample removes pre-installed languages from Windows 10 Professional.

If you use an **x64** Windows 10 image:

    Dism /image:"c:\mount\windows" /remove-package /packagename:Microsoft-Windows-Client-LanguagePack-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename: Microsoft-Windows-LanguageFeatures-Basic-en-us-Package~31bf3856ad364e35~amd64~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-Handwriting-en-us-Package~31bf3856ad364e35~amd64~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-OCR-en-us-Package~31bf3856ad364e35~amd64~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-Speech-en-us-Package~31bf3856ad364e35~amd64~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-TextToSpeech-en-us-Package~31bf3856ad364e35~amd64~~10.0.10240.16384 /packagename:Microsoft-Windows-Prerelease-Client-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:Microsoft-Windows-RetailDemo-OfflineContent-Content-en-us-Package~31bf3856ad364e35~amd64~~10.0.10240.16384

If you use an **x86** Windows 10 image:

    Dism /image:"c:\mount\windows" /remove-package /packagename:Microsoft-Windows-Client-LanguagePack-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-Basic-en-us-Package~31bf3856ad364e35~x86~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-Handwriting-en-us-Package~31bf3856ad364e35~x86~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-OCR-en-us-Package~31bf3856ad364e35~x86~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-Speech-en-us-Package~31bf3856ad364e35~x86~~10.0.10240.16384 /packagename:Microsoft-Windows-LanguageFeatures-TextToSpeech-en-us-Package~31bf3856ad364e35~x86~~10.0.10240.16384 /packagename:Microsoft-Windows-Prerelease-Client-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:Microsoft-Windows-RetailDemo-OfflineContent-Content-en-us-Package~31bf3856ad364e35~x86~~10.0.10240.16384

Troubleshoot: If an error occurs while removing a package, such as:

    Error: 0x800f0825
    Package Microsoft-Windows-LanguageFeatures-Basic-en-us-Package may have failed due to pending updates to servicing components in the image. Try the command again. The command completed with errors. For more information, refer to the log file. The DISM log file can be found at C:\windows\Logs\DISM\dism.log. 
    
Run dism.exe remove package again on only the failing package.

#### [Optional] Remove additional languages from WinRE

If you removed any languages, they must also be removed from WinRE.

If you use an **x64** Windows 10 image:

    Dism /image:"c:\mount\winre" /remove-package /packagename:Microsoft-Windows-WinPE-LanguagePack-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-EnhancedStorage-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-HTA-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-Rejuv-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-Scripting-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-SecureStartup-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-SRT-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-StorageWMI-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-WDS-Tools-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384 /packagename:WinPE-WMI-Package~31bf3856ad364e35~amd64~en-US~10.0.10240.16384

If you use an **x86** Windows 10 image:

    Dism /image:"c:\mount\winre" /remove-package /packagename:Microsoft-Windows-WinPE-LanguagePack-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-EnhancedStorage-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-HTA-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-Rejuv-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-Scripting-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-SecureStartup-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-SRT-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-StorageWMI-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-WDS-Tools-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384 /packagename:WinPE-WMI-Package~31bf3856ad364e35~x86~en-US~10.0.10240.16384

#### Verify language components are part of the image

Use Dism to verify language components are part of the image.

    Dism /Get-Capabilities /Image:"C:\mount\windows"

#### Setting language regional settings

1.  Use Dism to set the default language of the image:

        Dism /Image:C:\mount\windows /Set-AllIntl:de-DE

        Dism /Image:C:\mount\winre /Set-AllIntl:de-DE

1.  Verify your changes:

        Dism /Image:C:\mount\windows /Get-Intl

1.  Set the timezone for the region of the default language applied:

        Dism /Image:C:\mount\windows /set-timezone:"W. Europe Standard Time"

#### Add update packages (KB packages) to the image

Use Dism to apply the latest GDR and any prerequisite KBs. Please verify on [SOC (Software  Order Center)](https://www.microsoftoem.com) the latest version of the "Windows Desktop OPK Supplemental" package. For example, this guide will use the latest GDR 2015.11 D. KB3118754. 

**Important: Do not install a language pack after an update. If an update (such as a hotfix, or general distribution release [GDR], or service pack [SP]) is installed that contains language-dependent resources before a language pack is installed, the language-specific changes that are contained in the update are not applied, and the update will need to be reinstalled. Always install language packs before installing updates.**

The following example shows how to extract KBs from the OPK download from SOC.

1.  Copy both architecture folders for KB3081452 to **USB-B**\Updates.

    Navigate to X20-53652 Windows Desktop OPK Supp Updates Sep15\Software - DVD\X20-53672 SW DVD5 Windows Supp Sep15 Disk 1 OEM\x20-53672.img and double-click. Find the 3081452 folder, as shown in following image.
    
    ![Update 3081452](images/update-3081452.png)
    
1.  Copy both architecture folders for KB3097617 to **USB-B**\Updates.

    Navigate to X20-86795 Windows Desktop OPK Supp Updates Oct15\Software - DVD\X20-86816 SW DVD5 Windows Supp Oct15 OEM\x20-86816.img and double-click. Find the 3097617 folder, as shown in following image.

    ![Update 3097617](images/update-3097617.png)

##### Apply GDR 2015.11 D. KB3118754

If you use an **x64** Windows 10 image:

    Dism /Add-Package /Image:C:\mount\windows /PackagePath:"E:\updates\KB3118754\x64\NEU\Windows10.0-KB3118754-x64.msu"      

If you use an **x86** Windows 10 image:

    Dism /Add-Package /Image:C:\mount\windows /PackagePath:"E:\updates\KB3118754\x86\NEU\Windows10.0-KB3118754-x86.msu"

Note: Each package will usually be a new KB, and will increase the build revision number of Windows on the device. The revision number of Windows for a device can be found in the following registry key: 

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\UBR

#### Add Update packages to WinRE

Important: KB3118754 must also be applied to WinRE.wim to fix a known issue: Recovery fails during Reset/Refresh when a Scanstate package (USMT.ppkg) contains an application which has a path with a leading space. Example: c :\Program Files\ Contoso\RunMe.exe.

Apply GDR 2015.11 D. KB3118754

If you use an **x64** Windows 10 image:

    Dism /Add-Package /Image:C:\mount\winre /PackagePath:"E:\updates\KB3118754\x64\NEU\Windows10.0-KB3118754-x64.msu"

If you use an **x86** Windows 10 image:

    Dism /Add-Package /Image:C:\mount\windows /PackagePath:"E:\updates\KB3118754\x86\NEU\Windows10.0-KB3118754-x86.msu"

#### Reinstall inbox apps

When you add language packs, you should reinstall the inbox apps by removing each app and then installing it again by using DISM. The following procedure shows you how to reinstall the Get Started inbox app, but the steps will work for all apps by substituting the appropriate package.

Please verify on [SOC (Software  Order Center)](https://www.microsoftoem.com) the latest version of the "Windows Desktop OPK Supplemental" package.

1.  Right-click each folder and extract all to e:\apps.

    ![Extract application files](images/extract-app-files.png)

Remove the Get Started inbox app (Example)

    Dism /Image:"c:\mount\windows" /Remove-ProvisionedAppxPackage /PackageName: Microsoft.Getstarted_2015.522.28.1146_neutral_~_8wekyb3d8bbwe

Install the apps

**Note: There are 27 in-box apps to re-install into the image. Use the list below to identify the apps to re-apply to the image. If a Windows 10 supplemental update does not contain all 27 apps, install the remaining apps from the previous Windows 10 supplemental OPK. **

[Desktop_2015.1071.40.0_Microsoft.Camera.appxbundle_Windows10_PreinstallKit]

[Desktop_Builder3D_10.9.50.0_x86_x64.appxbundle_Windows10_PreinstallKit]

[Desktop_Mobile_x86_ARM_1.10.26007.0_MicrosoftMessaging.appxbundle_Windows10_PreinstallKit]

[Desktop_x86_x64_10.1510.9010.0_WindowsPhone.appxbundle_Windows10_PreinstallKit]

[Desktop_x86_x64_15.1001.16470.0_WindowsPhotos.appxbundle_Windows10_PreinstallKit]

[Desktop_x86_x64_3.6.13281.0_Music_Desktop_Production.appxbundle_Windows10_PreinstallKit]

[Desktop_x86_x64_3.6.13571.0_Video_Desktop_Production.appxbundle_Windows10_PreinstallKit]

[Desktop_x86_x64_ARM_10.0.2840.0_MicrosoftPeople.appxbundle_Windows10_PreinstallKit]

[Desktop_x86_x64_ARM_10.1510.13110.0_SoundRecorder.appxbundle_Windows10_PreinstallKit]

[GetSkype_3.2.1.0_x86_bundle.appxupload_Windows10_PreinstallKit]

[Microsoft.ConnectivityStore_8wekyb3d8bbwe.appxbundle_Windows10_PreinstallKit]

[Microsoft.WindowsStore_8wekyb3d8bbwe_2015.1013.14.0.1509.Universal.appxbundle_Windows10_PreinstallKit]

[Mobile_Desktop_x86_x64_ARM_1.10.23004.0_CommsPhone.appxbundle_Windows10_PreinstallKit]

[MoneyApp_4.6.169.0_x86.appxbundle_Windows10_PreinstallKit]

[News_4.6.169.0_x86.appxbundle_Windows10_PreinstallKit]

[PC_storeandTH2_6314.2375.officehubim.appxbundle_Windows10_PreinstallKit]

[PC_Sway_6216.2025.storyim.appxbundle_Windows10_PreinstallKit]

[PC_TH2RC_store.16.0.6131.1005.onenoteim.appxbundle_Windows10_PreinstallKit]

[PC_TH2_store.16.0.6308.4227.Sc6131.1009.outlookim.appxbundle_Windows10_PreinstallKit]

[Solitaire_3.4.9241.0_x86_x64.appxbundle_Windows10_PreinstallKit]

[Universal_Maps.Windows_4.1509.50911.0_ARM_x64_x86.appxbundle_Windows10_PreinstallKit]

[Universal_Sports_4.6.169.0_x86.appxbundle_Windows10_PreinstallKit]

[Universal_Weather_4.6.169.0_x86.appxbundle_Windows10_PreinstallKit]

[Universal_x86_x64_ARM_10.1510.13020_WindowsCalculator.appxbundle_Windows10_PreinstallKit]

[Universal_x86_x64_ARM_10.1510.14020.0_WindowsAlarms.appxbundle_Windows10_PreinstallKit]

[Universal_x86_x64_ARM_2.4.13.0_GetStarted.appxbundle_Windows10_PreinstallKit]

[XboxApp_9.9.30030.0]

Important: The appx bundles must install the matching dependency packages or apps will fail to work after OOBE. The correct dependency packages are defined in the \*.provxml files in the app folders. The following example has the correct dependency packages for each app:

    dism /image:"c:\mount\windows" /Add-ProvisionedAppxPackage /PackagePath:"E:\apps\Universal_Microsoft.GetStarted_2.2.7.0_8wekyb3d8bbwe.appxbundle_Windows10_PreinstallKit\aed1db6c4a954880b3ff43b8e4d1a76d.appxbundle" /DependencyPackagePath:"E:\Apps\Universal_Microsoft.GetStarted_2.2.7.0_8wekyb3d8bbwe.appxbundle_Windows10_PreinstallKit\Microsoft.NET.Native.Framework.1.0_1.0.22929.0_ARM__8wekyb3d8bbwe.appx" /DependencyPackagePath:"E:\Apps\Universal_Microsoft.GetStarted_2.2.7.0_8wekyb3d8bbwe.appxbundle_Windows10_PreinstallKit\Microsoft.NET.Native.Runtime.1.0_1.0.22929.0_ARM__8wekyb3d8bbwe.appx" /DependencyPackagePath:"E:\Apps\Universal_Microsoft.GetStarted_2.2.7.0_8wekyb3d8bbwe.appxbundle_Windows10_PreinstallKit\Microsoft.VCLibs.140.00_14.0.22929.0_ARM__8wekyb3d8bbwe.appx" /licensepath:"E:\apps\Universal_Microsoft.GetStarted_2.2.7.0_8wekyb3d8bbwe.appxbundle_Windows10_PreinstallKit\aed1db6c4a954880b3ff43b8e4d1a76d_License1.xml"

#### Adding Windows Universal Office Mobile

Obtain X20-88613 Office Mobile MultiLang v1.2 OPK (This is the latest update when this guide is published. Please verify on [www.microsoftoem.com](http://www.microsoftoem.com) > SOC (Software Order Center) if a newer version of update packages exits.

Please refer to [Office Mobile Communication](https://myoem.microsoft.com/oem/myoem/en/product/office/Pages/COMM-OfficeUnvrslAppsOPKRlsTmng.aspx) for more details.

Note: Microsoft Office Single image installation is covered in the section [Preload Microsoft Office Single Image v15.4](#preload-microsoft-office-single-image-v15.4) for devices with screen size above 10.1".

Pre-install Office single image (either with or with out perpetual or subscription license) or Office Mobile.  Office Mobile must be used on devices with screen size of 10.1" and below, and Office single image must be used on devices with screen sizes above 10.1". For devices that have a single fixed storage drive with less than 32 GB, OEMs may preinstall Office Mobile, regardless of the screen size. OEMs must have only one Office image on the Customer System at a time.

1.  Extract all folders to E:\Universal_Office

    ![Extract Office](images/extract-office.png)

        dism /image:"c:\mount\windows" /add-provisionedappxpackage /packagepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Excelim.appxbundle_Windows10_PreinstallKit\1b0569bd5fbd41d6bf0669beb013073c.appxbundle" /dependencypackagepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Excelim.appxbundle_Windows10_PreinstallKit\Microsoft.VCLibs.140.00_14.0.22929.0_x86__8wekyb3d8bbwe.appx" /licensepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Excelim.appxbundle_Windows10_PreinstallKit\1b0569bd5fbd41d6bf0669beb013073c_License1.xml"

        dism /image:"c:\mount\windows" /add-provisionedappxpackage /packagepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Pptim.appxbundle_Windows10_PreinstallKit\7f255062294a415a974b4958961df056.appxbundle" /dependencypackagepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Pptim.appxbundle_Windows10_PreinstallKit\Microsoft.VCLibs.140.00_14.0.22929.0_x86__8wekyb3d8bbwe.appx" /licensepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Pptim.appxbundle_Windows10_PreinstallKit\7f255062294a415a974b4958961df056_License1.xml"

        dism /image:"c:\mount\windows" /add-provisionedappxpackage /packagepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Wordim.appxbundle_Windows10_PreinstallKit\532f710ca9d34f0aae6af4abe0af0592.appxbundle" /dependencypackagepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Wordim.appxbundle_Windows10_PreinstallKit\Microsoft.VCLibs.140.00_14.0.22929.0_x86__8wekyb3d8bbwe.appx" /licensepath:"e:\Universal_office\PC_TH1_store.16.0.6228.1011.Wordim.appxbundle_Windows10_PreinstallKit\532f710ca9d34f0aae6af4abe0af0592_License1.xml"

#### Modify the Start layout

The Start tile layout in Windows 10 provides OEMs the ability to append tiles to the default Start layout to include Web links, secondary tiles, classic Windows applications, and universal Windows apps. OEMs can use this layout to make it applicable to multiple regions or markets without duplicating a lot of the work. In addition, OEMs can add up to three default apps to the frequently used apps section in the system area, which delivers sytem-driven lists to the user, including important or frequently accessed system locations and recently installed apps.

To take advantage of all these new features and have the most robust and complete Start customization experience for Windows 10, consider creating a LayoutModification.xml file. This file specifies how the OEM tiles should be laid out in Start. For more information about how to customize the new Start layout, see [Customize the Windows 10 Start screen](https://msdn.microsoft.com/library/windows/hardware/Mt170651.aspx).

1.  Create Layoutmodification.xml.

    Note: It is recommended to start with the sample on USB-B\StartLayout\layoutModification.xml as it conforms to the samples in this document (example only).

    The Sample LayoutModification.xml shows two groups called “Fabrikam Group 1" and “Fabrikam Group 2”, which contain tiles that will be applied if the device country/region matches what’s specified in Region (in this case, the regions are Germany and United States). Each group contains three tiles and the various elements you need to use depending on the tile that you want to pin to Start.

    Keep the following in mind when creating your LayoutModification.xml file:

    -   If you are pinning a Classic Windows application using the **start:DesktopApplicationTile** tag and you don’t know the application’s application user model ID, you need to create a .lnk file in a legacy Start Menu directory before first boot.

    -   If you use the **start:DesktopApplicationTile** tag to pin a legacy .url shortcut to Start, you must create a .url file and add this file to a legacy Start Menu directory before first boot.

    For those scenarios, you can use the following directories to put the .url or .lnk files:

    -   %APPDATA%\Microsoft\Windows\Start Menu\Programs\

    -   %ALLUSERSPROFILE%\Microsoft\Windows\Start Menu\Programs\

1.  Save the LayoutModification.xml file.

2.  Add your LayoutModification.xml file to the Windows image. You’ll need to put the file in the following specific location before first boot. If the file exists, you should replace the LayoutModification.XML that is already included in the image.

        Copy E:\StartLayout\layoutmodification.xml c:\mount\windows\users\default\AppData\Local\Microsoft\Windows\Shell\

    Where E: is the drive letter of **USB-B**.

1.  If you pinned tiles that require .url or .lnk files, add the files to the following legacy Start Menu directories :

    1.  %APPDATA%\Microsoft\Windows\Start Menu\Programs\

    2.  %ALLUSERSPROFILE%\Microsoft\Windows\Start Menu\Programs\

            Copy e:\StartLayout\Bing.url "C:\mount\windows\ProgramData\Microsoft\Windows\Start Menu\Programs\"

            copy e:\StartLayout\Paint.lnk "c:\mount\windows\ProgramData\Microsoft\Windows\Start Menu\Programs"

            Copy E:\StartLayout\Bing.url "c:\mount\windows\users\All Users\Microsoft\Windows\Start Menu\Programs"

            Copy E:\StartLayout\Paint.lnk "C:\Mount\Windows\Users\All Users\Microsoft\Windows\Start Menu\Programs"

Note: If you don’t create a LayoutModification.xml file and you continue to use the Start Unattend settings, the OS will use the Unattend answer file and take the first 12 SquareTiles or DesktoporSquareTiles settings specified in the Unattend file. The system then places these tiles automatically within the newly-created groups at the end of Start—the first six tiles are placed in the first OEM group and the second set of six tiles are placed in the second OEM group. If OEMName is specified in the Unattend file, the value for this element is used to name the OEM groups that will be created.

##### Add an OEM-specific license

1.  An OEM may add its OEM license terms to the License Terms screen in the first experience. 

    Note: If the license terms are included, the OEM must include a version of the license terms in each language that is preinstalled onto the PC. A license term text must be an .**rtf** file, saved as .**rtf** format.

1.  Create folders under the following directory: 

    C:\mount\windows\Windows\System32\oobe\info\default\

2.  Name each folder under C:\mount\windows\Windows\System32\oobe\info\default\ directory as the **Language Decimal Identifier** corresponding the language. Do this step for each language pack added to the Windows image.

    For the complete list of language decimal identifiers of corresponding languages, see [Available Language Packs for Windows](available-language-packs-for-windows.md).

    For example, if en-us and de-de language packs are added to the Windows image, add a folder named “1033” (representing en-us language) under C:\mount\windows\Windows\System32\oobe\info\default\. Then add a folder named “1031” (representing de-de language) under the same directory.

        MD c:\mount\windows\windows\system32\oobe\info\default\1033

        MD c:\mount\windows\windows\system32\oobe\info\default\1031

1.  Create license term document for each language specified. Move each license term document to the corresponding language folder.

    For example, move the agreement.rtf file **in English** to:
    
    C:\mount\windows\Windows\System32\oobe\info\default\\**1033**\  
    
    And move the agreement.rtf file **in German** to: 
    
    C:\mount\windows\Windows\System32\oobe\info\default\\**1031**\ 

        Copy E:\resources\agreement.rtf c:\mount\windows\windows\system32\oobe\info\default\1033

1.  Create an **oobe.xml** file to specify the agreement.rtf file path. In the following image, you can see a sample oobe.xml which is located in **USB-B**\ConfigSet\oobe.xml destination.

    ![Sample OOBE](images/sample-oobe.png)

2.  Copy **oobe.xml file** to each language folder.

    For example, copy oobe.xml to C:\mount\windows\Windows\System32\oobe\info\default\\**1033**\ where the agreement.rtf in English is valid and to C:\mount\windows\Windows\System32\oobe\info\default\\**1031**\ directory where the agreement.rtf in German is valid.

        Copy e:\configset\oobe.xml c:\mount\windows\windows\system32\oobe\info\default\1033

1.  Finally, each language folder must contain an **oobe.xml** file and an agreement.rtf file in that corresponding language.

    ![Agreement and OOBE files](images/agreement-and-oobe-files.png)
    
    
#### Modify the answer file

1.  The OEM may wish to create a new answer file. The following sample answer file covers most the required settings for [Windows OEM Policy Document (OPD)](http://click.email.microsoftemail.com/?qs=4e5762cd9d90bfec0d11dfa9bca1a9efd4924672c363af092f176011a77178f6f1caa72260b9766a). Therefore it is recommended to use this answer file:

    <table>
    <tr>
    <td>For OA 3.0 systems: 
    </td>
    <td><code>Copy /y E:\AnswerFiles\OA3.0\Unattend.xml C:\Mount\Windows\Windows\Panther</code><p>(where E:\ is **USB-B**)</p>
    </td>
    </tr>
    <tr>
    <td>For non-OA 3.0 systems: 
    </td>
    <td><code>Copy /y E:\AnswerFiles\Non_OA3.0\Unattend.xml C:\Mount\Windows\Windows\Panther</code><p>(where E:\ is **USB-B**)</p>
    </td>
    </tr>
    </table>

#### Optimize WinRE

1.  Increase scratchspace size.

        Dism /image:"c:\mount\winre" /set-scratchspace:512

1.  Cleanup unused files and reduce size of winre.wim.

        dism /image:"c:\mount\winre" /Cleanup-Image /StartComponentCleanup /Resetbase 

#### Unmount the Image

1.  Close all applications that might access files from the image.

2.  Comit the changes and unmount the Windows RE image:

        Dism /Unmount-Image /MountDir:"C:\mount\winre" /Commit

    where C is the drive letter of the drive that contains the image.

    This process can take a few minutes.

1.  Make a backup copy of the updated Windows RE image:

    Troubleshoot: If you cannot see winre.wim under the specified directory, use the following command to make the file visible:

        attrib -h -a -s C:\mount\windows\Windows\System32\Recovery\winre.wim

        dism /export-image /sourceimagefile:c:\mount\windows\windows\system32\recovery\winre.wim /sourceindex:1 /DestinationImageFile:e:\images\winre_bak.wim

        Del c:\mount\windows\windows\system32\recovery\winre.wim

        Copy e:\images\winre_bak.wim c:\mount\windows\windows\system32\recovery\winre.wim

    When prompted, specify "F" for file.

1.  Check the new size of the Windows RE image.

        Dir "C:\mount\windows\Windows\System32\Recovery\winre.wim"

    Use the following partition layout size guidance to determine the size of your recovery partition in createartitions-&lt;firmware&gt;.txt files. The amount of free space left is after you copy winre.wim to the hidden partition.

    Please reference [Disk Partition rules](configure-uefigpt-based-hard-drive-partitions.md#DiskPartitionRules) for more information.

    -   If the partition is less than 500 MB, it must have at least 50 MB of free space.

    -   If the partition is 500 MB or larger, it must have at least 320 MB of free space.

    -   If the partition is larger than 1 GB, we recommend that it should have at least 1 GB free.

            rem == Windows RE tools partition =============== 
            create partition primary size=500

    Optional: This section assumes you’d rather keep winre.wim inside of install.wim to keep your languages and drivers in sync. If you’d like to save a bit of time on the factory floor, and if you’re OK managing these images separately, you may prefer to pull winre.wim from the image and apply it separately.

1.  Commit the changes and unmount the Windows image:

        Dism /Unmount-Image /MountDir:"C:\mount\windows" /Commit

    where C is the drive letter of the drive that contains the image.

This process may take several minutes.

### Deploy the image to new computers

In this section, the device is prepared for deployment by booting into WinPE, creating a partition layout, and deploying the image.

#### Boot to WinPE

1.  On the technician computer, locate the following files in **USB-B**/Deployment destination. Please see [Creating My USB-B](#creating-my-usb-b) to create and place the files in the correct paths. Skip this step if it was done previously.

2.  Connect the **USB-A** drive and boot the reference computer.

3.  After WinPE has been booted, connect **USB-B**.

4.  At the **X:\Windows\system32&gt;** command line,type ***diskpart*** and press &lt;Enter&gt; to start Diskpart.

5.  At the **\DISKPART&gt;** command line, type ***list volume***.

6.  Under the “*Label”* column, identify the **USB-B** drive and note the letter of the volume under the “*Ltr”* column (for example, E).

7.  Type ***exit*** to quit Diskpart.

#### Deploy the image

Using the deployment script walkthrough-deploy.bat in **USB-B**/Deployment folder, lay out the partitions on the device and apply the image. 

**Important: The Recovery partition must be the partition after the Windows partition to ensure winre.wim can be kept up-to-date during life of the device.**

In Windows 10 Version 1511, we are changing our recommendation to have the WinRE partition placed after the OS partition. This allows future growth of the WinRE partition during updates. Today with the WinRE partition at the front of the disk, the size of it can never be changed, making it difficult to update WinRE when needed. We will continue to support having the WinRE partition located in different parts of the disk, but we encourage you to follow the new recommendation.

    E:\Deployment\walkthrough-deploy.bat E:\Images\BasicImage.wim
    

There are several pauses in the script. You will be prompted Y/N for the Apply operation if this is a Compact OS deployment.

Note: Only use Compact OS on Flash drive based devices because Compact OS performance depends heavily on the storage device capabilities. Compact OS is NOT recommended on rotational devices. For more information, see [Compact OS](compact-os.md).

After the computer boots to the OOBE screen, press this key combination to boot into Audit mode:

    Ctrl+Shift+F3

### Preload Microsoft Office

This section provides guidance for pre-loading Office 2016 and Office v15.4.

#### Preload Microsoft Office 2016

This guide provides information for licensed original equipment manufacturers (OEMs) about how to use the Office Deployment Tool to preinstall Office 2016 on to devices that are running the Windows operating system.

Note: This guide doesn’t cover the PIPC scenarios for OEMs in Japan. 

##### Prepare Office files on Technician PC

Obtain Office Deployment Tool from X20-92403 Office 2016 v16 Deployment tool for OEM OPK.

1.  Mount X20-92403 Office 2016 v16 Deployment Tool for OEM OPK\Software - DVD\X20-92404 SW DVD5 Office 2016 v16 Deployment Tool for OEM\x20-92404.img.
2.	Copy files from mounted drive to USB-B (where E:\ is driver letter for USB-B) E:\OfficeV16.
3.	Double click e:\Officev16\officedeploymenttool.exe.
4.	Provide folder path to extract files E:\Officev16.

    Setup.exe and configuration.xml are extracted to E:\Officev16.

    ![Setup and configuration.xml](images/setup-and-configuation.png)
    
    Obtain Office v16 in the desired language; this sample uses English X20-39283 Office 2016 v16 32-BIT X64 English OPK.
    
5. Copy the folder Office from mounted drive X20-39283 Office 2016 v16 32-BIT X64 English OPK\Software - DVD\X20-37728 SW DVD5 Office Pro 2016 32 64 English C2ROPK Pro HS HB OEM\X20-37728.img to USB-B (where E:\ is drive letter for USB-B) E:\OfficeV16.

    ![Office folder](images/office-folder.png)
    
    [Optional] If you applied a language interface pack, you may want to add the language interface pack for Office 2016 as well. The below samples will show with the Language interface pack applied.    

6. Notepad E:\Officev16\ConfigureO365Home.xml

7. Add language ID and verify SourcePath as in the following screenshot.

    ![Language ID](images/language-id.png)
    
8. Close and save ConfigureO365Home.xml.

9. Open an elevated command prompt as administrator.

10. From E:\Officev16, type and run setup.exe /download ConfigureO365Home.xml:

    CD E:\Officev16
    Setup.exe /download ConfigureO365Home.xml
    
    This will download the language packs for German and Japanese.
    
11. Type and run echo %errorlevel% and verify return code is 0.

12. Unplug USB-B from the technician computer. 

##### Install Office 2016 on Reference PC

1. Plug USB-B into the reference computer, which is in Audit mode.
2.	Find the drive letter for USB-B; for this example USB-B is E:\.
3.	Notepad ConfigureO365Home.xml.
4.	Configure the SourcePath to point to USB-B E:\Officev16.

    ![Configure the source path](images/configure-source-path.png)
    
    Note: the only Product ID that needs to be specified in the configuration.xml file is O365HomePremRetail. If the user enters a key for another product, such as for Office Home & Student 2016, then Office will automatically be configured as the product associated with that key.
    
5.	Close and Save ConfigureO365Home.xml.
6.	Open a command prompt and navigate to d:\Officev16.
7.  Type:

    Setup.exe /configure ConfigureO365Home.xml

##### Pin Office tiles to the Start menu

You must pin the Office tiles to the Start menu, otherwise Windows will remove the Office files during OOBE boot phase.

Note: You must be using at least version 10.0.10586.0 of Windows 10. The following steps don’t work with earlier versions of Windows 10.

1. Open a command prompt and type:

        notepad C:\Users\Default\AppData\Local\Microsoft\Windows\Shell\layoutmodification.xml.
        
2. Add &lt;AppendOfficeSuiteChoice Choice="Desktop2016" /&gt; to layoutmodification as you see highlighted in the following example:

    ![Layout Modification](images/layoutmodification.png)

    Note: The Choice attribute is new. This allows different versions of Office to be pinned to the Start screen at the same time. For now, Desktop2016 is the only valid value. Other values will be available in the future.

3. Close and save layoutmodification.xml.

    Note: for recovery purposes the layoutmodification.xml will need to be copied during recovery. 
    
4.  Open a command prompt and type:

        copy C:\Users\Default\AppData\Local\Microsoft\Windows\Shell\layoutmodification.xml c:\recovery\OEM   

    Once the machine is booted to desktop after going through OOBE, the Start menu will have these three tiles appended as shown in the following diagram: 
    
    ![Office tiles pinned to the Start menu](images/office-tiles-pinned-to-start-menu.png)
    
#####  Configure the Setup experience for the user   

After you install Office on the device, you also need to configure the Setup experience for the user. This is the experience the user sees when they open an Office app for the first time on the device. This also is intended to ensure that Office is properly licensed and activated.

|   Setup mode  | Description   |
|---------------|---------------|
|   OEM         | In this mode, a customer can choose to try, buy, or activate Office with an existing account, PIN, or product key. This mode doesn’t support Activation for Office (AFO) or AFO late binding. Therefore, if you choose this mode, you need to provide the customer with an Activation Card (formerly called a product key card or a Microsoft Product Identifier (MPI) card). |
|   OEMTA       |   This mode supports the try, buy, or activate experience of the OEM mode as well as supporting AFO and AFO late binding. This mode supports Office activation through the device’s Windows product key, which means the customer wouldn’t need to enter a 5x5 product key code. |


OEM Mode – Provide user with activation card
1.	In command prompt go to drive letter for USB-B\Officev16
2.	Type and run: 

    oemsetup.cmd Mode=OEM

OEMTA Mode – Activation is done through the device’s Windows product key.

1. Type and run:
 
    oemsetup.cmd Mode=OEMTA Referral=####

#### Preload Microsoft Office Single Image v15.4

The current preload method of Office 2013 is different from other desktop apps. OEMs preinstall desktop apps so the apps are installed. This means that when the user launches the app, the app is already installed and automatically opens with no additional installation tasks other than EULA acceptance and/or user registration. With Office, compressed setup files are copied to the disk in addition to an out-of-box experience (OOBE) application which captures Office file associations and gives users an entry point to Try, Buy or Activate Office. Users do not have access to Word or Excel on the Start screen or in the All Apps view until they launch Office OOBE which is typically made available as a tile on the Start screen. When the user launches Office OOBE, setup of Office begins. Once Office setup is complete, the compressed setup files that are no longer necessary are removed from the disk.

-   Pre-install Office single image (either with or without perpetual or subscription license) or Office Mobile.  Office Mobile must be used on devices with screen size of 10.1" and below, and Office single image must be used on devices with screen sizes above 10.1". For devices that have a single fixed storage drive with less than 32 GB, OEMs may preinstall Office Mobile, regardless of the screen size. OEMs must have only one Office image on the customer system at a time.

-   Pin Office tiles to the Start Menu in such slots that are visible without scrolling, regardless of OEM Systems’ Start Menu layout, unless the Office Tiles or Office tile, as applicable, are pinned as part of the Microsoft Group of Start Menu tiles.

Obtain: Office Single Image  OPK for language of your region. For this document, we will use en-us OPK for sample OPK X19-96440 Office 2013 Single Image v15.4 english OPK.  This is the latest Office OPK when this guide is published. Please verify on SOC ([https://www.microsoftoem.com](https://www.microsoftoem.com) &gt; Software  Order Center) if a newer version of Office Single Image OPK exists.

Note: If Office Tiles are automatically pinned as part of the Microsoft Group of Start Menu tiles, skip the entire following section about tile replacement.

-   For Office single image v15, pin the Office tile as medium sized tile or larger.

-   For Office Mobile or Office single image v15 successor, pin 3 Office tiles (Word, Excel, PowerPoint tiles) as small sized tiles or larger in a 2x2 orientation.

<table width="462" border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="110" valign="top">
            </td>
            <td width="167" valign="top">
                <p>
                    Windows 8, 8.1
                </p>
            </td>
            <td width="185" valign="top">
                <p>
                    Windows 10
                </p>
            </td>
        </tr>
        <tr>
            <td width="110" valign="top">
                <p>
                    Single Image
                </p>
            </td>
            <td width="167" valign="top">
                <p>
                    Version 15 = 1 tile
                </p>
                <p>
                    Next version = 3 tiles
                </p>
            </td>
            <td width="185" valign="top">
                <p>
                    Version 15 = 1 tile
                </p>
                <p>
                    Next version = 3 tiles
                </p>
            </td>
        </tr>
        <tr>
            <td width="110" valign="top">
                <p>
                    Office "universal" apps
                </p>
            </td>
            <td width="167" valign="top">
                <p>
                    N/A
                </p>
            </td>
            <td width="185" valign="top">
                <p>
                    3 tiles
                </p>
            </td>
        </tr>
    </tbody>
</table>

3 tiles pinning examples                   

![4 tiles](images/four-tiles-pinning.png) ![3 tiles](images/three-tiles-pinning.png) 

1 tile pinning example

![1 tile](images/one-tile-pinning.png)

1.  Multiple language versions of Office Single Image v15.4 OPK may be preloaded. Download all the Microsoft Office Single Image v15.4 OPK languages which are relevant to the languages added to Windows image. For example, if English and French language packs are added to Windows image please download:

    -   Office 2013 32 64-bit Single Image v15.4 **English** OPK

    -   Office 2013 32 64-bit Single Image v15.4 **French** OPK

    Note: When different languages of Office get installed consecutively, each additional Office having the different language will occupy ~300MB of disk space instead of ~1GB. For example, when Office15 English and Office15 French get installed, total disk space required will be ~1.3GB instead of ~2GB.

1.  Open an elevated command prompt (with administrative permissions).

2.  Copy the content of the OPK to a directory, which will be the OfficeSingleImagev15.4 InstallationDirectory.

3.  Navigate to the installation directory. The installation directory is the folder that contains the files shown in the following figure:

    ![](images/installation-directory.png)
    
    For example: Cd C:\&lt;OfficeSingleImagev15.4 InstallationDirectory&gt;

    Note: The installation process for the OPK is the same for computers that run 32-bit operating systems or 64-bit operating systems. The 32-bit version of Office 2013 may be preloaded on computers that run either 32-bit or 64-bit operating systems. The 64-bit version of Office 2013 can be preloaded only on computers that run 64-bit operating systems. To prevent possible compatibility issues with add-ins or third-party applications, preload *only the 32-bit version* of Office Single Image on both 32-bit and 64-bit computers.

1.  Start the installation by running the office installation script.

    Office Single image install:

        Oemsetup.en-us.cmd

    Office Single image install with AFO (Activation for Office)

        Oemsetup.en-us.oa30.cmd

After the process is completed, the Microsoft Office application tile will be placed on the Start screen. However, the user can install only one language version of Office Single Image v15.4. By default, the language version of the OOBE application matches the Windows language settings and the Office Single Image v15.1 language that is preloaded on the computer. If this match doesn’t take place, the language dialog box will contain languages that are based on the Office 2013 preloaded languages.

### Prepare system for recovery push-button reset scenarios

Push-button reset, first introduced in Windows 8, is a built-in recovery tool that allows users to recover the OS while preserving their data and important customizations, without having to back-up their data in advance. It reduces the need for custom recovery applications by providing users with more recovery options and the ability to fix their own PCs with confidence.

In Windows 10, the Push-button reset features have been updated to include the following improvements:

-   **Image-less recovery**

    Push-button reset features no longer require or support a separate recovery image on a local partition or on media. This significantly reduces the disk space needed to support the features, and makes recovery possible even on devices with limited storage capacity.

-   **Recovers to an updated state**

Push-button reset features now recover the Operating System (OS) and drivers (including device applets that are installed as part of INF-based driver packages) to an updated state. This reduces the amount of time users have to spend reinstalling the OS updates and drivers after performing a recovery.

The Push-button reset user experience continues to offer customization opportunities. Manufacturers can insert custom scripts, install applications or preserve additional data at available extensibility points.

The following Push-button reset features are available to users with Windows 10 PCs:

-   **Refresh your PC**

    Fixes software problems by reinstalling the OS while preserving the user data, user accounts, and important settings. All other preinstalled customizations are restored to their factory state. In Windows 10, this feature no longer preserves user-acquired Universal Windows apps1.

-   **Reset your PC**

    Prepares the PC for recycling or for transfer of ownership by reinstalling the OS, removing all user accounts and contents (e.g. data, Classic Windows applications, and Universal Windows apps), and restoring preinstalled customizations to their factory state.

-   **Bare metal recovery**

    Restores the default or preconfigured partition layout on the system disk, and reinstalls the OS and preinstalled customizations from external media.

For more information, see:
- [Push-button reset](push-button-reset-overview.md)
- [Windows Recovery Environment (Windows RE)](windows-recovery-environment--windows-re--technical-reference.md)
- [Hard Drives and Partitions](hard-drives-and-partitions.md)

#### Prepare ScanState tool 

Prepare ScanState tool to capture Classic Windows applications after they have been installed and additional settings like registry.

Note: You will do this on the technician system.

1.  On Technician PC, Insert USB-B.

    If you use an **x64** Windows 10 image:

        md E:\ScanState_amd64
        copy "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\User State Migration Tool\amd64" E:\ScanState_amd64
        copy /Y "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Setup\amd64\Sources" E:\ScanState_amd64
        
    If you use an **x86** Windows 10 image:
    
        md E:\ScanState_x86
        copy "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\User State Migration Tool\x86" E:\ScanState_x86
        copy /Y "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Setup\x86\Sources" E:\ScanState_x86

    Where E: is the USB-B drive letter.

#### Create ScanState configuration file

OEM can use a configuration file to restore and exclude registry keys and files during the PBR process.

**Important: this section includes a workaround for a known issue in Windows 10. You must apply this workaround to avoid issues with the PBR process. **

In some instances, Windows Defender settings and detection history might be captured into the customizations package by the ScanState tool. This can lead to failures during recovery due to file conflicts, and causes the PC to reboot and enter the “Installing Windows” phase repeatedly.

Note: You may use the sample configuration file on USB-B\Recovery\recoveryimage\pbr_config.xml, which covers the steps below.

1.  Use the ScanState tool on the **reference PC running in Audit mode**.

    If you use an **x64** Windows 10 image:
    
        MD c:\Recovery\OEM
        E:\ScanState_amd64\ScanState.exe /apps /genconfig:C:\Recovery\OEM\pbr_config.xml
    
    If you use an **x86** Windows 10 image:
     
        MD c:\Recovery\OEM
        E:\ScanState_x86\ScanState.exe /apps /genconfig:C:\Recovery\OEM\pbr_config.xml
    

1.  Open the configuration file in notepad

        Notepad c:\Recovery\OEM\pbr_config.xml

1.  Search for the following lines in the configuration file:

    &lt;component displayname="Windows-Defender-AM-Sigs" migrate="yes"…

    &lt;component displayname="Windows-Defender-AM-Engine" migrate="yes"…

    &lt;component displayname="Security-Malware-Windows-Defender" migrate="yes"…

1.  Modify the "migrate" value from "yes" to "no" for each line. For example:

    &lt;component displayname="Windows-Defender-AM-Sigs" migrate="**no**"…

    &lt;component displayname="Windows-Defender-AM-Engine" migrate="**no**"…

    &lt;component displayname="Security-Malware-Windows-Defender" migrate="**no**"…

#### Create ScanState migration file

OEM may use a configuration file to restore registry keys and files.

Create a migration XML file used to restore registry values manually entered during manufacturing process. The sample below restores the OEMID registry value set earlier in this document.

Note: USB-B\recovery\recoveryimage\regrecover.xml sample already contains the registry values.

    <migration urlid="http://www.microsoft.com/migration/1.0/migxmlext/test">

     <component type="System" context="UserAndSystem">

       <displayName>OEMID</displayName>

       <role role="Settings">

       <rules>

         <include>

            <objectSet>

               <pattern type="Registry">HKLM\Software\Microsoft\Windows\CurrentVersion\Store [OEMID]</pattern>

            </objectSet>

         </include>

       </rules>

       </role>

     </component>
    
    </migration>

#### Create recovery package using ScanState

Use the ScanState tool to capture the installed customizations into a provisioning package, and save it in the folder c:\Recovery\customizations. This document uses the samples from **USB-B**\Recovery\RecoveryImage to create the scanstate package.

Important: The ScanState package used by PBR must be a .ppkg file stored in C:\Recovery\Customizations folder or PBR will not be able to restore the package.

1.  Create the recovery OEM folder and copy the contents of **USB-B**\Recovery\RecoveryImage.

        Copy E:\Recovery\recoveryimage c:\recovery\OEM

        Copy E:\StartLayout\layoutmodification.xml c:\recovery\OEM

1.  Run ScanState to gather app and customizations.

   If you use an **x64** Windows 10 image:
    
    '''syntax
    Mkdir c:\recovery\customizations
    E:\ScanState_amd64\scanstate.exe /apps /ppkg C:\Recovery\Customizations\apps.ppkg /i:c:\recovery\oem\regrecover.xml /config:C:\Recovery\OEM\pbr_config.xml /o /c /v:13 /l:C:\ScanState.log
    '''
    
   If you use an **x86** Windows 10 image:
    
    '''syntax
    Mkdir c:\recovery\customizations
    E:\ScanState_x86\scanstate.exe /apps /ppkg C:\Recovery\Customizations\apps.ppkg /i:c:\recovery\oem\regrecover.xml /config:C:\Recovery\OEM\pbr_config.xml /o /c /v:13 /l:C:\ScanState.log
    '''
    
   Where E: is the drive letter of **USB-B***

#### Create extensibility scripts to restore additional settings

You can customize the Push-button reset experience by configuring extensibility points. This enables you to run custom scripts, install additional applications, or preserve additional user, application, or registry data.

The sample script EnableCustomizations.cmd will be called during PBR and will do two things:

1.  Copy the unattend.xml file used for initial deployment to the \windows\panther folder.

2.  Copy the layoutmodification.xml to the system.

This will restore the additional layout settings from these two answer files during PBR.

**Important: Recovery scripts and unattend.xml must be copied to c:\Recovery\OEM folder for PBR to pickup and restore correctly.**

Copy unattend.xml files for restoring settings.

For OA 3.0 systems: 

    '''syntax
    Copy /y E:\AnswerFiles\OA3.0\Unattend.xml C:\Mount\Windows\Windows\Panther
    '''
    
For non-OA 3.0 systems:

    '''syntax
    Copy /y E:\AnswerFiles\Non_OA3.0\Unattend.xml C:\Mount\Windows\Windows\Panther
    '''
    
where E:\ is **USB-B**

#### Copy winre.wim backup

During the deployment the winre.wim file is moved. Before capturing the final image the backup winre.wim created in section 4.5.17 must be copied back otherwise the recovery environment will not work on the final image deployment.

    Copy e:\images\winre_bak.wim c:\windows\system32\recovery\winre.wim

### Finalize and capture the manufacturing image

1.  Delete installation folders and files that have been created of the preloaded applications which are for example, *C:\Office-SingleImagev15.4-Setup*. Existence of these folders may increase the size of .wim file when the image of Windows drive gets captured.

2.  If the SysPrep Tool is open, close it and open a command prompt as an Administrator.

3.  Generalize the image by using answerfile with additional settings.

    '''syntax
    C:\Windows\System32\Sysprep\sysprep /unattend:c:\recovery\oem\Unattend.xml /generalize /oobe /shutdown
    '''

1.  Connect "**USB-A**" and boot the Reference computer.

2.  After WinPE has been booted, connect **USB-B**.

    Troubleshoot: The reference system was shutdown. While turning on, if the system continues to boot from Internal HDD, Windows will enter the specialize pass and then the OOBE pass. In order to capture a generalized and stable image, none of the Windows passes must be completed. To fix this, we need to generalize the image again, and at the OOBE screen, press &lt;Ctrl&gt;+&lt;Shift&gt;+&lt;F3&gt;. The system restarts and boots in Audit mode. In Audit mode, Sysprep the system by using the OOBE Shutdown and Generalize switches, as explained previously. After the system reboots, make sure to boot from USB-A to WinPE. 

    If the system still boots with internal HDD, please make sure USB boot is prioritized instead of HDD boot. To do so, it may be necessary to enter the Reference Computer BIOS menu and adjust the boot priority order so that the USB Key is at the top of the list.

1.  Identify Windows Partition Drive letter.

    -   At the **X:\windows\system32&gt;** prompt, type ***diskpart*** and press the **&lt;Enter&gt;** key to start Diskpart.

    -   At the **\DISKPART&gt;** prompt type ***list volume***.

    -   Under the “Label” column, locate the volume that is labeled “Windows”.

    -   Note what letter it is has been assigned under the “Ltr” column (Example: C). This is the drive letter that needs to be used.

    -   Type ***exit*** to quit Diskpart.

        ![Diskpart](images/diskpart-v10.png)
        
#### [Compact OS limited storage devices only] Convert installed customizations

Only do this step if you are deploying to limited storage device. Single instance will impact the launch performance of some desktop applications.

Please reference [Compact OS](compact-os.md) for more information.

    DISM /Apply-CustomDataImage /CustomDataImage:C:\Recovery\Customizations\apps.ppkg /ImagePath:C:\ /SingleInstance

#### Reduce size of the manufacturing image

After the manufacturing image is ready, choose to reduce the size of the image by clearing up the SXS store using DISM to offline service the image. Switch to the Technician Computer and mount the image.

Important: By default, non-major updates (e.g. ZDPs, KB’s, LCUs) are not restored. To ensure that updates preinstalled during manufacturing are not discarded after recovery, they should be marked as permanent by using the /Cleanup-Image command in DISM with the /StartComponentCleanup and [/ResetBase [/Defer]] options. Updates marked as permanent are always restored during recovery. Running this is a must to retain updates applied during manufacturing in order for PBR to restore them in first 28 days.

    MD c:\scratchdir

    Dism /Cleanup-Image /Image:C:\ /StartComponentCleanup /resetbase /scratchdir:c:\scratchdir

    RD c:\scratchdir

#### Capture the final manufacturing image

Note: It is recommended to run dism operations using a cache directory. For this sample, we create a scratchdir on the USB-B key for temporary files. However, you may choose any hard drive has a large amount of available space to create a scratch directory.

    MD c:\scratchdir

    Dism /Capture-Image /CaptureDir:C:\ /ImageFile:E:\Images\FinalImage.wim /Name:"FinalImage" /scratchdir:c:\scratchdir

    RD c:\scratchdir

#### Deploy the final image for verification

Run the sample walkthrough-deploy.cmd script to deploy the finalimage.wim.

    E:\Deployment\walkthrough-deploy.cmd E:\Images\FinalImage.wim

### Verify image customizations and recovery

-   Computer restarts and starts. This may take a few minutes.

-   Computer will start in OOBE mode. Create a dummy user which will be deleted later.

-   Please verify your applications are added and offline customizations are valid.

    For example:

    -   Taskbar

    -   Pinned Apps

    -   Websites

    -   Desktop Wallpaper

    -   OEM Information

    -   OEM App ID

    -   Default Theme

    -   Applications start ok

Note: First boot to desktop may show duplicate tiles with no title. This is related to App Promotions for Windows 10.

**How App Promotions work**

Promoted Apps are installed right after OOBE on Windows 10 PCs. Two tiles will be downloaded apps and three tiles will be deep links to the Store. This mix can change over time. If the PC is not connected to the internet during OOBE, placeholder tiles will be used until the PC is connected.

![App promotions](images/app-promotions.png)

#### Verify recovery

1.  Verify that your customizations are restored after recovery, and that they continue to function by running the **Refresh your PC** and **Reset your PC** features from the following entry points:

    -   Settings

        1. From the Start Menu, click **Settings**.

        2. In the Settings app, click **Update & security**, and then click **Recovery**.

        3. Click **Get Started** under **Reset this PC** and follow the on-screen instructions.

    -   Windows RE

        1. From the **Choose an option** screen in Windows RE, click **Troubleshoot**.

        2. Click **Reset this PC** and then follow the on-screen instructions.

1.  Verify that recovery media can be created, and verify its functionality by running the bare metal recovery feature:

    1.  Launch **Create a recovery drive** from Control Panel.

    2.  Follow the on-screen instructions to create the USB recovery drive.

    3. Boot the PC from the USB recovery drive.

    4. From the **Choose an option** screen, click **Troubleshoot**.

    5. Click **Recover from a drive** and then follow the on-screen instructions.

Note: The Push-button reset UI has been redesigned in Windows 10. The **Keep my files** option in the UI now corresponds to the **Refresh your PC** feature, whereas the **Remove everything** option corresponds to the **Reset your PC** feature. Verify recovery media can be created.

### Final shipment

OEMs must power on the PC at least once, and allow the specialize configuration pass of Windows Setup to complete, before shipping the PC to customers.

The specialize configuration pass adds hardware-specific information to the PC and is complete when Windows OOBE appears.

Please reference [OEM Policy Documentation](https://myoem.microsoft.com/oem/myoem/en/topics/Licensing/roylicres/ost2016/Pages/COMM-Win10-OPD-RTM-Now-Avail.aspx).

### Create recovery media

Choose **Option 1** to create the recovery media the same as the manufacturing image. This is the default and recommended recovery media configuration option but keep in mind that your manufacturing image may be larger than 4.6GB size. If it is and you are using DVD as a recovery media, your recovery file size will exceed the DVD capacity.

#### Option 1: Create recovery media from custom manufacturing image

1.  On the Technician Computer, mount the manufacturing image which is to be used as the recovery media. Mount the final image.

        Dism /Mount-Wim /WimFile:E:\Images\FinalImage.wim /index:1 /MountDir:C:\mount\windows 
                                                                                            
        (where E:\ is the volume label of **USB-B**)      
        
1.  Create the Windows PE folder structure. Run Deployment and Imaging Tool Environment in elevated permissions and type in:

    If you use an **x64** Windows 10 image:
    
        copype amd64 C:\resetmedia
    
    If you use an **x86** Windows 10 image:
    
        copype x86 C:\resetmedia
    
1.  Replace the default Windows PE boot image (Boot.wim) with the Windows RE image of the mounted manufacturing image.

        xcopy C:\mount\windows\Windows\System32\Recovery\winre.wim C:\resetmedia \media\sources\boot.wim /H

1.  Unmount the Windows image.

        dism /Unmount-Image /MountDir:C:\mount\windows /Discard 

1.  Copy Windows image to Windows PE folder as being the recovery media image.

        copy E:\Images\FinalImage.wim C:\resetmedia \media\sources\install.wim 
                                                                               
        (where E:\ is the volume label of **USB-B**)  
        
1.  Run Deployment and Imaging Tool Environment and make an iso file.

        Makewinpemedia /iso C:\resetmedia C:\MyRecoveryImage.iso 

1.  After process has finished, right-click RecoveryImage.iso and select **Burn disc image**.

2.  This recovery media will boot to Windows 10 Recovery Screen:

    ![Recovery screen](images/recovery-screen.png)

For more information about creating a recovery image, see [Bare metal reset/recovery: create recovery media while deploying new devices](create-media-to-run-push-button-reset-features-s14.md).

#### Option 2: Create recovery media from the base Windows 10 OPK

Choose **Option 2** to create the recovery media from scratch by using the base Windows 10 OPK.

1.  Copy the base Windows 10 image (**USB-B**\myWindows) to a folder named “my_distribution” located under C:\.

        md C:\my_distribution                                                    
                                                                                
        xcopy /s E:\myWindows\*.\* C:\my_distribution\                        
                                                                                
        md C:\mount\boot                                                          
                                                                                
        md C:\mount\windows                                                       
                                                                                
        copy E:\Images\FinalImage.wim C:\my_distribution\sources\install.wim  
    
    (where E:\ is the volume label of **USB-B**)                             

##### Add Language Packs to Windows Setup

1.  In order to add language packs to Windows setup, language pack files must be added to both install.wim and winre.wim images of the targeted Windows distributon.

2.  Mount the second image (index 2) in Boot.wim to a local mount directory using the **Dism /Mount-Image** command. For example:

        Dism /mount-image /imagefile:C:\my_distribution\sources\boot.wim /index:2 /mountdir:C:\Mount\boot

1.  Add Windows PE Setup optional component and language packs into your mounted image using the **Dism /Add-Package** command for each language you want to support. Windows PE language packs are available in the Windows ADK. For example:

        Dism /image:C:\mount\boot /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\fr-fr\lp.cab"                        
                                                                                                                                                                                                                                 
        Dism /image:C:\mount\boot /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\fr-fr\WinPE-Setup_fr-fr.cab"         
                                                                                                                                                                                                                                 
        Dism /image:C:\mount\boot /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\fr-fr\WinPE-Setup-Client_fr-fr.cab"  

1.  For Japanese (ja-JP), Korean (ko-KR), and Chinese (zh-HK, zh-CN, zh-TW), you must add additional font support to your image. For example, to add Japanese font support, enter the following command. For example:

        Dism /image:C:\mount\boot /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-FontSupport-JA-JP.cab" 

1.  Recreate the Lang.ini file to reflect the additional language support using the **Dism /Gen-LangINI** command.

        Dism /image:C:\mount\boot /gen-langINI /distribution:C:\my_distribution 

1.  Change the Windows Setup default language by using DISM. For example:

        Dism /image:C:\mount\boot /Set-SetupUILang:fr-FR /distribution:C:\my_distribution 

1.  Save your changes back into the image using the **Dism /Unmount–Image /Commit** command.

        Dism /unmount-image /mountdir:C:\mount\boot /commit 

1.  If you added font support for Japanese (ja-JP), Korean (ko-KR), or Chinese (zh-HK, zh-CN, zh-TW) to the default boot.wim image, you must also add font support to the first image (index 1) in the Boot.wim file.

    Use the **Dism /Mount-Image** command to mount the first image (index 1) in the Boot.wim file to a local mount directory. For example:

        Md C:\mount\boot1                                                                                      
                                                                                                           
        Dism /mount-image /imagefile:C:\my_distribution\sources\boot.wim /index:1 /mountdir:C:\Mount\boot1  

1.  Add the same font support you added to the boot.wim default boot image in the previous step. For example, to add Japanese font support, enter the following command:

        Dism /image:C:\mount\boot1 /add-package /packagepath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-FontSupport-JA-JP.cab" 

1.  Save your changes back into the image using the **Dism /Unmount-Image /Commit** command:

        Dism /unmount-image /mountdir:C:\mount\boot1 /commit 

##### Create a Bootable DVD with oscdimg tool

1.  Start **Deployment and Imaging Tools** with administrator elevation.

2.  Run **oscdimg** tool with following parameters. For example:

        oscdimg -m -o -u2 -udfver102 -bootdata:2#p0,e,bc:\my_distribution\boot\etfsboot.com#pEF,e,bc:\my_distribution\efi\microsoft\boot\efisys.bin c:\my_distribution c:\myISOname.iso

       ![Oscdimg.exe](images/oscdimg.png)

1.  Burn the .iso file to a new DVD. This DVD will be your recovery media.

2.  This recovery media will boot to Windows setup to install Windows 10 in a regular fashion by deleting your files.

