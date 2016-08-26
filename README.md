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
1. Open the config  

    `$ nano ~/.bashrc`

2. Add the following somewhere:
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
Then run `source ~/.bashrc` to reload your config.

## 3. Troubleshoot ssh-agent forwarding
Connect with ssh and check if you forward your keys by running `echo "$SSH_AUTH_LOCK"`. IF you get no output, that means it's not working. Make sure it's running (above script should work) and that your `~/.ssh/config` is configured to run ForwardAgent, for example:

```
Host 123.456.123.45
 ForwardAgent yes
```


## 4. Use Windows 10 Virtual desktop to have your own workspace
Create a new virtual desktop from `Win+Tab` and setup your ubuntu workspace. Or run 4 terminals on that screen, for different ssh sessions for example. Switch desktops easily and fast by either `Win+Ctrl+left` / `Win+Ctrl+right` or `win+tab tab enter`


## 5.  How to access the filesystem from Windows / Ubuntu

In Ubuntu, you can find your entire C drive under `/mnt/c`. (You have the same permissions as the User you launched Ubuntu with)

In Windows, you can find your entire Ubuntu installation under `%LocalAppData%\lxss`. (C:/Users/YOURUSERNAME/AppData/Local/lxss)

## 6. Disable ding/beep/bell sound when tabbing
You know that annoying bell sound you get when you try to autocomplete something and it doesn't exist? It's super loud and annoying, so lets mute it. Run this command and restart your shell to give peace to your ears:  

`echo 'set bell-style none' >> ~/.inputrc`  

Restart your shell and it's quiet :)

