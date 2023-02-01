# Notes on PS commands for $PATH

## Introduction
My notes on PS cammoands and WiP scripts for modyfyinmg $PATH on Windows systems.
Purely for my education to learn more on PowerShell and Windows systems.

### Purpose
To modify $PATH variable on windows, need as I have added a few standalon programs and want these to be easily accesssilble from the command line.
To also learn PowerShell

### References
[StackOverflow Post - Setting Windows PowerShell environment variables](https://stackoverflow.com/questions/714877/setting-windows-powershell-environment-variables)

[StackOverflow Post - PowerShell: how to delete a path in the Path environment variable](https://stackoverflow.com/questions/39010405/powershell-how-to-delete-a-path-in-the-path-environment-variable)

[MS Learn PowerShell - about Environment Variables](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-5.1)

### Note on Environment $PATH

 "HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Control\\Session Manager\\Environment" (HKLM) are system variables - *Needs Admin Powershell*
	 
"HKEY_CURRENT_USER\\Environment" hive (HKCU) a path variable only available when user is logged in - *Can be done by user*
	 
## User and System powershell to add and Remove Directory

### How To

#### Add directory *Permanently* to $PATH
Set registry that is to be modified (HKLM/HKCU, note above)
```powershell
$regLocation = "Registry::HKEY_CURRENT_USER\Environment"
```
Backup the current $PATH
```powershell
$oldPath = (Get-ItemProperty -Path $regLocation -Name PATH).path
$oldPath
```
Add the new directory to path (spaces allowed)
```powershell
$newPath = “$oldPath;c:\Test\Path\ To Add”
$newPath
```
Set the new $PATH
```powershell
Set-ItemProperty -Path $regLocation -Name PATH -Value $newPath
```
Check the new path
```powershell
(Get-ItemProperty -Path $regLocation -Name PATH).path
```


#### Remove directory *Permanently* from $PATH
```powershell
Set registry that is to be modified (HKLM/HKCU, note above)
$regLocation = "Registry::HKEY_CURRENT_USER\Environment"
```
Check original path
```powershell
(Get-ItemProperty -Path $regLocation -Name PATH).path
```
Backup the current $PATH
```powershell
$oldPath = (Get-ItemProperty -Path $regLocation -Name PATH).path
```
Set the newpath variable for modification
```powershell
$newPath = $oldPath
```
Set a remove varible for the directory string to be removed
```powershell
$removeDir = “c:\Test\Path\ To Remove”
```
Modify $newpath to remove desired old directory by split, delete and join on the ';' seperator, check changes
```powershell
$newPath = ($newPath.Split(';') | Where-Object { $_ -ne "$removeDir" }) -join ';'
$newPath
```
Set the new $PATH
```powershell
Set-ItemProperty -Path $regLocation -Name PATH -Value $newPath
```
Check the new path
```powershell
(Get-ItemProperty -Path $regLocation -Name PATH).path
```


---




### As a script
!!! ** WiP ** !!!
STILL TO COMPLETE

