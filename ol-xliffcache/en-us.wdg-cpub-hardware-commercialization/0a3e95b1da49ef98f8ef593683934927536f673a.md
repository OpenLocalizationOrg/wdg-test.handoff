---
author: Justinha
Description: 'Oobe.xml Settings'
ms.assetid: 2c8ecddc-7099-451e-a069-642f654d4fbf
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: 'Oobe.xml Settings'
---

# Oobe.xml Settings


This topic describes the settings that can be set in Oobe.xml.

## <span id="oobe.xml_settings"></span><span id="OOBE.XML_SETTINGS"></span>Oobe.xml Settings


The following tables show the available settings in the Oobe.xml file, by section, along with the description and the value for each setting.

**HID Pairing**

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Setting</th>
<th align="left">Description</th>
<th align="left">Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>mouseImagePath</p></td>
<td align="left"><p>Absolute path to the mouse pairing instruction image.</p>
<p>The image must not be larger than 630 x 372 pixels. It's scaled to fit in portrait mode or on small form factors.</p></td>
<td align="left"><p>Absolute path to image</p></td>
</tr>
<tr class="even">
<td align="left"><p>mouseText</p></td>
<td align="left"><p>Help text that displays at the bottom of the page if the mouse pairing instruction image cannot be opened. This text is also used by Narrator for accessibility.</p></td>
<td align="left"><p>String</p></td>
</tr>
<tr class="odd">
<td align="left"><p>mouseErrorImagePath</p></td>
<td align="left"><p>Absolute path to the mouse pairing error image.</p>
<p>The image must not be larger than 630 x 372 pixels. It will be scaled to fit in portrait mode or on small form factors.</p></td>
<td align="left"><p>Absolute path to the image</p></td>
</tr>
<tr class="even">
<td align="left"><p>mouseErrorText</p></td>
<td align="left"><p>This error text is displayed if there are issues loading the mouse pairing error image. This text is also used by Narrator for accessibility.</p></td>
<td align="left"><p>String</p></td>
</tr>
<tr class="odd">
<td align="left"><p>keyboardImagePath</p></td>
<td align="left"><p>Absolute path to the first keyboard pairing instruction image.</p>
<p>The image must not be larger than 630 x 372 pixels. It will be scaled to fit in portrait mode or on small form factors.</p></td>
<td align="left"><p>Absolute path to the image</p></td>
</tr>
<tr class="even">
<td align="left"><p>keyboardText</p></td>
<td align="left"><p>Help text that displays at the bottom of the page if the keyboard pairing instruction image cannot be opened. This text is also used by Narrator for accessibility.</p></td>
<td align="left"><p>String</p></td>
</tr>
<tr class="odd">
<td align="left"><p>keyboardPINImagePath</p></td>
<td align="left"><p>Absolute path to the second keyboard pairing instruction image.</p>
<p>The image must not be larger than 630 x 372 pixels. It's scaled to fit in portrait mode or on small form factors.</p></td>
<td align="left"><p>Absolute path to the image</p></td>
</tr>
<tr class="even">
<td align="left"><p>keyboardErrorImagePath</p></td>
<td align="left"><p>Absolute path to the keyboard pairing error image.</p>
<p>The image must not be larger than 630 x 372 pixels. It's scaled to fit in portrait mode or on small form factors.</p></td>
<td align="left"><p>Absolute path to the image</p></td>
</tr>
<tr class="odd">
<td align="left"><p>keyboardErrorText</p></td>
<td align="left"><p>This error text is displayed if there are issues loading the keyboard pairing error image. This text is also used by Narrator for accessibility.</p></td>
<td align="left"><p>String</p></td>
</tr>
</tbody>
</table>

 

**Registration page**

The following table shows the Registration page settings and their allowed values in OOBE.xml.

