# Bash On Ubuntu On Windows 
Here we share resources, tips, known issues etc for "Bash On Ubuntu On Windows".


## How to Instsall Bash on Ubuntu on Windows
1. Install the Windows 10 Anniversary Update
2. Go to "Turn Windows features on or off"
3. Scroll down to "Windows subsystem for Linux (Beta)"


## Start an bash ssh-agent
1. Open the config

    $ nano ~/.bashrc

2. Add the following somewhere:


    `if [ ! -S ~/.ssh/ssh_auth_sock ]; then
      eval `ssh-agent`
      ln -sf "$SSH_AUTH_SOCK" ~/.ssh/ssh_auth_sock
    fi
    export SSH_AUTH_SOCK=~/.ssh/ssh_auth_sock
    ssh-add -l | grep "The agent has no identities" && ssh-add`
