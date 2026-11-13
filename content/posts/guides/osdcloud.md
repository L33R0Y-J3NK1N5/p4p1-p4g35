---

title: "Using OSDCloud"
description: "This post outlines how I use OSDCloud for clean Windows installations"
publishDate: "13 November 2026"
updatedDate: "13 November 2026"
tags: ["guides", "osdcloud"]
pinned: true
---

# Overview

OSDCloud can be used to install a fresh, bloat-free copy of Windows 11 on a device.

This can be very useful for a quick wipe and fresh Windows installation that doesn't include all crappy vendor bloat and unneccessary drivers.

This guide outlines how to set up and use OSDCloud for this purpose.

# Building an OSDCloud USB

Setting up the USB itself is surprisingly simple, even though some guides make it a lot more complicated than it needs to be.

1. Download Windows ADK + WinPE add-on for Windows ADK (https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)

2. Install Windows ADK, select 'Deployment Tools' under the features to install. Install the add-on afterward

3. Install the OSD PowerShell Module
```powershell
Set-ExecutionPolicy RemoteSigned -Force
Install-Module OSD -Force
Import-Module OSD
```

4. Create the OSDCloud Template
```powershell
New-OSDCloudTemplate -Language en-us -Verbose
```

5. Set the OSDCloud Workspace location (can be changed)
```powershell
$WorkingDir="C:\OSDCloud"
```

6. Create the OSDCloud Workspace
```powershell
New-OSDCloudWorkSpace -WorkspacePath $WorkingDir -Verbose
```

7. Set any boot parameters (optional)
```powershell
# If you want GUI/manual install (preferred for flexibility)
Edit-OSDCloudWinPE -StartOSDCloudGUI
# If you want zero-touch install (can be convenient)
Edit-OSDCloudWinPE -CloudDriver * -StartOSDCloud "-OSVersion 'Windows 11' -OSEdition Pro -OSLanguage en-us -OSActivation Retail -ZTI -Restart"
```

8. Once set up, you can either install it directly onto a USB or turn it into an ISO (for use with Rufus, etc.)
```powershell
# If making an ISO for Rufus
New-OSDCloudISO -WorkspacePath $WorkingDir
# If making it directly on the USB
New-OSDCloudUSB -WorkspacePath $WorkingDir
```

# Using an OSDCloud USB

Once you have powered the device off and assigned it to the appropriate user via Intune, boot and install using the USB

1. Plug the OSDCloud USB into the target device

2. Press the pre-boot menu key during startup (F12 for Dell)

3. Select the USB as the boot option  
(NOTE: USB boot may be blocked in the BIOS settings, make sure USB boot is enabled in the BIOS or it will not show up)

4. If it does not open automatically, type the following to start the GUI
```powershell
Start-OSDCloudGUI
```

5. Select the relevant Driver Pack (should set itself automatically), OS version and details (generally Pro, en-us, Retail) and press 'Start'.

6. Wait for the installation to finish, it should restart itself automatically.

7. If successful, Windows will boot to the startup screen. (or Autopilot OOBE login screen if company-joined)


# References

https://akosbakos.ch/osdcloud-1-basics/  
https://www.osdcloud.com/  
https://www.osdsune.com/home/archive/deployment/osdcloud-zti-way  
https://4sysops.com/archives/deploy-windows-11-with-osdcloud/  