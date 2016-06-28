---
author: Justinha
Description: 'Lab 2b: Windows desktop apps and data: capturing changes'
ms.assetid: 142bc507-64db-43dd-8432-4a19af3c568c
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: 'Lab 2b: Windows desktop apps and data: capturing changes'
---

# Lab 2b: Windows desktop apps and data: capturing changes


You can use audit mode to add Windows desktop applications and other data to the device. After doing this, we'll use the ScanState tool to capture these changes into a provisioning package so they can be used by the recovery tools.

We'll also show you how to generalize the image, which prepares the Windows files to be captured and applied to other devices.

## <span id="Step_1__Prepare_a_copy_of_ScanState"></span><span id="step_1__prepare_a_copy_of_scanstate"></span><span id="STEP_1__PREPARE_A_COPY_OF_SCANSTATE"></span>Step 1: Prepare a copy of ScanState


1.  On your technician PC, plug in another USB key or drive.
2.  In File Explorer, create a new folder on the USB key, for example: **D:\\ScanState\_x64**.
3.  Copy the files from **"C:\\Program Files (x86)\\Windows Kits\\10\\Assessment and Deployment Kit\\User State Migration Tool\\amd64"** into **D:\\ScanState\_x64**. You don't need to copy the subfolders.
4.  Copy the files from **"C:\\Program Files (x86)\\Windows Kits\\10\\Assessment and Deployment Kit\\Windows Setup\\amd64\\Sources"** into **D:\\ScanState\_x64**. There will be duplicate files, it's OK to skip copying these files. You don't need to copy the subfolders.

``` syntax
md C:\ScanState_amd64
xcopy /E "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\User State Migration Tool\amd64" D:\ScanState_x64
xcopy /E /Y "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Setup\amd64\Sources" D:\ScanState_x64
```

## <span id="Prepare_a_device_for_image_capture"></span><span id="prepare_a_device_for_image_capture"></span><span id="PREPARE_A_DEVICE_FOR_IMAGE_CAPTURE"></span>Prepare a device for image capture


**Get into audit mode**

1.  Boot up the reference device, if it's not already booted.
2.  If the device boots to the **Languages** or the **Get going fast** screen, press **Ctrl+Shift+F3** to enter Audit mode.
3.  In audit mode, the device reboots to the Desktop, and the System Preparation Tool (Sysprep) appears. Ignore Sysprep for now.

**Verify drivers and packages**

1.  Right-click the **Start** button, and select **Command Prompt (Admin)**.
2.  Verify that the drivers appear correctly:

    ``` syntax
    C:\Windows\System32\Dism /Get-Drivers /Online
    ```

    If you've added the sample driver, look for "Provider Name: DMI Test team"

3.  Verify that the packages appear correctly:

    ``` syntax
    C:\Windows\System32\Dism /Get-Packages /Online
    ```

    Review the resulting list of packages and verify that the list contains the package. For example:

    ``` syntax
    Package Identity : Microsoft-Windows-Client-LanguagePack  ...  fr-FR~10.0.10240.0
    State : Installed
    ```

## <span id="Step_3__Install_a_Classic_Windows_application"></span><span id="step_3__install_a_classic_windows_application"></span><span id="STEP_3__INSTALL_A_CLASSIC_WINDOWS_APPLICATION"></span>Step 3: Install a Classic Windows application


-   Install a Classic Windows application. For example, to install Office 2013, put in a USB key with the Office installation program, open File Explorer and navigate to **oemsetup.en-us.cmd**. To learn more, download the Office OPK Update image from the Office OPK Connect site.

## <span id="Step_4__Capture_your_changes_into_a_provisioning_package"></span><span id="step_4__capture_your_changes_into_a_provisioning_package"></span><span id="STEP_4__CAPTURE_YOUR_CHANGES_INTO_A_PROVISIONING_PACKAGE"></span>Step 4: Capture your changes into a provisioning package


1.  Capture the changes into the provisioning package, and save it on the hard drive:

    ``` syntax
    D:\ScanState_x64\scanstate.exe /apps /ppkg C:\Recovery\Customizations\usmt.ppkg /o /c /v:13 /l:C:\ScanState.log
    ```

    where *D* is the drive letter of the USB drive with ScanState.

    **Note**  Optional: Delete the ScanState logfile: `del C:\Scanstate.log`.

     

2.  Prepare the device for the end user: Right-click **Start**, select **Command Prompt (Admin)**, and from the command prompt, run the following command:

    ``` syntax
     
    C:\Windows\System32\Sysprep\sysprep /oobe /generalize /shutdown
    ```

    The Sysprep tool reseals the device. This process can take several minutes. After the process completes, the device shuts down automatically.

**Boot to Windows PE and prepare to capture**

1.  Plug in the lab USB key with Windows Preinstallation Environment (WinPE).
2.  Turn on the device and press the key that opens the boot-device selection menu for the device (for example, the **Esc** key or **Volume Up** key).

    Select the option in the firmware menus to boot to the USB flash drive.

    **Warning**   If Windows begins booting instead of Windows PE, you must generalize the device again before capturing the image: After Windows boots, press **Ctrl+Shift+F3** to enter audit mode. The device will reboot. Generalize the device again: `C:\Windows\System32\Sysprep\sysprep /oobe /generalize /shutdown`.

     

3.  Optional: speed up the optimization and image capture processes by setting the power scheme to High performance:

    ``` syntax
    powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
    ```

