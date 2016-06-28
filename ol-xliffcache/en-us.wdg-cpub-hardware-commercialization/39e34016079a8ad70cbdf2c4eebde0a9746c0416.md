---
author: Justinha
Description: 'Windows 10 includes tools to help you use less drive space.'
ms.assetid: f58ee9cf-0dd1-4c46-9b1f-d16891247f2f
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: 'Compact OS, single-instancing, and image optimization'
---

# Compact OS, single-instancing, and image optimization


Windows 10 includes tools to help you use less drive space. You can now compress the files for the entire operating system, including your preloaded desktop applications. Compact OS lets you run the operating system from compressed files (similar to WIMBoot in Windows 8.1 Update 1), and single-instancing helps you run your pre-loaded Windows desktop applications in compressed files. The new processes helps maintain a small footprint over time by using individual files, rather than combining them in a WIM file.

Here's some ways to shrink the image, optimize the image, and some considerations when deploying to low-cost devices.

## <span id="Deployment_tools_that_help_save_space"></span><span id="deployment_tools_that_help_save_space"></span><span id="DEPLOYMENT_TOOLS_THAT_HELP_SAVE_SPACE"></span>Deployment tools that help save space


### <span id="Compact_OS"></span><span id="compact_os"></span><span id="COMPACT_OS"></span>Compact OS

Compact OS installs the operating system files as compressed files. Compact OS is supported on both UEFI-based and BIOS-based devices.

Unlike WIMBoot, because the files are no longer combined into a single WIM file, Windows update can replace or remove individual files as needed to help maintain the drive footprint size over time.

**Note**  When running Windows Imaging and Configuration Designer (ICD) on a PC running a previous version of Windows, such as Windows 8.1, you'll need to install the [Windows Assessment and Deployment Kit (ADK)](http://go.microsoft.com/fwlink/?LinkId=526803) with both the Windows ICD and **Deployment Tools** features. This installs the latest versions of the drivers required by DISM (wimmount.sys and adkwof.sys) used to create Compact OS images.

 

**To deploy Compact OS with a USB bootable drive**

1.  On your technician PC, open Windows ICD and create your project.
2.  Plug in a USB flash drive and note the drive letter (example: D:).
3.  Click **Create** &gt; **Production Media** &gt; **WIM** &gt; **Enable OS File Compression: Yes** &gt; **Next** &gt; **USB Bootable drive** &gt; drive letter (D:) &gt; **Next** &gt; **Build**.
4.  Boot the destination PC using the USB flash drive. Windows installs automatically.

**To deploy Compact OS using a WIM file**

1.  Create a pagefile equal to 256 MB in WinPE.

    ``` syntax
    Wpeutil createpagefile C:\pagefile /size=256
    ```

    Where "C" is the Windows partition.

2.  Boot your destination device with the Windows 10 version of Windows PE. (To use a previous version of Windows PE, make sure you use the Windows 10 version of DISM. To learn more, see [Copy DISM to Another Computer](copy-dism-to-another-computer.md).)
3.  Format and prepare the partitions, and then apply the image to a partition using the DISM /Apply-Image /Compact option:

    ``` syntax
    DISM /Apply-Image /ImageFile:install.wim /Index:1 /ApplyDir:D:\ /compact
    ```

    This is usually done by running a deployment script. To learn more, see [Apply Images Using DISM](apply-images-using-dism.md).

**To deploy Compact OS from an FFU image**

1.  To deploy an FFU image as compressed, the original FFU image must be created as a compressed image.

    From Windows ICD, click **Create** &gt; **Production Media** &gt; **FFU** &gt; **Enable OS File Compression: Yes** &gt; name the file, for example, D:\\flash.ffu &gt; **Build**.

2.  You can deploy the FFU image directly to a drive from Windows ICD or from Windows Preinstallation Environment (WinPE). To learn more, see [Deploy Windows using Full Flash Update (FFU)](deploy-windows-using-full-flash-update--ffu.md).

**To deploy Compact OS from Windows Setup**

