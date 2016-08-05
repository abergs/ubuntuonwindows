# Bash On Ubuntu On Windows 
Here we share resources, tips, known issues etc for "Bash On Ubuntu On Windows".


## How to Instsall Bash on Ubuntu on Windows
1. Install the Windows 10 Anniversary Update
2. Go to "Turn Windows features on or off"
3. Scroll down to "Windows subsystem for Linux (Beta)" 
 

(video: http://www.hanselman.com/blog/VIDEOHowToRunLinuxAndBashOnWindows10AnniversaryUpdate.aspx)

## Start an bash ssh-agent on launch
1. Open the config  

    `$ nano ~/.bashrc`

2. Add the following somewhere:
   ``` 
   if [ ! -S ~/.ssh/ssh_auth_sock ]; then
      eval `ssh-agent`
      ln -sf "$SSH_AUTH_SOCK" ~/.ssh/ssh_auth_sock
    fi
    export SSH_AUTH_SOCK=~/.ssh/ssh_auth_sock
    ssh-add -l | grep "The agent has no identities" && ssh-add
```

## Use Windows 10 Virtual desktop to have your own workspace
Create a new virtual dekstop from `Win+Tab` and setup your ubuntu workspace. Or run 4 terminals on that screen, for different ssh sesssoin for example. Switch desktops easily and fast by either `Win+Ctrl+left` / `Win+Ctrl+right` or `win+tab tab enter`


## How to access the filesystem from Windows / Ubuntu

In Ubuntu, you can find your entire C drive under `/mnt/c`. (You have the same permissions as the User you launched Ubuntu with)

In Windows, you can find your entire Ubuntu installation under `%LocalAppData%\lxss`. (C:/Users/YOURUSERNAME/AppData/Local/lxss)
