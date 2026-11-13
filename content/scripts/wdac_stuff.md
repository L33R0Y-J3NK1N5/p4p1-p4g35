---
title: Checking WDAC Logs 
description: Random WDAC log collection scripts.
publishDate: "2026-11-13"
---

WDAC is annoying and has a separate log location for script and app blocks.

Within Powershell:
```powershell
Get-WinEvent -LogName "Microsoft-Windows-CodeIntegrity/Operational" | Where-Object {$_.Id -in 3076, 3077} | Select-Object TimeCreated, Message | Format-List
```
```powershell
Get-WinEvent -LogName "Microsoft-Windows-AppLocker/MSI and Script" -ErrorAction Ignore | Where-Object {$_.Id -eq 8029 -or $_.Id -eq 8028} | Select-Object TimeCreated, Id, Message | Format-List
```

Export to CSV:
```powershell
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-CodeIntegrity/Operational'; ID=3076,3077} | Select-Object TimeCreated, ProviderName, Id, Message | Export-Csv -Path "C:\temp\WDAC_Logs.csv" -NoTypeInformation
```

Export EVTX (Event Viewer):
```powershell
wevtutil epl "Microsoft-Windows-AppLocker/MSI and Script" C:\wdac\ScriptBlocks.evtx /q:"*[System[(EventID=8029)]]"
```