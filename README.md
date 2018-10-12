# win10-ubuntu

## Installing Ubuntu on Windows 10

## Install the Windows Subsystem for Linux

1. Open PowerShell as Administrator and run

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

2. Restart your computer when prompted.

## Install your Linux Distribution of Choice

1. Open the Microsoft Store and choose your favorite Linux distribution.

[Ubuntu](https://www.microsoft.com/store/p/ubuntu/9nblggh4msv6)

[OpenSUSE](https://www.microsoft.com/store/apps/9njvjts82tjx)

[SLES](https://www.microsoft.com/store/apps/9p32mwbh6cns)

[Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)

[Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)

2. From the distro's page, select "Get"

## Complete initialization of your Linux distribution

Now that your Linux distribution is installed, you must initialize your new distribution instance once, before it can be used.

## Optimizations

Windows Defender with Real Time Protection check all files handled by Linux Distribution, but it's not recommended to disable this feature.

The solution is to exclude the Linux Distribution folder and other folder that you use for linux from Windows Defender's Virus & Threat protection.

```
Windows 10 > Setting > Update & Security > Windows Defender > Open Windows Defender Security Center > Virus & Threat protection > Virus & Threat protection settings > Exclusions > Add or remove exclusions > Add an exclusions > Folder
```

The folder to your Linux Distribution you can find in

```
C:\Users\{USERNAME}\AppData\Local\Packages
```

#### Example: Ubuntu

```
C:\Users\{USERNAME}\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc
```

> Replace {USERNAME} with you Windows 10 username

## Requirements

Windows build 16215 or later