---
author: Justinha
Description: OEM Windows Desktop Deployment and Imaging Lab
ms.assetid: 04dace4d-9df9-4ead-b23d-f168832c9c04
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: OEM Windows Desktop Deployment and Imaging Lab
---

# OEM Windows Desktop Deployment and Imaging Lab


Getting ready to build and test Windows 10 for desktop editions? Here's a lab that walks through building new devices and customizing them for specific markets to meet your customers' needs.

We’ve developed new tools and strategies to help you create custom images faster. With provisioning packages, you can now capture and apply bundles of Classic Windows applications. You can avoid some of the time-consuming steps involved in generalizing and recapturing images, and mix and match your customizations as new Windows editions are released, either through automated scripts or through a familiar Windows interface.

This lab is organized around these three hardware configurations:

|                              |              |                     |                                   |
|------------------------------|--------------|---------------------|-----------------------------------|
| Hardware Configuration:      | 1            | 1B                  | 2                                 |
| Form factor                  | Small tablet | 2-in-1              | Notebook                          |
| RAM                          | 1 GB         | 2 GB                | 4 GB                              |
| Disk capacity and type       | 16 GB eMMC   | 32 GB eMMC          | 500 GB HDD                        |
| Display size                 | 8”           | 10”                 | 14”                               |
| Windows SKU                  | Core         | Pro                 | Core                              |
| Language(s)                  | EN-US        | EN-US, FR-FR, ES-ES | EN-GB, DE-DE, FR-FR, ES-ES, ZH-CN |
| Cortana                      | Yes          | Yes                 | Yes                               |
| Inbox apps (Universal)       | Yes          | Yes                 | Yes                               |
| Pen                          | No           | Yes                 | No                                |
| Office (Universal)           | Yes          | Yes                 | Yes                               |
| Classic Windows applications | No           | Yes                 | Yes                               |
| Office 2013                  | No           | Yes                 | Yes                               |
| Compact OS                   | Yes          | Yes                 | No                                |

 

For each of them, we will create customized Windows images using the following methods:

-   [Customize and install Windows using the Windows Imaging and Configuration Designer (ICD)](install-windows-automatically-from-a-usb-drive-sxs.md)

    Use Windows Imaging and Configuration Designer (ICD) to customize a Windows image, save it to a USB drive, and use it to deploy to a PC or tablet. If you're new to Windows imaging and deployment, start here.

-   [Classic-style imaging and deployment using command line tools](part-2--classic-style-deployment.md)

    Use command line tools and offline servicing to customize and deploy your Windows image. This part is for partners who have been using our deployment tools and want to get an overview on what has changed in this release and who want to set up advanced configurations like custom drive partition layouts that aren't otherwise supported in Windows ICD.

 

 





