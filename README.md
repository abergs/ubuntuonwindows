# Bash On Ubuntu On Windows 
This is a collaborative document where we help **new bash users** get the basics things working in Bash. It's especially targeted for the users of *Bash On Ubuntu On Windows* - where the bash environment is fairly new.

In other words: Here we share resources, tips, known issues etc for *Bash On Ubuntu On Windows*.
[![Analytics](https://ga-beacon.appspot.com/UA-37656767-10/repo-readme)](https://github.com/abergs/ubuntuonwindows)

## 1. How to Install Bash on Ubuntu in Windows 10 (Windows Subsystem for Linux)
1. Install the Windows 10 Anniversary Update
2. Go to "Turn Windows features on or off"
3. Scroll down to "Windows subsystem for Linux (Beta)" 
 

(video: http://www.hanselman.com/blog/VIDEOHowToRunLinuxAndBashOnWindows10AnniversaryUpdate.aspx)

## 2. Start an bash ssh-agent on launch
2.1: Open the config  

    `$ nano ~/.bashrc`

2.2: Add the following somewhere:
   ``` 
    #!/bin/bash
    
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
2.3: Then run `source ~/.bashrc` to reload your config.


## 3. Troubleshoot ssh-agent forwarding
Connect with ssh and check if you forward your keys by running `echo "$SSH_AUTH_SOCK"`. If you get no output, that means it's not working. Make sure it's running (above script should work) and that your `~/.ssh/config` is configured to run ForwardAgent, for example:

```
Host 123.456.123.45
 ForwardAgent yes
```


## 4. Use Windows 10 Virtual desktop to have your own workspace
Create a new virtual desktop from `Win+Tab` and setup your ubuntu workspace. Or run 4 terminals on that screen, for different ssh sessions for example. Switch desktops easily and fast by either `Win+Ctrl+left` / `Win+Ctrl+right` or `win+tab tab enter`


## 5. How to access the filesystem from Windows / Ubuntu

In Ubuntu, you can find your entire C drive under `/mnt/c`. (You have the same permissions as the User you launched Ubuntu with)

In Windows, you can find your entire Ubuntu installation under `%LocalAppData%\lxss`. (C:/Users/YOURUSERNAME/AppData/Local/lxss)

## 6. Make full use of Interop
With [interoperability](https://msdn.microsoft.com/en-us/commandline/wsl/interop), you can open Windows programs from WSL. Here are some ways to use to your advantage:

### 6.1: Set a Default Windows Browser
Some commands, such as Heroku CLI's `heroku open`, need to open a browser. There's no default browser in WSL by default, but one easy way to set this up is by adding the following to `~/.bashrc`: 

```
# replace with relevant browser
export BROWSER=/mnt/c/Program\ Files\ \(x86\)/Google/Chrome/Application/chrome.exe
```

### 6.2: Use Symlinks to open Windows Programs
One use case is to open your text editor to the current directory. Interop + symlinks make this possible. For example:
```
ln -s /mnt/c/Program\ Files/Microsoft\ VS\ Code/Code.exe code
```
Now in any directory, type `code` and your text editor opens. Even better, type `code .` and it opens that directory, ready for editing.

## 7. Disable ding/beep/bell sound when tabbing
You know that annoying bell sound you get when you try to autocomplete something and it doesn't exist? It's super loud and annoying, so lets mute it. Run this command and restart your shell to give peace to your ears:  

`echo 'set bell-style none' >> ~/.inputrc`  

Restart your shell and it's quiet :)


## 8.  How to configure proxy settings in apt

If your network uses a proxy-server, services like apt-get, git, wget, and curl, etc. would not be able to access internet directly.

There is an open source tool : [ProxyMan](https://github.com/himanshub16/ProxyMan), which lets you easily configure system-wide proxy settings from the command-line in one go. [Download latest release](https://github.com/himanshub16/ProxyMan/releases/latest/).
*As of now, ProxyMan is capable of managing GNOME desktop, /etc/environment, .bashrc, apt.conf, git, npm, and Dropbox proxy settings*

However, you can also manually modify the configuration file.

To add prox in apt, modify `/etc/apt/apt.conf` and add the following:
```
   Acquire::http::Proxy "http://username:password@proxy.server:port";
   Acquire::https::Proxy "https://username:password@proxy.server:port";
   Acquire::ftp::Proxy "ftp://username:password@proxy.server:port";
   Acquire::socks::Proxy "socks://username:password@proxy.server:port";
```
Do a `sudo apt-get update` afterwards to update repository infromation

Example: 

username: `johnwick` password `password` proxy-server `proxy.foobar.com` port `8080`

`
   Acquire::http::Proxy "http://johnwick:password@proxy.foobar.com:8080";`

To add system wide proxy settings, go to `/etc/environment` and add the following:

```
   export http_proxy="http://username:password@proxy.server:port"
   export https_proxy="https://username:password@proxy.server:port"
   export ftp_proxy="ftp://username:password@proxy.server:port"
   export socks_proxy="socks://username:password@proxy.server:port"
```

Example: 

`  export http_proxy="http://johnwick:password@proxy.foobar.com:8080";`
  
  Use `source /etc/environment` to load the new environment variables.
  
To make git work behind proxy use the following commands

```
git config --global http.proxy http://johnwick:password@proxy.foobar.com:8080`
git config --global https.proxy https://johnwick:password@proxy.foobar.com:8080
```
## 9. Program compatibility - What works, what doesn't?
This [crowdsourced list of programs and their compatibility](https://github.com/ethanhs/WSL-Programs) gives you a searchable list for compatibility. Want to know if `apt` works 100%? Just check the list. Also worth a mention, the [Official repository](https://github.com/microsoft/BashOnWindows/) contains a full list of all issues reported.

## 10. Mapping a Driver Letter to your linux root folder

Open a Windows Terminal (cmd) and:

```
subst l: c:\Users\path\to\your\rootfs
```

Now you can access the root linux folder typing `l:` in the Windows Command, or Explorer.

Your rootfs might be located in different paths: [Check this guide on askubuntu to find your linux folder on windows](https://askubuntu.com/questions/759880/where-is-the-ubuntu-file-system-root-directory-in-windows-nt-subsystem-and-vice)
