---
author: Justinha
Description: Append a Volume Image to an Existing Image Using DISM
ms.assetid: c537f8bb-05ed-48cd-b75a-a8c1ed3bc66f
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: Append a Volume Image to an Existing Image Using DISM
---

# Append a Volume Image to an Existing Image Using DISM


The Deployment Image Servicing and Management (DISM) tool is a command-line tool that enables the creation of Windows® image (.wim) files for deployment in a manufacturing or corporate IT environment. The **/Append-Image** option appends a volume image to an existing .wim file allowing you to store many customized Windows images in a fraction of the space. When you combine two or more Windows image files into a single .wim, any files that are duplicated between the images are only stored once.

## <span id="multiple_windows_images_in_a_.wim_file"></span><span id="MULTIPLE_WINDOWS_IMAGES_IN_A_.WIM_FILE"></span>Multiple Windows Images in a .wim file


Before you can append data to an image, you must have the following:

-   A technician computer running Windows 8 or a technician computer with the Windows Assessment and Deployment Kit (Windows ADK) tools installed on it.

-   A Windows image (.wim) file. For more information about how to capture an image using DISM, see [Capture Images of Hard Disk Partitions Using DISM](capture-images-of-hard-disk-partitions-using-dism.md).

**To append a volume image to an existing image**

1.  Open a command prompt with administrator privileges. If you are using a version of Windows other than Windows 8, navigate to the DISM directory.

2.  Append a volume image to an existing image. For example, you can append an image of the D drive to an existing image called my-windows-partition.wim.

    ``` syntax
    Dism /Append-Image /ImageFile:c:\my-windows-partition.wim /CaptureDir:D:\ /Name:Drive-D
    ```

**Next Steps**

1.  You can apply the image by referring to it by image number or image name, for example:

    ``` syntax
    Dism /apply-image /imagefile:install.wim /name:Drive-D /ApplyDir:D:\
    ```

2.  You can extract the image into a separate file by using the **/Export-Image** option. For example:

    ``` syntax
    Dism /Export-Image /SourceImageFile:install.wim /SourceName:Drive-D /DestinationImageFile:DriveD.wim
    ```

For more information, see [DISM Image Management Command-Line Options](dism-image-management-command-line-options-s14.md).

## <span id="related_topics"></span>Related topics


[Capture Images of Hard Disk Partitions Using DISM](capture-images-of-hard-disk-partitions-using-dism.md)

[DISM Image Management Command-Line Options](dism-image-management-command-line-options-s14.md)

 

 






