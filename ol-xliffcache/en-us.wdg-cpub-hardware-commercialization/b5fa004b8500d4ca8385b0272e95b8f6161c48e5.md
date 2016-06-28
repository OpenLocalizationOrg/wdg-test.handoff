---
author: Justinha
Description: 'Lab 2a: Answer files: Update settings and run scripts'
ms.assetid: a29101dc-4922-44ee-a758-d555e6cf39fa
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: 'Lab 2a: Answer files: Update settings and run scripts'
---

# Lab 2a: Answer files: Update settings and run scripts


Answer files (or Unattend files) can be used to modify Windows settings in your images during Setup. You can also create scripts that you can inject into your offline images, so that you can efficiently make changes to multiple images without ever booting them.

![diagram of creating a new answer file](images/dep-win8-sxs-createanswerfile.jpg)

## <span id="overview"></span><span id="OVERVIEW"></span>Windows settings overview


While you can set many Windows settings in audit mode, some settings can only be set by using an answer file or Windows Imaging and Configuration Designer (ICD), such as adding manufacturer’s support information. A full list of answer file settings (also known as Unattend settings) is in the [Unattended Windows Setup Reference](https://msdn.microsoft.com/library/windows/hardware/dn923277).

Enterprises can control other settings by using Group Policy. For more info, see [Group Policy](http://go.microsoft.com/fwlink/p/?linkid=268543).

You canspecify which configuration pass to add new settings:

-   **1 windowsPE**: These settings are used by the Windows Setup installation program. If you’re modifying existing images, you can usually ignore these settings.
-   **4 specialize**: Most settings should be added here. These settings are triggered both at the beginning of audit mode and at the beginning of OOBE. If you need to make multiple updates or test settings, generalize the device again and add another batch of settings in the Specialize Configuration pass.
-   **6 auditUser**: Runs as soon as you start audit mode.

    This is a great time to run a system test script - we'll add [Microsoft-Windows-Deployment\\RunAsynchronousCommand](https://msdn.microsoft.com/library/windows/hardware/dn915797) as our example. To learn more, see [Add a Custom Script to Windows Setup](add-a-custom-script-to-windows-setup.md).

-   **7 oobeSystem**: Use sparingly. Most of these settings run after the user completes OOBE. The exception is the Microsoft-Windows-Deployment\\Reseal\\[Mode](https://msdn.microsoft.com/library/windows/hardware/dn923110) = Audit setting, which we’ll use to bypass OOBE and boot the PC into audit mode.

    If your script relies on knowing which language the user selects during OOBE, you’d add it to the oobeSystem pass.

-   For info on the other configuration passes, see [Windows Setup Configuration Passes](windows-setup-configuration-passes.md).

**Note**  These settings could be lost if the user resets their PC with the built-in recovery tools. To see how to make sure these settings stay on the device during a reset, see [Sample scripts](windows-deployment-sample-scripts-sxs.md): Keeping Windows settings through a recovery.

 

## <span id="createanswer"></span><span id="CREATEANSWER"></span>Create and modify an answer file


**Create a catalog file**

1.  Start **Windows System Image Manager**.

2.  On the **File** menu, click **Select Windows Image**.

3.  In the **Select a Windows Image** dialog, browse to and select the base-image file. Next, select an edition of Windows, for example, Windows 10 Pro, and click **OK**. Click **Yes** to create the catalog file. Windows SIM creates the file based on the base-image file, and saves it to the Windows desktop. This process can take several minutes.

    The catalog file appears in the **Windows Image** pane. Windows SIM also lists the configurable components and packages in that image.

    **Troubleshooting:** If Windows SIM does not create the catalog file, try the following steps:

    -   Make sure you are using a Windows 10 for desktop editions (Home, Pro, Enterprise, and Education) version of the Windows Assessment and Deployment Kit (Windows ADK) tools.

    -   To create a catalog file for either 32-bit or ARM-based devices, use a 32-bit device.

    -   Make sure the Windows base-image file **(\\Sources\\Install.wim**) is in a folder that has read-write privileges, such as a USB flash drive or on your hard drive.

**Create an answer file**

-   On the **File** menu, click **New Answer File**.

    The new answer file appears in the **Answer File** pane.

    **Note**   If you open an existing answer file, you might be prompted to associate the answer file with the image. Click **Yes**.

     

**Add new answer file settings**

1.  Set the device to automatically boot to audit mode:

    In the **Windows Image** pane, expand **Components**, right-click **amd64\_Microsoft-Windows-Deployment\_(version)**, and then select **Add Setting to Pass 7 oobeSystem**.

    In the **Answer File** pane, select **Components\\7 oobeSystem\\amd64\_Microsoft-Windows-Deployment\_neutral\\Reseal**.

    In the **Reseal Properties** pane, in the **Settings** section, select Mode=`Audit`.

2.  Prepare a script to run after Audit mode begins.

    In the **Windows Image** pane, right-click **amd64\_ Microsoft-Windows-Deployment\_(version)** and then click **Add Setting to Pass 6 auditUser**.

    In the **Answer File** pane, expand **Components\\6 auditUser\\amd64\_Microsoft-Windows-Deployment\_neutral\\RunAsynchronous**. Right-click **RunAsynchronousCommand Properties** and click **Insert New AsynchronousCommand**.

    In the **AsynchronousCommand Properties** pane, in the **Settings** section, add the following values:

    `Path = C:\Fabrikam\SampleCommand.cmd`

    `Description = Sample command to run a system diagnostic check.`

    `Order = 1`

**Save the answer file**

-   Save the answer file, for example: **C:\\AnswerFiles\\BootToAudit-x64.xml**.

    **Note**   Windows SIM will not allow you to save the answer file into the mounted image folders.

     

**Create a script**

-   Copy the following sample script into Notepad, and save it as **C:\\AnswerFiles\\SampleCommand.cmd**.

    ``` syntax
    @rem Scan the integrity of system files 
    @rem (Required after removing the base English language from an image)
    sfc.exe /scannow

    @rem Check to see if your drivers are digitally signed, and send output to a log file.
    md C:\Fabrikam
    C:\Windows\System32\dxdiag /t C:\Fabrikam\DxDiag-TestLogFiles.txt
    ```

## <span id="Add_the_answer_file_and_script_to_the_image"></span><span id="add_the_answer_file_and_script_to_the_image"></span><span id="ADD_THE_ANSWER_FILE_AND_SCRIPT_TO_THE_IMAGE"></span>Add the answer file and script to the image


1.  Mount the Windows image. The mounting process maps the contents of an image file to a location where you can view and modify the mounted image.

    ``` syntax
    md C:\mount\windows
    Dism /Mount-Image /ImageFile:"C:\Images\WindowsWithOffice.wim" /Index:1 /MountDir:"C:\mount\windows" /Optimize
    ```

    Where *C* is the drive letter of the drive that contains the image and *1* is the index of the image that you want to use.

    This step can take several minutes.

2.  Copy the answer file into the image into the \\Windows\\Panther folder, and name it unattend.xml. Create the folder if it doesn’t exist. If there’s an existing answer file, replace it or use Windows System Image Manager to edit/combine settings if necessary.

    ``` syntax
    MkDir c:\mount\windows\Windows\Panther
    Copy C:\AnswerFiles\BootToAudit-x64.xml  C:\mount\windows\Windows\Panther\unattend.xml
    MkDir c:\mount\windows\Fabrikam
    Copy C:\AnswerFiles\SampleCommand.cmd    C:\mount\windows\Fabrikam\SampleCommand.cmd
    ```

3.  Commit the changes and unmount the Windows image:

    ``` syntax
    Dism /Unmount-Image /MountDir:"C:\mount\windows" /Commit
    ```

    where *C* is the drive letter of the drive that contains the image.

    This process may take several minutes.

**Verify your customizations**

1.  Apply the Windows image to a new device as described in [Lab 2: Classic-style deployment](part-2--classic-style-deployment.md), Step 15: Apply Windows images using a script.
2.  When audit mode starts, the script should start automatically.
3.  In File Explorer, check to see if the file: **C:\\Fabrikam\\DxDiag-TestLogFiles.txt** exists. If so, your sample script ran correctly.

While in Audit mode, you can start [Lab 2b: Windows desktop apps and data: capturing changes](prepare-a-snapshot-of-the-pc-generalize-and-capture-windows-images-blue-sxs.md).

 

 





