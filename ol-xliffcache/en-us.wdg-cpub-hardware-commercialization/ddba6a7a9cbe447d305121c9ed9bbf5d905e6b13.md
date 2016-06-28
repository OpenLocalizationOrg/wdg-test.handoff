---
author: kpacquer
Description: 'Test an appx file on an IoT device.'
ms.assetid: ae043bb0-656e-4439-bdbe-a8d370629ab7
MSHAttr: 'PreferredLib:/library'
title: 'Test an appx file on an IoT device'
---

# <span id="TEST_AN_APPX_FILE_ON_AN_IOT_DEVICE"></span>Test an appx file on an IoT device

Connect to the device from another PC**

1.  Connect a network cable.

2.  Note the IP address that shows up on the device.

3.  On your technician PC, open Internet Explorer, and type in the address with an http:// prefix and :8080 suffix:

    ``` syntax
    http://100.100.0.100:8080
    ```

    This opens the [Windows Device Portal](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.md). From here, you can upload app packages, see what apps are installed, and switch between them.

4.  Use the default username (Administrator) and password (p@ssw0rd) to log on.

Test the app by installing it**

1.  Click on **Apps**.

2.  Under **Install app**, under **App package**, click **Browse**, and select your .appx file.
    **Note**  If you built your app as a bundle, you may need to use a tool such as 7-Zip to extract these files from the bundle.

3.  Add your app's **Certificate** the same way.

4.  Click **Add Dependency** and add each of your app's dependency files.

5.  Under **Deploy**, click **Go**. The app installs.

6.  Under **Installed apps**, click the drop-down box and select your app. Your app should start on the device.

Great, your app works! Now let's package it up so you can maintain your app even after it's been delivered to your customers.