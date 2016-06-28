---
author: Justinha
Description: Install Windows PE
ms.assetid: e6e571df-8b4f-43f8-9a8c-cb5f25969a5d
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: Install Windows PE
---

# Install Windows PE


Windows Preinstallation Environment (WinPE) is a small, command-line based operating system. You can use it to capture, update, and optimize Windows images, which you'll do in later sections. In this section, you'll prepare a basic WinPE image on a bootable USB flash drive and try it out.

For this lab, you'll need two storage locations:

-   **A Windows PE USB key**. Must be at least 512MB and at most 32GB. It should not be a Windows-to-Go key or a key marked as a non-removable drive.
-   **A storage USB key or external hard drive**. Must be at least 8GB.

**Prepare the WinPE files**

1.  Click **Start**, type **Deployment and Imaging Tools Environment**. Right-click **Deployment and Imaging Tools Environment** and select **Run as administrator**.
2.  From the **Deployment and Imaging Tools Environment** command prompt, copy the base WinPE files into a new folder:

    ``` syntax
    copype amd64 C:\winpe_amd64
    ```

    Repeat if you’re also deploying x86 devices:

    ``` syntax
    copype x86 C:\winpe_x86
    ```

**Adding to WinPE**

1.  Mount the WinPE image:

    ``` syntax
    Dism /Mount-Image /ImageFile:"C:\WinPE_amd64\media\sources\boot.wim" /index:1 /MountDir:"C:\WinPE_amd64\mount"
    ```

2.  For some devices, add drivers:

    **Note**  Most devices don't need this step. But if you boot WinPE later and find you can't see the screen or connect to the network, you may need to add a video or network driver.

    ``` syntax
    Dism /Add-Driver /Image:"C:\WinPE_amd64\mount" /Driver:"C:\SampleDriver\driver.inf"
    ```

3.  Unmount the WinPE image:

    ``` syntax
    Dism /Unmount-Image /MountDir:"C:\WinPE_amd64\mount" /commit
    ```

**Create a bootable drive**

1.  Plug in a USB key that you don't mind formatting. Note the drive letter it uses, for example, D.
2.  Install WinPE to an empty USB drive:

    ``` syntax
    MakeWinPEMedia /UFD C:\winpe_amd64 D:
    ```

    When prompted, press **Y** to format the drive and install WinPE.

    Repeat if necessary, plugging in a separate USB key for use when deploying x86 devices:

    ``` syntax
    MakeWinPEMedia /UFD C:\winpe_x86 E:
    ```

    When prompted, press **Y** to format the drive and install WinPE.

3.  In File Explorer, right-click the drive and select **Eject**.

**Try it out**

1.  Connect the WinPE USB drive to your reference device.
2.  After WinPE starts, manually create a pagefile equal to 256 MB in the WinPE. You'll need this later to deploy compressed images (CompactOS); those operations will require that WinPE has a pagefile equal to 256 MB.

    ``` syntax
    Wpeutil createpagefile C:\pagefile.sys /size=256
    ```

    Where "C" is the Windows partition.

3.  Turn off the device, and then boot to the USB drive. You usually do this by powering on the device and quickly pressing a key (for example, the **Esc** key or the **Volume up** key).

    **Note**   On some devices, you might need to go into the boot menus to choose the USB drive. If you're given a choice between booting in UEFI mode or BIOS mode, choose UEFI mode. To learn more, see [Boot to UEFI Mode or Legacy BIOS mode](http://go.microsoft.com/fwlink/?LinkId=526943).
    If the device does not boot from the USB drive, see the troubleshooting tips in [WinPE: Create USB Bootable drive](http://go.microsoft.com/fwlink/?LinkId=526944).

    WinPE starts at a command line, and runs **wpeinit** to set up the system. This can take a few minutes.

 

 

 





