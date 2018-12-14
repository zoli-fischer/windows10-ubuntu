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

#### Example: Ubuntu 18

```
C:\Users\{USERNAME}\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc
```

#### Example: Debian

```
C:\Users\{USERNAME}\AppData\Local\Packages\TheDebianProject.DebianGNULinux_76v4gfsz19hv4
```

> Replace {USERNAME} with you Windows 10 username

## Before you start - Install essentials

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential gettext bash-completion openssh-client curl
```

## Fix file permission problems

In some cases a file/folder in Windows 10 drives (c:, d: etc.) is not writable or not found. Example: npm module less-loader, file access permission problems.

> To fix this the Linux Distribution application needs to be run as administrator.

Steps to make run the Linux Distribution application always as administrator:

> For ilustration I will use Ubuntu 18.04

1. Start the Ubuntu 18.04 application
2. Open the 'Task Manager'
3. In Processes > Apps search for Ubuntu 18.04 and expand it 
4. In the list right click on ubuntu 1804 and select Properties
5. Under 'Compatibility' tab check the checkbox for 'Run this program as an administrator'
6. Click OK to close the window

## Chmod/Chown WSL Improvements

> By default there is no support for chmod and chown for files in /mnt/c.

```
sudo umount /mnt/c
sudo mount -t drvfs C: /mnt/c -o metadata
```

This will only affect the current instance.

## Errors & fixes

### ssh ForwardAgent not forwarding key

The problem is that the ssh is not started when you open a new session. With this fix the ssh agen will start on every new session.

> This fix not works on Ubuntu

> Source: https://stackoverflow.com/questions/18404272/running-ssh-agent-when-starting-git-bash-on-windows/45562886#45562886

1. Edit ~/.bashrc
2. Add at the end
```
# Set up ssh-agent
SSH_ENV="$HOME/.ssh/environment"

function start_agent {
    echo "Initializing new SSH agent..."
    touch $SSH_ENV
    chmod 600 "${SSH_ENV}"
    /usr/bin/ssh-agent | sed 's/^echo/#echo/' >> "${SSH_ENV}"
    . "${SSH_ENV}" > /dev/null
    /usr/bin/ssh-add
}

# Source SSH settings, if applicable
if [ -f "${SSH_ENV}" ]; then
    . "${SSH_ENV}" > /dev/null
    kill -0 $SSH_AGENT_PID 2>/dev/null || {
        start_agent
    }
else
    start_agent
fi
```
3. Reload the configuration
```
source ~/.bashrc
```
4. Create ssh config file
```
touch ~/.ssh/config
```
5. Add your host configuration to ```touch ~/.ssh/config```. You can add multiple host by repeating this.
```
Host {HOST}
HostName {HOSTALIAS}
User {USER}
PreferredAuthentications publickey
IdentityFile {PATH_TO_YOUR_PRIVATE_KEY}
AddKeysToAgent yes
ForwardAgent yes
```
> Don't forget to change {HOST}, {HOSTALIAS}, {USER} and {PATH_TO_YOUR_PRIVATE_KEY}

### invoke-rc.d: could not determine current runlevel

> Source: https://github.com/Microsoft/WSL/issues/1761

1. Edit /etc/init.d/ebtables
2. Search for
```
case "$1" in
    start)
        [ "$EBTABLES_LOAD_ON_START" = "yes" ] && load
        ;;
    stop)
        [ "$EBTABLES_SAVE_ON_STOP" = "yes" ] && save
        clear
        ;;
```
3. Comment out the actions taken by stop)
```
    stop)
        # [ "$EBTABLES_SAVE_ON_STOP" = "yes" ] && save
        # clear
        ;;
```

## Requirements

Windows build 16215 or later