-   Use an unattend.xml file with the setting: Microsoft-Windows-Setup\\ImageInstall\\OSImage\\[Compact](https://msdn.microsoft.com/library/windows/hardware/dn949267).

**Command-line support** In the next release of Windows 10 and WinPE, you can query whether the operating system is running Compact OS, and change it at any time, using the [Compact.exe]( http://go.microsoft.com/fwlink/?LinkId=623487) command.

**From Windows PE, determine if the OS is compacted:**

-   ``` syntax
    Compact.exe /CompactOS:Query /WinDir:E:\Windows
    ```

    Where E:\\Windows is the folder where Windows has been installed.

**From an online installation, change from non-compacted to compacted OS:**

-   ``` syntax
    Compact.exe /CompactOS:always
    ```

### <span id="Single-instancing_of_provisioning_packages"></span><span id="single-instancing_of_provisioning_packages"></span><span id="SINGLE-INSTANCING_OF_PROVISIONING_PACKAGES"></span>Single-instancing of provisioning packages

For Windows 10, when you add new Windows desktop applications to a device, you'll capture these changes into a compressed provisioning package for use by the automatic recovery tools. Rather than maintaining both the original files and the provisioning package, you can use DISM to remove the original files, and run from directly from the compressed provisioning package instead. This is known as single-instancing the image.

While single-instancing is supported on both solid-state drives and rotational drives, for performance reasons, we recommend that single-instancing is only used on devices with solid-state drives.

Example:

``` syntax
DISM /Apply-CustomDataImage /CustomDataImage:C:\Recovery\Customizations\USMT.ppkg /ImagePath:C:\ /SingleInstance
```

where *C* is the drive letter of the Windows partition.

**Warning**  Do not put quotes with the /ImagePath:C:\\ option.

 

You can determine whether a provisioning package (.ppkg) is single-instanced by using fsutil.exe:

``` syntax
fsutil.exe wim enumwims C:
```

where *C* is the drive that contains the provisioning package. Any single-instanced provisioning package on the drive will be listed in the command output. If there are none, the command will return "Error: The system cannot find the file specified."

### <span id="Image_optimization"></span><span id="image_optimization"></span><span id="IMAGE_OPTIMIZATION"></span>Image optimization

After applying updates to a Windows image, cleanup the image and then export it to a new file:

``` syntax
md c:\mount\Windows
md C:\mount\temp

Dism /Mount-Image /ImageFile:"C:\Images\install.wim" /Index:1 /MountDir:C:\mount\Windows

Dism /Cleanup-Image /Image=C:\mount\Windows /StartComponentCleanup /ResetBase /ScratchDir:C:\mount\temp

Dism /Unmount-Image /MountDir:C:\mount\Windows /Commit

Dism /Export-Image /SourceImageFile:C:\Images\install.wim /SourceIndex:1 /DestinationImageFile:C:\Images\install_cleaned.wim
```

where *C:\\Images\\install.wim* is a Windows image file that you want to update. You can optionally specify the /Defer parameter with /ResetBase to defer any long-running cleanup operations to the next automatic maintenance, but we highly recommend that **only** use /Defer as an option in the factory where DISM /ResetBase requires more than 30 minutes to complete. 

## <span id="Size_requirements_and_considerations"></span><span id="size_requirements_and_considerations"></span><span id="SIZE_REQUIREMENTS_AND_CONSIDERATIONS"></span>Size requirements and considerations


You'll still need to meet minimum size requirements for the hard drive, RAM, application resource usage, and data storage.

### <span id="Hard_Drive"></span><span id="hard_drive"></span><span id="HARD_DRIVE"></span>Hard Drive

Windows 10 requires a minimum of 16 gigabytes (GB) of space on 32-bit devices, and 20 GB on 64-bit devices.

Although some configurations of Windows may appear to fit on smaller drives when Windows is first installed, 8 GB SSDs are not large enough. Even if a user pairs an 8 GB hard drive with a second drive that is 4 GB or larger for application and data file storage, 8 GB hard drives do not allow for the increase in the Windows memory footprint that is expected to occur as users work on their computer.

Some of the primary reasons for the increase over time in the memory footprint include the following:

-   **Servicing**. Hard disk space must be reserved for software patches to the operating system and for service pack releases.
-   **System Restore Points**. Windows automatically generate restore points. The amount of space that is required by default is relative to the size of the hard drive. For more information about restore points, see the [Restore Points](http://go.microsoft.com/fwlink/?LinkId=142170) topic on MSDN.
    **Note**   Users can adjust the amount of space used on the computer for System Restore by using the **System Protection** user interface in the **System Properties** dialog box (Sysdm.cpl). Users can also use system image backups that are stored on an external hard disk to restore a system.

     

-   **Logs and Caches**. The operating system stores files such as event logs and error logs on the drive.

### <span id="RAM"></span><span id="ram"></span>RAM

The Pagefile.sys and Hiberfil.sys files increase in size in direct proportion to the amount of RAM on the computer. Windows installations on 16 GB drives have a smaller memory footprint when the computer is limited to 1 GB of RAM. An increase of RAM to a size that is greater than 1 GB will result in increased size of the system files and less space on the hard drive for other applications and files. Increasing the size of the hard drive, however, does not affect the size of these system files.

### <span id="Applications_"></span><span id="applications_"></span><span id="APPLICATIONS_"></span>Applications

Software applications that are installed on the computer may require additional space for caches, logs, and updates. Disk space must also be available on the drive to account for temporary increases in resource usage during installation of applications, patches, and updates.

### <span id="User_Data"></span><span id="user_data"></span><span id="USER_DATA"></span>User Data

On computers that support removable media such as an SD card or USB flash drive, users can easily expand personal data file storage for user documents by using this removable media. However, we recommend that users reserve some space on the hard drive for these types of files.

## <span id="related_topics"></span>Related topics


[Windows Imaging and Configuration Designer](https://msdn.microsoft.com/library/windows/hardware/dn916113)

[Capture and Apply Windows, System, and Recovery Partitions](capture-and-apply-windows-system-and-recovery-partitions.md)

[DISM Image Management Command-Line Options](dism-image-management-command-line-options-s14.md)

 

 






