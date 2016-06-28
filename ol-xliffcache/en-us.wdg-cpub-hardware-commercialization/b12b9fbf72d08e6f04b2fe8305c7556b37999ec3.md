---
author: Justinha
Description: 'As new Windows updates become available, add them to your image.'
ms.assetid: 9a8f525c-bb8f-492c-a555-0b512e44bcd1
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: 'Lab 2c: Servicing the image with Windows Updates'
---

# Lab 2c: Servicing the image with Windows Updates


As new Windows updates become available, add them to your image.

By default, updates installed after a target rollup update are not restored. To ensure that updates preinstalled during manufacturing are not discarded after recovery, they should be marked as permanent by using the /Cleanup-Image command in DISM with the /StartComponentCleanup and /ResetBase options. Updates marked as permanent are always restored during recovery.

**Step 1: Mount the Windows image file**

-   Run the following commands:

    ``` syntax
    md c:\mount\windows
    Dism /Mount-Image /ImageFile:"C:\Images\WindowsWithOffice.wim" /Index:1 /MountDir:"C:\mount\windows" /Optimize
    ```

    Where /Index:1 refers to the image you want to mount. Note, rem use /Index:2 to mount the Windows 10 Home edition from the default Windows 10 Home/Pro edition.

**Step 2: Add the Windows update package**

1.  From Microsoft Connect, download the Windows update. Save this in the folder: C:\\WindowsUpdates.
2.  Add the updates to the image. For packages with dependencies, make sure you install the packages in order. If you’re not sure of the dependencies, it’s OK to put them all in the same folder, and then add them all using the same DISM /Add-Package command.

    ``` syntax
    Dism /Add-Package /Image:"C:\mount\windows" /PackagePath="C:\WindowsUpdates\kb1010101.cab" /PackagePath="C:\WindowsUpdates\kb1020202.cab" /PackagePath="C:\WindowsUpdates\kb1030303.cab" /LogPath=C:\mount\dism.log
    ```

    where C is the drive letter of the drive and `kb1010101.cab`, `kb1020202.cab`, and `kb3030303.cab` are update packages that you’re adding to the image.

3.  Lock in the updates, so that they are restored during a recovery.

    ``` syntax
    DISM /Cleanup-Image /Image=C:\ /StartComponentCleanup /ResetBase /ScratchDir:C:\Temp
    ```

**Step 3: Unmount the images**

1.  Close all applications that might access files from the image.
2.  Commit the changes and unmount the Windows image:

    ``` syntax
    Dism /Unmount-Image /MountDir:"C:\mount\windows" /Commit
    ```

 

 