**Note**  
The ampersand symbol, '&', cannot be used in this section of OOBE.xml. Use the word 'and' instead.

 

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Section</th>
<th align="left">Setting</th>
<th align="left">Description</th>
<th align="left">Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>title</p></td>
<td align="left"><p>Required. Text to title the Registration page.</p></td>
<td align="left"><p>String of up to 25 characters.</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>subtitle</p></td>
<td align="left"><p>Required. Text to describe the Registration page.</p></td>
<td align="left"><p>String of up to 200 characters.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>textbox1</strong></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Text to label textbox1. Required for textbox1 to appear.</p></td>
<td align="left"><p>String of up to 20 characters.</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>inputscope</p></td>
<td align="left"><p>Value to set scope of touch keyboard.</p></td>
<td align="left"><p>Two supported scopes:</p>
<p>4 (IS_EMAIL_USERNAME)</p>
<p>29 (IS_NUMBERS)</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>textbox2</strong></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Text to label textbox2. Required for textbox2 to appear.</p>
<p></p></td>
<td align="left"><p>String of up to 20 characters.</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>inputscope</p></td>
<td align="left"><p>Value to set scope of touch keyboard.</p></td>
<td align="left"><p>Two supported scopes:</p>
<p>4 (IS_EMAIL_USERNAME)</p>
<p>29 (IS_NUMBERS)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>checkbox1</strong></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Text to label checkbox1. Required for checkbox1 to appear.</p></td>
<td align="left"><p>String of up to 250 characters. We strongly recommend that you use no more than 100 characters because this length of text will fit on one line.</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>defaultvalue</p></td>
<td align="left"><p>Value to set checkbox1 as selected or not selected.</p></td>
<td align="left"><p>True or False. True means the check box default condition is selected. False means the check box default condition is not selected.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>checkbox2</strong></p>
<p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Text to label checkbox2. Required for checkbox2 to appear</p></td>
<td align="left"><p>String of up to 250 characters. We strongly recommend that you use no more than 100 characters because this length of text will fit on one line.</p>
<p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>defaultvalue</p></td>
<td align="left"><p>Value to set checkbox2 as selected or not selected.</p></td>
<td align="left"><p>True or False. True means the check box default condition is selected. False means the check box default condition is not selected.</p>
<p></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>checkbox3</strong></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Text to label checkbox3. Required for checkbox3 to appear</p></td>
<td align="left"><p>String of up to 250 characters. We strongly recommend that you use no more than 100 characters because this length of text will fit on one line.</p>
<p></p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>defaultvalue</p></td>
<td align="left"><p>Value to set checkbox3 as selected or not selected.</p></td>
<td align="left"><p>True or False. True means the check box default condition is selected. False means the check box default condition is not selected.</p>
<p></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>link1</strong></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Label for the link to the <strong>.rtf</strong> file. Required for link1 to appear.</p></td>
<td align="left"><p>String of up to 100 characters.</p>
<p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>link</p></td>
<td align="left"><p>File must be named linkfile1.rtf. OOBE searches for files named linkfile1.rtf, linkfile2.rtf, or linkfile3.rtf under the oobe\info folder. OOBE searches for files under the appropriate locale and language specific subfolders of oobe\info.</p></td>
<td align="left"><p>linkfile1.rtf</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>link2</strong></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Label for the link to the <strong>.rtf</strong> file. Required for link2 to appear.</p></td>
<td align="left"><p>String of up to 100 characters.</p>
<p></p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>link</p></td>
<td align="left"><p>File must be named linkfile2.rtf. OOBE searches for files named linkfile1.rtf, linkfile2.rtf, or linkfile3.rtf under the oobe\info folder. OOBE searches for files under the appropriate locale and language specific sub-folders of oobe\info.</p></td>
<td align="left"><p>linkfile2.rtf</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>link3</strong></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>label</p></td>
<td align="left"><p>Label for the link to the <strong>.rtf</strong> file. Required for link3 to appear.</p></td>
<td align="left"><p>String of up to 100 characters.</p>
<p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>link</p></td>
<td align="left"><p>File must be named linkfile3.rtf. OOBE searches for files named linkfile1.rtf, linkfile2.rtf, or linkfile3.rtf under the oobe\info folder. OOBE searches for files under the appropriate locale and language specific sub-folders of oobe\info.</p></td>
<td align="left"><p>linkfile3.rtf</p></td>
</tr>
</tbody>
</table>

 

