---
author: Justinha
Description: 'The following scripts are used in the lab. It may be helpful to create these all at once, or to download the samples from the web.'
ms.assetid: 621503da-e74f-4eef-8315-72c8be67747a
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: Sample scripts
---

# Sample scripts

The following scripts are used in the lab. It may be helpful to create these all at once, or to [download the samples](http://go.microsoft.com/fwlink/p/?LinkId=800657) from the web.

## <span id="Image_deployment_scripts"></span><span id="image_deployment_scripts"></span><span id="IMAGE_DEPLOYMENT_SCRIPTS"></span>Image deployment scripts


The following scripts set up Windows devices by using an image file, and configure push-button reset features.

### <span id="CreatePartitions-_firmware_.txt"></span><span id="createpartitions-_firmware_.txt"></span><span id="CREATEPARTITIONS-_FIRMWARE_.TXT"></span>CreatePartitions-(firmware).txt

Use these scripts together with DiskPart to format and set up the hard disk partitions for Windows, including recovery tools. Adjust the partition sizes to fill the drive as necessary.

**CreatePartitions-UEFI.txt**:

Creates the System, MSR, Windows, and recovery tools partitions for UEFI-based PCs.

This script temporarily assigns these drive letters: System=S, Windows=W, and Recovery=R. The MSR partition doesn't get a letter. The letter W is used to avoid potential drive letter conflicts. After the device reboots, the Windows partition is assigned the letter C, and the other partitions don’t receive drive letters.

The following diagram shows the resulting partition configuration:

![diagram of default partition layout: system, msr, windows, and recovery](images/dep-win10-partitions-uefi.png)

``` syntax
rem == CreatePartitions-UEFI.txt ==
rem == These commands are used with DiskPart to
rem    create four partitions
rem    for a UEFI/GPT-based PC.
rem    Adjust the partition sizes to fill the drive
rem    as necessary. ==
select disk 0
clean
convert gpt
rem == 1. System partition =========================
create partition efi size=100
rem    ** NOTE: For Advanced Format 4Kn drives,
rem               change this value to size = 260 ** 
format quick fs=fat32 label="System"
assign letter="S"
rem == 2. Microsoft Reserved (MSR) partition =======
create partition msr size=16
rem == 3. Windows partition ========================
rem ==    a. Create the Windows partition ==========
create partition primary 
rem ==    b. Create space for the recovery tools ===
shrink minimum=500
rem       ** NOTE: Update this size to match the
rem                size of the recovery tools 
rem                (winre.wim)                    **
rem ==    c. Prepare the Windows partition ========= 
format quick fs=ntfs label="Windows"
assign letter="W"
rem === 4. Recovery partition ======================
create partition primary
format quick fs=ntfs label="Recovery"
assign letter="R"
set id="de94bba4-06d1-4d40-a16a-bfd50179d6ac"
gpt attributes=0x8000000000000001
list volume
exit
```

**CreatePartitions-BIOS.txt**:

Creates the System, Windows, and recovery tools partitions for BIOS-based PCs.

This script temporarily assigns these drive letters: System=S, Windows=W, and Recovery=R. The letter W is used to avoid potential drive letter conflicts. After the device reboots, the Windows partition is assigned the letter C, and the other partitions don’t receive drive letters.

The following diagram shows the resulting partition configuration:

![diagram of default partition layout: system, windows, and recovery](images/dep-win10-partitions-bios.png)

``` syntax
rem == CreatePartitions-BIOS.txt ==
rem == These commands are used with DiskPart to
rem    create three partitions
rem    for a BIOS/MBR-based computer.
rem    Adjust the partition sizes to fill the drive
rem    as necessary. ==
select disk 0
clean
rem == 1. System partition ======================
create partition primary size=100
format quick fs=ntfs label="System"
assign letter="S"
active
rem == 2. Windows partition =====================
rem ==    a. Create the Windows partition =======
create partition primary
rem ==    b. Create space for the recovery tools  
shrink minimum=500
rem       ** NOTE: Update this size to match the
rem                size of the recovery tools 
rem                (winre.wim)                 **
rem ==    c. Prepare the Windows partition ====== 
format quick fs=ntfs label="Windows"
assign letter="W"
rem == 3. Recovery partition ====================
create partition primary
format quick fs=ntfs label="Recovery image"
assign letter="R"
set id=27
list volume
exit
```

### <span id="Hide_the_recovery_partitions"></span><span id="hide_the_recovery_partitions"></span><span id="HIDE_THE_RECOVERY_PARTITIONS"></span>Hide the recovery partitions

Use these scripts with DiskPart to hide the recovery partitions after they have been configured.

**HideRecoveryPartitions-UEFI.txt**:

``` syntax
rem === HideRecoveryPartitions-UEFI.txt ===
select disk 0
select partition 4
set id=de94bba4-06d1-4d40-a16a-bfd50179d6ac
gpt attributes=0x8000000000000001
remove
list volume
```

**HideRecoveryPartitions-BIOS.txt:**

``` syntax
rem === HideRecoveryPartitions-BIOS.txt ===
select disk 0
select partition 3
set id=27
remove
list volume
```

### ApplyImage.bat

Use this script to launch the other scripts that deploy Windows to a new device.

``` syntax
@echo ApplyImage.bat
@echo     Run from the reference device in the WinPE environment
@echo     This script erases the primary hard drive and applies a new image
@if not exist X:\Windows\System32 echo ERROR: This script is built to run in Windows PE.
@if not exist X:\Windows\System32 goto END
@if %1.==. echo ERROR: To run this script, add a path to a Windows image file.
@if %1.==. echo Example: ApplyImage D:\WindowsWithFrench.wim
@if %1.==. goto END
@echo *********************************************************************
@echo Checking to see if the PC is booted in BIOS or UEFI mode.
wpeutil UpdateBootInfo
for /f "tokens=2* delims=	 " %%A in ('reg query HKLM\System\CurrentControlSet\Control /v PEFirmwareType') DO SET Firmware=%%B
@echo            Note: delims is a TAB followed by a space.
@if x%Firmware%==x echo ERROR: Can't figure out which firmware we're on.
@if x%Firmware%==x echo        Common fix: In the command above:
@if x%Firmware%==x echo             for /f "tokens=2* delims=	 "
@if x%Firmware%==x echo        ...replace the spaces with a TAB character followed by a space.
@if x%Firmware%==x goto END
@if %Firmware%==0x1 echo The PC is booted in BIOS mode. 
@if %Firmware%==0x2 echo The PC is booted in UEFI mode. 
@echo *********************************************************************
@echo Formatting the primary disk...
@if %Firmware%==0x1 echo    ...using BIOS (MBR) format and partitions.
@if %Firmware%==0x2 echo    ...using UEFI (GPT) format and partitions. 
@echo CAUTION: All the data on the disk will be DELETED.
@SET /P READY=Erase all data and continue? (Y or N):
@if %READY%.==y. set READY=Y
@if not %READY%.==Y. goto END
if %Firmware%==0x1 diskpart /s %~dp0CreatePartitions-BIOS.txt
if %Firmware%==0x2 diskpart /s %~dp0CreatePartitions-UEFI.txt
@echo *********************************************************************
@echo  == Set high-performance power scheme to speed deployment ==
call powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
@echo *********************************************************************
@echo  == Apply the image to the Windows partition ==
@SET /P COMPACTOS=Deploy as Compact OS? (Y or N):
@if %COMPACTOS%.==y. set COMPACTOS=Y
@echo Does this image include Extended Attributes?
@echo    (If you're not sure, type N).
@SET /P EA=(Y or N):
@if %EA%.==y. set EA=Y
if %COMPACTOS%.==Y.     if %EA%.==Y.     dism /Apply-Image /ImageFile:%1 /Index:1 /ApplyDir:W:\ /Compact /EA
if not %COMPACTOS%.==Y. if %EA%.==Y.     dism /Apply-Image /ImageFile:%1 /Index:1 /ApplyDir:W:\ /EA
if %COMPACTOS%.==Y.     if not %EA%.==Y. dism /Apply-Image /ImageFile:%1 /Index:1 /ApplyDir:W:\ /Compact
if not %COMPACTOS%.==Y. if not %EA%.==Y. dism /Apply-Image /ImageFile:%1 /Index:1 /ApplyDir:W:\
@echo *********************************************************************
@echo == Copy boot files to the System partition ==
W:\Windows\System32\bcdboot W:\Windows /s S:
@echo *********************************************************************
@echo   Next steps:
@echo   * Add Windows Classic apps (optional):
@echo       DISM /Apply-SiloedPackage /ImagePath:W:\ 
@echo            /PackagePath:"D:\App1.spp" /PackagePath:"D:\App2.spp"  ...
@echo   * Add the recovery image:
@echo       ApplyRecovery.bat
@echo   * Reboot:
@echo       exit
:END
```

### ApplyRecovery.bat

Use this script to prepare the Windows recovery partition.

``` syntax
@echo == ApplyRecovery.bat ==
@echo  *********************************************************************
@echo  == Copy the Windows RE image to the Windows RE Tools partition ==
md R:\Recovery\WindowsRE
xcopy /h W:\Windows\System32\Recovery\Winre.wim R:\Recovery\WindowsRE\
@echo  *********************************************************************
@echo  == Register the location of the recovery tools ==
W:\Windows\System32\Reagentc /Setreimage /Path R:\Recovery\WindowsRE /Target W:\Windows
@echo  *********************************************************************
@echo  == If Compact OS, single-instance the recovery provisioning package ==
@echo     Options: N: No
@echo              Y: Yes
@echo              D: Yes, but defer cleanup steps to first boot.
@echo                 Use this if the cleanup steps take more than 30 minutes.
@echo                 defer the cleanup steps to the first boot.
@SET /P COMPACTOS=Deploy as Compact OS? (Y, N, or D):
@if %COMPACTOS%.==y. set COMPACTOS=Y
@if %COMPACTOS%.==d. set COMPACTOS=D
if %COMPACTOS%.==Y. dism /Apply-CustomDataImage /CustomDataImage:W:\Recovery\Customizations\USMT.ppkg /ImagePath:W:\ /SingleInstance
if %COMPACTOS%.==D. dism /Apply-CustomDataImage /CustomDataImage:W:\Recovery\Customizations\USMT.ppkg /ImagePath:W:\ /SingleInstance /Defer
@rem *********************************************************************
@echo Checking to see if the PC is booted in BIOS or UEFI mode.
wpeutil UpdateBootInfo
for /f "tokens=2* delims=	 " %%A in ('reg query HKLM\System\CurrentControlSet\Control /v PEFirmwareType') DO SET Firmware=%%B
@echo            Note: delims is a TAB followed by a space.
@if x%Firmware%==x echo ERROR: Can't figure out which firmware we're on.
@if x%Firmware%==x echo        Common fix: In the command above:
@if x%Firmware%==x echo             for /f "tokens=2* delims=	 "
@if x%Firmware%==x echo        ...replace the spaces with a TAB character followed by a space.
@if x%Firmware%==x goto END
@if %Firmware%==0x1 echo The PC is booted in BIOS mode. 
@if %Firmware%==0x2 echo The PC is booted in UEFI mode. 
@echo  *********************************************************************
@echo == Hiding the recovery tools partition
if %Firmware%==0x1 diskpart /s %~dp0HideRecoveryPartitions-BIOS.txt
if %Firmware%==0x2 diskpart /s %~dp0HideRecoveryPartitions-UEFI.txt
@echo *********************************************************************
@echo == Verify the configuration status of the images. ==
W:\Windows\System32\Reagentc /Info /Target W:\Windows
@echo    (Note: Windows RE status may appear as Disabled, this is OK.)
@echo *********************************************************************
@echo      All done!
@echo      Disconnect the USB drive from the reference device.
@echo      Type exit to reboot.
@echo.
:END
```

### <span id="Walkthrough-Deploy.bat"></span><span id="walkthrough-deploy.bat"></span><span id="WALKTHROUGH-DEPLOY.BAT"></span>Walkthrough-Deploy.bat

Use this script to launch the other scripts, deploying Windows to a new device.

``` syntax
@echo Walkthrough-Deploy.bat
@echo     Run from the reference device in the WinPE environment
@echo     This script erases the primary hard drive and applies a new image
@echo.
@echo UPDATE (JUNE 2016):
@echo   To apply siloed provisioning packages (SPPs), use these scripts instead:
@echo     ApplyImage.bat install.wim
@echo       (apply your SPPs)
@echo     ApplyRecovery.bat
@echo.
@if not exist X:\Windows\System32 echo ERROR: This script is built to run in Windows PE.
@if not exist X:\Windows\System32 goto END
@if %1.==. echo ERROR: To run this script, add a path to a Windows image file.
@if %1.==. echo Example: Walkthrough-Deploy D:\WindowsWithFrench.wim
@if %1.==. goto END
@echo *********************************************************************
SET /P M=Deploy as Compact OS? (Y or N):
@if %M%.==Y. set COMPACTOS=True
@if %M%.==y. set COMPACTOS=True
@if %COMPACTOS%==True echo Deploying as Compact OS.
@rem *********************************************************************
@echo Checking to see if the PC is booted in BIOS or UEFI mode.
wpeutil UpdateBootInfo
for /f "tokens=2* delims=	 " %%A in ('reg query HKLM\System\CurrentControlSet\Control /v PEFirmwareType') DO SET Firmware=%%B
@echo            Note: delims is a TAB followed by a space.
@if x%Firmware%==x echo ERROR: Can't figure out which firmware we're on.
@if x%Firmware%==x echo        Common fix: In the command above:
@if x%Firmware%==x echo             for /f "tokens=2* delims=	 "
@if x%Firmware%==x echo        ...replace the spaces with a TAB character followed by a space.
@if x%Firmware%==x goto END
@if %Firmware%==0x1 echo The PC is booted in BIOS mode. 
@if %Firmware%==0x2 echo The PC is booted in UEFI mode. 
@rem *********************************************************************
@echo Formatting the primary disk...
@if %Firmware%==0x1 echo ...using BIOS (MBR) format and partitions.
@if %Firmware%==0x2 echo ...using UEFI (GPT) format and partitions. 
@echo CAUTION: All the data on the disk will be DELETED.
@SET /P READY=Erase all data and continue? (Y or N):
@if %READY%.==y. set READY=Y
@if not %READY%.==Y. goto END
if %Firmware%==0x1 diskpart /s %~dp0CreatePartitions-BIOS.txt
if %Firmware%==0x2 diskpart /s %~dp0CreatePartitions-UEFI.txt
@echo *********************************************************************
@echo  == Set high-performance power scheme to speed deployment ==
call powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
@echo *********************************************************************
@echo  == Apply the image to the Windows partition ==
@SET /P COMPACTOS=Deploy as Compact OS? (Y or N):
@if %COMPACTOS%.==y. set COMPACTOS=Y
if %COMPACTOS%.==Y.     dism /Apply-Image /ImageFile:%1 /Index:1 /ApplyDir:W:\ /Compact
if not %COMPACTOS%.==Y. dism /Apply-Image /ImageFile:%1 /Index:1 /ApplyDir:W:\
@echo  *********************************************************************
@echo == Copy boot files to the System partition ==
W:\Windows\System32\bcdboot W:\Windows /s S:
@echo  *********************************************************************
@echo  == Copy the Windows RE image to the Windows RE Tools partition ==
md R:\Recovery\WindowsRE
xcopy /h W:\Windows\System32\Recovery\Winre.wim R:\Recovery\WindowsRE\
@echo  *********************************************************************
@echo  == Register the location of the recovery tools ==
W:\Windows\System32\Reagentc /Setreimage /Path R:\Recovery\WindowsRE /Target W:\Windows
@echo  *********************************************************************
@echo == Verify the configuration status of the images. ==
@echo    Note: Windows RE may appear as Disabled, this is OK.
W:\Windows\System32\Reagentc /Info /Target W:\Windows
pause
@echo  *********************************************************************
@echo == Hiding the recovery tools partition
if %Firmware%==0x1 diskpart /s %~dp0HideRecoveryPartitions-BIOS.txt
if %Firmware%==0x2 diskpart /s %~dp0HideRecoveryPartitions-UEFI.txt
@echo *********************************************************************
@echo      All done!
@echo      Disconnect the USB drive from the reference device.
@echo      Type   exit   to reboot.
@echo.
:END
```

## <span id="start_layout__layoutmodification.xml_"></span><span id="START_LAYOUT__LAYOUTMODIFICATION.XML_"></span>Start layout (LayoutModification.xml)


The Start tile layout in Windows 10 provides OEMs the ability to append tiles to the default Start layout to include Web links, secondary tiles, Windows apps, and Classic Windows applications. OEMs can use this layout to make it applicable to multiple regions or markets without duplicating a lot of the work. In addition, OEMs can add up to three default apps to the frequently used apps section in the system area, which delivers sytem-driven lists o the user including important or frequently accessed system locations and recently installed apps.

To take advantage of all these new features and have the most robust and complete Start customization experience for Windows 10, consider creating a LayoutModification.xml file. This file specifies how the OEM tiles should be laid out in Start. For more information about how to customize the new Start layout, see the topic [Customize the Windows 10 Start screen](https://msdn.microsoft.com/library/windows/hardware/mt170651) in the Windows 10 Partner Documentation.

Sample **LayoutModification.xml**:

``` syntax
<LayoutModificationTemplate
    xmlns="http://schemas.microsoft.com/Start/2014/LayoutModification"
    xmlns:defaultlayout="http://schemas.microsoft.com/Start/2014/FullDefaultLayout"
    xmlns:start="http://schemas.microsoft.com/Start/2014/StartLayout"
    Version="1">
  <RequiredStartGroupsCollection>
    <RequiredStartGroups
      Region="DE|ES|FR|GB|IT|US">
      <AppendGroup
        Name="Fabrikam Group 1">
        <start:Tile
          AppUserModelID="Microsoft.Office.Word_8wekyb3d8bbwe!microsoft.word"
          Size="2x2"
          Row="0"
          Column="0"/>
        <start:DesktopApplicationTile
          DesktopApplicationID="Microsoft.Windows.Explorer"
          Size="2x2"
          Row="0"
          Column="2"/>
        <start:Tile
          AppUserModelID="Microsoft.Office.Excel_8wekyb3d8bbwe!microsoft.excel"
          Size="2x2"
          Row="0"
          Column="4"/>
      </AppendGroup>    
      <AppendGroup
        Name="Fabrikam Group 2">
        <start:Tile
          AppUserModelID="Microsoft.Reader_8wekyb3d8bbwe!Microsoft.Reader"
          Size="2x2"
          Row="0"
          Column="0"/>
        <start:DesktopApplicationTile
          DesktopApplicationID="http://www.bing.com/"
          Size="2x2"
          Row="0"
          Column="2"/>
        <start:DesktopApplicationTile
          DesktopApplicationLinkPath="%ALLUSERSPROFILE%\Microsoft\Windows\Start Menu\Programs\Accessories\Paint.lnk"
          Size="2x2"
          Row="0"
          Column="4"/>
      </AppendGroup>
    </RequiredStartGroups>
    <RequiredStartGroups>
      <AppendGroup
        Name="Fabrikam Group 1">
        <start:Tile
          AppUserModelID="Microsoft.Office.Word_8wekyb3d8bbwe!microsoft.word"
          Size="2x2"
          Row="0"
          Column="0"/>
        <start:SecondaryTile
          AppUserModelID="Microsoft.Windows.Spartan_cw5n1h2txyewy!Microsoft.Spartan.Spartan"
          TileID="FabrikamWeblinkTile"
          Arguments="http://www.fabrikam.com"
          DisplayName="Fabrikam"
          Square150x150LogoUri="ms-appx:///Assets/SpartanMedium.png"
          ShowNameOnSquare150x150Logo="true"
          BackgroundColor="#FF112233"
          Size="2x2"
          Row="0"
          Column="2"/>
      </AppendGroup>    
    </RequiredStartGroups>
  </RequiredStartGroupsCollection> 
  <TopMFUApps>
    <Tile AppUserModelID="Microsoft.WindowsCalculator_8wekyb3d8bbwe!App" />
  </TopMFUApps>  
</LayoutModificationTemplate>
```

**Start layout walkthrough**

1.  If you don’t already have one, create a file called **LayoutModification.xml**.
2.  If you need to specify whether the append groups must only be applied to specific regions, use the optional **Region** attribute in the **RequiredStartGroups** element. The Region value must be equal to two-letter country/region codes. Use a pipe “|” delimiter if you need to specify multiple countries/regions.

    ``` syntax
    <RequiredStartGroups
          Region="DE|ES|FR|GB|IT|US">
    ```

3.  Specify the tiles you want to add within an **AppendGroup**. OEMs can add a maximum of two **AppendGroup**. The following example shows two groups called “Fabrikam Group 1” and “Fabrikam Group 2”, which contain tiles that will be applied if the device country/region matches what’s specified in **Region** (in this case, the regions are Germany, Spain, France, United Kingdom, Italy, and United States). Each group contains three tiles and the various elements you need to use depending on the tile that you want to pin to Start.

    ``` syntax
    <RequiredStartGroups
          Region="DE|ES|FR|GB|IT|US">
          
          <!-- OEMs can add a maximum of two AppendGroup. Each AppendGroup specifies a group of
               tiles that will be appended to Start. -->
          <AppendGroup
            Name="Fabrikam Group 1">
            <!-- Add the News Universal Windows app to Start -->
            <start:Tile
              AppUserModelID="Microsoft.Office.Word_8wekyb3d8bbwe!microsoft.word"
              Size="2x2"
              Row="0"
              Column="0"/>
            <!-- Add a Classic Windows application with a known AppUserModelID  -->
            <start:DesktopApplicationTile
              DesktopApplicationID="Microsoft.Windows.Explorer"
              Size="2x2"
              Row="0"
              Column="2"/>
            <!-- Add the Excel Preview Universal Windows app -->
            <start:Tile
              AppUserModelID="Microsoft.Office.Excel_8wekyb3d8bbwe!microsoft.excel"
              Size="2x2"
              Row="0"
              Column="4"/>
          </AppendGroup>
          
          <AppendGroup
            Name="Fabrikam Group 2">
            <!-- Add a Windows 8.1 app -->
            <start:Tile
              AppUserModelID="Microsoft.Reader_8wekyb3d8bbwe!Microsoft.Reader"
              Size="2x2"
              Row="0"
              Column="0"/>
            <!-- Web link tile with associated .url file is in legacy Start Menu folder. This requires
                 a shortcut or .url file to be added in one of several legacy Start Menu directories, such as
                 "%APPDATA%\Microsoft\Windows\Start Menu\Programs\" 
                 or the all users profile "%ALLUSERSPROFILE%\Microsoft\Windows\Start Menu\Programs\" -->
            <start:DesktopApplicationTile
              DesktopApplicationID="http://www.bing.com/"
              Size="2x2"
              Row="0"
              Column="2"/>
            <!-- Add a Classic Windows application link in a legacy Start Menu folder. You must add the .lnk file 
                 in the specified location when the device first boots. -->
            <start:DesktopApplicationTile
              DesktopApplicationLinkPath="%ALLUSERSPROFILE%\Microsoft\Windows\Start Menu\Programs\Accessories\Paint.lnk"
              Size="2x2"
              Row="0"
              Column="4"/>
          </AppendGroup>
        </RequiredStartGroups>
    ```

    The following example shows one group called “Fabrikam Group 1”, which will be applied if the device country/region doesn’t match any of the ones specified in the previous RequiredStartGroups.

    ``` syntax
        <!-- Non-region specific group -->
        <RequiredStartGroups>
          <AppendGroup
            Name="Fabrikam Group 1">
            <!-- Add the Word Preview Universal Windows app -->
            <start:Tile
              AppUserModelID="Microsoft.Office.Word_8wekyb3d8bbwe!microsoft.word"
              Size="2x2"
              Row="0"
              Column="0"/>
            <!-- Add the Excel Preview Universal Windows app -->
            <start:Tile
              AppUserModelID="Microsoft.Office.Excel_8wekyb3d8bbwe!microsoft.excel"
              Size="2x2"
              Row="0"
              Column="2"/>
          </AppendGroup>    
        </RequiredStartGroups>
    ```

    Keep the following in mind when creating your LayoutModification.xml file:

    -   If you are pinning a Classic Windows applications using the **start:DesktopApplicationTile** tag and you don’t know the application’s application user model ID, you need to create a .lnk file in a legacy Start Menu directory before first boot.
    -   If you use the **start:DesktopApplicationTile** tag to pin a legacy .url shortcut to Start, you must create a .url file and add this file to a legacy Start Menu directory before first boot.

    For the above scenarios, you can use the following directories to put the .url or .lnk files:

    -   %APPDATA%\\Microsoft\\Windows\\Start Menu\\Programs\\
    -   %ALLUSERSPROFILE%\\Microsoft\\Windows\\Start Menu\\Programs\\

4.  Optionally, you can add up to 3 apps to the frequently used section of the system area. The following example shows how to add the calculator app to the frequently used system area.

    ``` syntax
      <!-- Add the calculator app to the frequently used system area -->
      <TopMFUApps>
        <Tile AppUserModelID="Microsoft.WindowsCalculator_8wekyb3d8bbwe!App" />
      </TopMFUApps>
    ```

5.  Save the LayoutModification.xml file.

    Once you’ve created the LayoutModification.xml file, you’ll need to place this file in the correct system location using either Windows ICD or classic-style deployment. For more information on how to do this, see:

    -   Windows ICD - Lab 1, Step 7: Customize the Start layout
    -   Classic-style deployment - Lab 2a, Step 5: Add the files you need to modify the Start layout

    If you don’t create a LayoutModification.xml file and you continue to use the Start Unattend settings, the OS will use the Unattend answer file and take the first 12 SquareTiles or DesktoporSquareTiles settings specified in the Unattend file. The system then places these tiles automatically within the newly-created groups at the end of Start—the first six tiles are placed in the first OEM group and the second set of six tiles are placed in the second OEM group. If OEMName is specified in the Unattend file, the value for this element is used to name the OEM groups that will be created.

## <span id="microphone_settings__speechsetting.cmd_"></span><span id="MICROPHONE_SETTINGS__SPEECHSETTING.CMD_"></span>Microphone settings (SpeechSetting.cmd)


Use this script to tune your device’s microphone to help maximize speech accuracy for features like Cortana. To learn how to test for the appropriate values for your device, see [Cortana Device Test Setup](https://msdn.microsoft.com/library/windows/hardware/dn957009) in the Hardware WEG.

**SpeechSetting.cmd**:

``` syntax
@echo off
@echo.
@echo This script will set the Device.SpeechRecognition.DefaultMicGain values.
@echo.
 
setlocal
setlocal ENABLEDELAYEDEXPANSION
 
if "%1"=="" goto :InputPrompt
 call :tohex %1
@echo %_DECVAL% == 0x%_HEXVAL%
set /a Percentage=%_DECVAL%/100
 
set RegPath=HKLM\Software\Microsoft\Speech_OneCore\AudioInput\MicWiz
set RegKey=DefaultDefaultMicGain
set RegValue=0x%_HEXVAL%
 
@echo reg add %RegPath% /v %RegKey% /t REG_DWORD /d %RegValue% /f
reg add %RegPath% /v %RegKey% /t REG_DWORD /d %RegValue% /f
if %errorlevel% NEQ 0 echo Reg Add function failed. && goto :Error
@echo.
@echo Successfully set the MicGain value to %Percentage% percent.
@echo.
goto :end
 
:ToHex
     set _DECVAL=%1
     set _HEXVAL=
     set _VAL=%1
     if %1 LSS 0 (
         REM break the number into two parts so that we can output the
         REM full value within the bounds of a 32 bit signed value
         set /A _offset="-(-2147483647 - !_VAL!) + 2"
         set /A _VAL="!_offset! / 16 + 0x7FFFFFF"
         set /A _P="!_offset! %% 16 + 0xF"
 
         if !_P! GEQ 16 (
         set /A _VAL="!_VAL! + 1"
         set /A _P="!_P! %% 16"
         )
         if !_P! LEQ 9 set _HEXVAL=!_P!!_HEXVAL!
         if "!_P!" == "10" set _HEXVAL=A!_HEXVAL!
         if "!_P!" == "11" set _HEXVAL=B!_HEXVAL!
         if "!_P!" == "12" set _HEXVAL=C!_HEXVAL!
         if "!_P!" == "13" set _HEXVAL=D!_HEXVAL!
         if "!_P!" == "14" set _HEXVAL=E!_HEXVAL!
         if "!_P!" == "15" set _HEXVAL=F!_HEXVAL!
     )
 
     :hexloop
     set /A _P="%_VAL% %% 16"
     if %_P% LEQ 9 set _HEXVAL=%_P%%_HEXVAL%
     if "%_P%" == "10" set _HEXVAL=A%_HEXVAL%
     if "%_P%" == "11" set _HEXVAL=B%_HEXVAL%
     if "%_P%" == "12" set _HEXVAL=C%_HEXVAL%
     if "%_P%" == "13" set _HEXVAL=D%_HEXVAL%
     if "%_P%" == "14" set _HEXVAL=E%_HEXVAL%
     if "%_P%" == "15" set _HEXVAL=F%_HEXVAL%
     set /A _VAL="%_VAL% / 16"
     if "%_VAL%" == "0" goto :endloop
     goto :hexloop
 
:InputPrompt
      @echo.
      @echo Incorrect Usage.
      @echo.
      @echo Please input a decimal value as a parameter.
      @echo.
      @echo Example:
      @echo SpeechSetting.cmd 4200
      @echo.
      @echo This example sets the MicGain as 42 percent which is 0x1068.
      @echo.
goto :end
 
:Error
@echo.
@echo Error occurred.
@echo.
goto :end
 
:endloop
     set _offset=
     set _P=
     set _VAL=
goto :eof
 
:end 
```

## <span id="Remove_Windows_apps"></span><span id="remove_windows_apps"></span><span id="REMOVE_WINDOWS_APPS"></span>Remove Windows apps


When you add language packs to an image, you’ll need to remove and reinstall each of your Windows apps to make sure they include the language assets.

Here's two scripts, one which can be used to remove the apps from an offline image, and another that can be used in audit mode to remove apps from a running image:

**Remove\_apps\_in\_offline\_image.cmd:**

This script assumes the file name is install.wim, that the script is being run from the same folder as install.wim, that the index being modified is \#1, and that there are no other Windows images in other index locations (\#2, \#3) that need to be preserved.

``` syntax
@echo off
@rem Run this script from the same folder that includes install.wim 
cls
md mount
dism /mount-image /mountdir:mount /imagefile:install.wim /index:1
if exist appx.txt del appx.txt
if exist applist.txt del applist.txt
if exist appremove.cmd del appremove.cmd
dism /image:mount /get-provisionedappxpackages > appx.txt
findstr /c:"PackageName" appx.txt > applist.txt
setlocal enabledelayedexpansion
set INTEXTFILE=applist.txt
set OUTTEXTFILE=appremove.cmd
set SEARCHTEXT=PackageName : 
set REPLACETEXT=dism /image:mount /remove-provisionedappxpackage /packagename:
set OUTPUTLINE=

for /f "tokens=1,* delims=¶" %%A in ( '"type %INTEXTFILE%"') do (
SET string=%%A
SET modified=!string:%SEARCHTEXT%=%REPLACETEXT%!

echo !modified! >> %OUTTEXTFILE%
)
del appx.txt
del applist.txt
call appremove.cmd
dism /unmount-image /mountdir:mount /commit
echo "Check to make sure operations completed successfully. If not, press Ctrl+C."
pause
ren install.wim install-temp.wim
dism /export-image /sourceimagefile:install-temp.wim /sourceindex:1 /destinationimagefile:install.wim
del install-temp.wim
```

**Remove\_apps\_in\_audit\_mode.cmd**

This script assumes you’re running in audit mode on the reference PC.

``` syntax
@echo off
@rem Use MOUNT folder in folder where script is run 
cls
dism /mount-image /mountdir:mount /imagefile:install.wim /index:1
if exist appx.txt del appx.txt
if exist applist.txt del applist.txt
if exist appremove.cmd del appremove.cmd
dism /image:mount /get-provisionedappxpackages > appx.txt
findstr /c:"PackageName" appx.txt > applist.txt
setlocal enabledelayedexpansion
set INTEXTFILE=applist.txt
set OUTTEXTFILE=appremove.cmd
set SEARCHTEXT=PackageName : 
set REPLACETEXT=dism /image:mount /remove-provisionedappxpackage /packagename:
set OUTPUTLINE=

for /f "tokens=1,* delims=¶" %%A in ( '"type %INTEXTFILE%"') do (
SET string=%%A
SET modified=!string:%SEARCHTEXT%=%REPLACETEXT%!

echo !modified! >> %OUTTEXTFILE%
)
del appx.txt
del applist.txt
call appremove.cmd
dism /unmount-image /mountdir:mount /commit
ren install.wim install-temp.wim
dism /export-image /sourceimagefile:install-temp.wim /sourceindex:1 /destinationimagefile:install.wim
del install-temp.wim
```

## <span id="BootToAudit"></span><span id="boottoaudit"></span><span id="BOOTTOAUDIT"></span>BootToAudit


Add an answer file to the Windows image in C:\\mount\\windows\\Windows\\Panther\\unattend.xml to instruct it to boot into audit mode. You can create this answer file in Windows System Image Manager.

**BootToAudit-x64:**

``` syntax
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
<!-- BootToAudit-x64.xml -->
    <settings pass="oobeSystem">
        <component name="Microsoft-Windows-Deployment" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <Reseal>
                <Mode>Audit</Mode>
            </Reseal>
        </component>
    </settings>
</unattend>
```

**BootToAudit-x86:**

``` syntax
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
<!-- BootToAudit-x86.xml -->
    <settings pass="oobeSystem">
        <component name="Microsoft-Windows-Deployment" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <Reseal>
                <Mode>Audit</Mode>
            </Reseal>
        </component>
    </settings>
</unattend>
```

## <span id="Keeping_Windows_settings_through_a_recovery"></span><span id="keeping_windows_settings_through_a_recovery"></span><span id="KEEPING_WINDOWS_SETTINGS_THROUGH_A_RECOVERY"></span>Keeping Windows settings through a recovery


Windows doesn't automatically save settings created through unattend.xml setup files, nor Windows Start Menu customizations created with LayoutModification.xml during a full-system reset, nor first-login info from oobe.xml.

To make sure your customizations are saved, that includes steps to put the unattend.xml, LayoutModification.xml, and oobe.xml files back into place. Here's some sample scripts that show how to retain these settings and put them back into the right spots. Save copies of unattend.xml, LayoutModification.xml, oobe.xml, plus these two text files, in C:\\Recovery\\OEM\\:

**ResetConfig.xml**

``` syntax
<?xml version="1.0" encoding="utf-8"?>
<!-- ResetConfig.xml -->
<Reset>
  <Run Phase="BasicReset_AfterImageApply">
    <Path>EnableCustomizationsAfterRecovery.cmd</Path>
    <Duration>2</Duration>
  </Run>
  <Run Phase="FactoryReset_AfterImageApply">
    <Path>EnableCustomizationsAfterRecovery.cmd</Path>
    <Duration>2</Duration>
  </Run>
</Reset>
```

**EnableCustomizationsAfterRecovery.cmd**

``` syntax
rem EnableCustomizationsAfterRecovery.cmd

rem Set the variable %TARGETOS%      (Typically this is C:\Windows)
for /F "tokens=1,2,3 delims= " %%A in ('reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\RecoveryEnvironment" /v TargetOS') DO SET TARGETOS=%%C

rem Set the variable %TARGETOSDRIVE% (Typically this is C:)
for /F "tokens=1 delims=\" %%A in ('Echo %TARGETOS%') DO SET TARGETOSDRIVE=%%A

rem Add back Windows settings, Start menu, and OOBE.xml customizations
copy "%TARGETOSDRIVE%\Recovery\OEM\Unattend.xml" "%TARGETOS%\Panther\Unattend.xml" /y
copy "%TARGETOSDRIVE%\Recovery\OEM\LayoutModification.xml" "%TARGETOSDRIVE%\Users\Default\AppData\Local\Microsoft\Windows\Shell\LayoutModification.xml" /y
xcopy "%TARGETOSDRIVE%\Recovery\OEM\OOBE\Info" "%TARGETOS%\System32\Info\" /s

rem Recommended: Create a pagefile for devices with 1GB or less of RAM.
wpeutil CreatePageFile /path=%TARGETOSDRIVE%\PageFile.sys /size=256


rem EnableCustomizationsAfterRecovery.cmd
set ScriptFolder=%~dp0

for /f "skip=2 tokens=2,*" %%a in ('reg.exe query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\RecoveryEnvironment" /v "TargetOS"') do set "TargetOS=%%b"
echo TargetOS: %TargetOS% >> %LogFile%
for %%A in (%TargetOS%) do set "TargetOSDrive=%%~dpA"

copy "%ScriptFolder%\Unattend.xml" "%TargetOSDrive%\Windows\Panther\Unattend.xml" /y
copy "%ScriptFolder%\LayoutModification.xml" "%TargetOSDrive%\Users\Default\AppData\Local\Microsoft\Windows\Shell\LayoutModification.xml" /y
copy "%ScriptFolder%\oobe.xml" "%TargetOSDrive%\System32\Info\Oobe.xml" /y
```

For multilingual deployments, OOBE.xml uses a more complicated folder structure. It's OK to just copy the entire folder structure into C:\\Recovery\\OEM, and then modify the script to copy the entire folder:

``` syntax
xcopy "%ScriptFolder%\Info\" "%TargetOSDrive%\System32\Info\" /s
```

To learn more about using extensibility points for push-button reset, see [Add a script to push-button reset features](http://go.microsoft.com/fwlink/?LinkId=618946).

 

 





