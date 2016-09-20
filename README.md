# KorpoCleaner

This is instruction how to rid my company laptop of company bloatware, spyware and preapre it for normal use.

1. Request Local Admin Account for recovery purposes. Then check if it works.
2. Open Computer Management (compmgmt.msc). Go to "Local Users and Groups"/Groups. Open Administrators. Add your normal domain user.
3. Sign out and Sign in to refresh Administrator privileges
4. Download and install PsExec from Chocolatey (**choco install sysinternals**) or directly from https://technet.microsoft.com/en-us/sysinternals/pxexec.aspx
5. Open CMD as Administrator and run **PSEXEC -i -s -d CMD** to open CMD as LocalSystem
6. Run **REG ADD "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t reg_dword /d 4 /f** as LocalSystem to disable Group Policy Client and stop applying corporate Group Policy. ***WARNING: After this step only admin account will be able to login your computer!***
7. Restart computer **shutdown /r /t 0 /f**
8. Run followed scripts:

### Remove corporate wallpaper
```powershell
Remove-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\ActiveDesktop NoChangingWallPaper
Remove-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\System Wallpaper
Remove-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\System WallpaperStyle
```

### Remove corporate lockscreen
```powershell
Remove-ItemProperty HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization NoChangingLockScreen
Remove-ItemProperty HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization LockScreenOverlaysDisabled
Remove-ItemProperty HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization LockScreenImage
```

### Reanable Windows Update
```powershell
Remove-ItemProperty HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate DisableWindowsUpdateAccess
Remove-ItemProperty HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer NoWindowsUpdate
```

### Allow Windows to remember your user
```powershell
Remove-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System dontdisplaylastusername
Remove-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System dontdisplaylockeduserid
```

### Disable DailyAdmin service (Corporate spying application)
```powershell
Set-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\dagkgsvc -Name Start -Value 4
```

### Remove "Network Share not found" warning
```powershell
Set-ItemProperty HKCU:\Network -Name RestoreDiskChecked -Value 0
```

### Fix Build-In admin 
```powershell
Set-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System -Name FilterAdministratorToken -Value 1
Set-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\UIPI -Name '(Default)' -Value 1
```

### Remove Group Policy warning (*Not jet working*)
```cmd
REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Winlogon\Notifications\Components\GPClient" /va /f
```

