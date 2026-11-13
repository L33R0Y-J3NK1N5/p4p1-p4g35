---
title: Forcing an Intune Sync via Powershell 
description: Useful for backstage shenanigans.
publishDate: "2026-11-13"
---

```powershell
Get-ScheduledTask | where {$_.TaskName -eq 'PushLaunch'} | Start-ScheduledTask
```