4.  Find the drive letters by using DiskPart:

    ``` syntax
    diskpart
    DISKPART> list volume
    DISKPART> exit
    ```

    For example, the drives can be lettered like this: *C* = Windows; *D* is the lab USB key, and *E* is an external hard drive.

    Note that some partitions might not receive a drive letter.

**Optimize the image to take up less drive space (optional)**

1.  Convert the Classic Windows application and files outside of the provisioning package into pointer files which reference the contents inside the provisioning package. This is known as single-instancing the image:

    ``` syntax
    DISM /Apply-CustomDataImage /CustomDataImage:C:\Recovery\Customizations\USMT.ppkg /ImagePath:C:\ /SingleInstance
    ```

    where *C* is the drive letter of the Windows partition.

    **Warning**  Do not put quotes with the /ImagePath:C:\\ option.

     

2.  Cleanup the Windows files:

    ``` syntax
    md temp

    DISM /Cleanup-Image /Image=C:\ /StartComponentCleanup /ResetBase /ScratchDir:C:\Temp
    ```

    where *C* is the drive letter of the Windows partition. If DISM /Resetbase is a long-running operation, you can specify the /Defer parameter with /Resetbase to defer any long-running cleanup operations to the next automatic maintenance. But we highly recommend you **only** use /Defer as an option in the factory where DISM /Resetbase requires more than 30 minutes to complete.

## <span id="Step_5__Capture_the_image"></span><span id="step_5__capture_the_image"></span><span id="STEP_5__CAPTURE_THE_IMAGE"></span>Step 5: Capture the image


-   Capture the image of the Windows partition, and then copy it to the external drive or share:

    ``` syntax
    dism /Capture-Image /CaptureDir:C:\ /ImageFile:"C:\WindowsWithOffice.wim" /Name:"French and desktop applications"
    ```

    where *C* is the drive letter of the Windows partition and *French and Desktop Applications* is the image name.

    The DISM tool captures the Windows partition into a new image file. This process can take several minutes.

    **Troubleshooting**: If you receive an: "A parameter is incorrect" error message when you try to capture or copy the file to the USB key, the file might be too large for the destination file system. Copy the file to a different drive that is formatted as NTFS.

## <span id="Step_6__Apply_the_image_to_a_new_device__optional__to_be_done_in_the_factory_process_"></span><span id="step_6__apply_the_image_to_a_new_device__optional__to_be_done_in_the_factory_process_"></span><span id="STEP_6__APPLY_THE_IMAGE_TO_A_NEW_DEVICE__OPTIONAL__TO_BE_DONE_IN_THE_FACTORY_PROCESS_"></span>Step 6: Apply the image to a new device (optional, to be done in the factory process)


This represents the steps you'd use in the factory. For the purposes of the lab, reuse the reference device for this.

**Prepare a place to store your files**

1.  For this step, you'll need another USB key, or an external hard drive or network location that can handle large files.
    -   To format a separate USB key to accept large files: On your technician PC, go to File Explorer, right-click the drive and select **Format**. Select File system: **NTFS**, accept all other defaults, and click **OK**.

        Then, connect the USB key to the reference device.

    -   To use a network file share: On your reference device, map a drive. Example: **net use N: \\\\server\\share**.
    -   **Note**  You won't be able to use the WinPE key. To make WinPE bootable for both UEFI and BIOS-based systems, WinPE formats the key using the FAT32 file system, which has a 4GB limit. For other ways to get around this limit, see [Split a Windows image file (.wim) for FAT32 media or to span across multiple DVDs](http://go.microsoft.com/fwlink/?LinkId=526946).

         

2.  Copy C:\\WindowsWithOffice.wim to the storage USB drive or network share.

Skip this step if the reference device is already booted to WinPE.

**Boot to WinPE**

1.  Connect the WinPE USB drive to your reference device.
2.  Turn off the device, and then boot to the WinPE USB drive. You usually do this by powering on the device and quickly pressing a key (for example, the **Esc** key or the **Volume up** key).

    **Note**   On some devices, you might need to go into the boot menus to choose the USB drive. If you're given a choice between booting in UEFI mode or BIOS mode, choose UEFI mode. For more info, see [Boot to UEFI Mode or Legacy BIOS mode](http://go.microsoft.com/fwlink/?LinkId=526943).
    If the device does not boot from the USB drive, see the troubleshooting tips in [WinPE: Create USB Bootable drive](http://go.microsoft.com/fwlink/?LinkId=526944).

     

    WinPE starts at a command line, and runs **wpeinit** to set up the system. This can take a few minutes.

**Format and set up the hard drive partitions on the reference device**

1.  On the reference device, connect the storage USB key or network share.
2.  Find the drive letters by using DiskPart:

    ``` syntax
    diskpart
    DISKPART> list volume
    DISKPART> exit
    ```

    For example, the drives can be lettered like this: *C* = Windows; *D* is the WinPE USB key, and *E* is an external hard drive.

    Some partitions might not receive a drive letter.

3.  Format the primary hard drive, create the partitions, and apply the image by using the pre-made lab script:

    ``` syntax
    D:
    Walkthrough-Deploy.bat "D:\WindowsWithOffice.wim"
    ```

    where *D* is the drive letter of the USB flash drive.

    The script changes the power configuration for high performance, and then creates and configures the partitions based on the firmware on the PC (either UEFI or BIOS).

4.  Shut down or reboot your device, either by holding down the power button for a full five seconds, or by using the following command:

    ``` syntax
    wpeutil shutdown
    ```

 

 