**EULA file and language, locale, and timezone default values.**

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Section</th>
<th align="left">Setting</th>
<th align="left">Description</th>
<th align="left">Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>eulafilename</p></td>
<td align="left"><p>Language- and location-specific version of manufacturer end-user license agreement (EULA).</p></td>
<td align="left"><p>.rtf file.</p></td>
</tr>
<tr class="even">
<td align="left"><p>defaults</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>language</p></td>
<td align="left"><p>Decimal identifier for input locale.</p></td>
<td align="left"><p>Decimal identifier for input locale. These values can be found in the following topic, [Default Input Locales for Windows Language Packs](default-input-locales-for-windows-language-packs.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>location</p></td>
<td align="left"><p>The location is specified by using a GEOID value that is converted to its decimal value.</p></td>
<td align="left"><p>For a list of GEOIDs, see this [MSDN Web site](http://go.microsoft.com/fwlink/?LinkId=141804).</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>locale</p></td>
<td align="left"><p>The locale is specified by using a locale identifier (LCID) value.</p></td>
<td align="left"><p>For a full list of LCIDs, see this [Microsoft Global Development Web site](http://go.microsoft.com/fwlink/?LinkId=63028). For a list of LCIDs and the versions of Windows in which they are available, download [&quot;Windows Language Code Identifier (LCID) Reference&quot;](http://go.microsoft.com/fwlink/?LinkId=140989) from MSDN. In this paper, in the left pane, go to &quot;Appendix A: Windows Behavior&quot; to see a table that shows the LCID and the Windows release in which it is available.</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>keyboard</p></td>
<td align="left"><p>Specifies the keyboard layout.</p></td>
<td align="left"><p>Use the keyboard value that is listed in the registry under <strong>HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts</strong>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>timezone</p></td>
<td align="left"><p>Specifies the time zone of the computer's end user. The time zone is set by a string that specifies the time zone for the computer. The maximum length is 256 characters. New time zones might appear in future releases. To add support for a new time zone, you must enter the exact time zone string.</p></td>
<td align="left"><p>For a full list of time zones, refer to the values listed in the registry under <strong>HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Time Zones</strong> on a computer running Windows® 7. On a computer running Windows 7 you can use the tzutil command-line tool to list the time zone for that computer. The tzutil tool is installed by default on Windows Server 2008 R2.</p>
<div class="alert">
<strong>Warning</strong>  
<p>If the time zone is not specified, a default time zone value is used. The default time zone is based on the installed language and region specified in an answer file. If a region has more than one time zone, the time zone is set to the default time zone of that region. The default time zone for that region is specified by the location of the capital/major city. For example, if en-CA is specified, <strong>Eastern Standard Time</strong> is used as the default time zone because the Canadian capital, Ottawa, uses Eastern Standard Time. If en-US is specified as the <code>UserLocale</code>, the time zone of the Windows installation defaults to Pacific Standard Time.</p>
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>adjustForDST</p></td>
<td align="left"><p>Specifies whether to adjust for Daylight Savings Time. This setting is effective only when used in combination with the <code>timezone</code> and <code>hideTimeAndDate</code> settings to specify the time settings for the end user.</p></td>
<td align="left"><p>True or False.</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p>hideRegionalSettings</p></td>
<td align="left"><p>If this setting is not specified as <strong>True</strong>, then the Regional Settings page will be shown, even if all of the values for that page have been preconfigured.</p></td>
<td align="left"><p>True or False.</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>hideTimeAndDate</p></td>
<td align="left"><p>If this setting is not specified as <strong>True</strong>, then the Time and Date page will be shown, even if all of the values for that page have been preconfigured.</p></td>
<td align="left"><p>True or False.</p></td>
</tr>
</tbody>
</table>

 

## <span id="How_to_Customize_OOBE"></span><span id="how_to_customize_oobe"></span><span id="HOW_TO_CUSTOMIZE_OOBE"></span>How to Customize OOBE


**To customize OOBE by using Oobe.xml**

1.  Create a file named Oobe.xml and store this file in Windows\\System32\\Oobe\\Info.

2.  By using an XML editor or a text editor, such as Notepad, update Oobe.xml with the appropriate files, paths, and content.

3.  Save your updated version of Oobe.xml in Windows\\System32\\Oobe\\Info, or in the appropriate language- and locale-specific folders required for your customizations.

4.  Test OOBE.

    **Test OOBE**

    1.  On the **Start** menu, point to **All Programs**, and then click **Accessories**.

    2.  Right-click the command prompt shortcut, and click **Run as administrator**. Accept the **User Account Control** dialog box.

    3.  Navigate to \\Windows\\System32\\Sysprep

    4.  Run **sysprep /oobe**.

    5.  Start the computer.

## <span id="related_topics"></span>Related topics


[Configure Oobe.xml](configure-oobexml.md)

 

 






