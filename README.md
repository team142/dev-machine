# Dev Machine

The goal of this project is to create a workable - reusable ephemeral development environment.

The idea is to use an IDE which allows for remove development through SSH.

We DO NOT want multiple users to use the same container.

We also wanted to have the ability to spin up multiple Hot and Cold
Remote Dev Instances

# How to use:
At the moment you need to have docker, k8 is optional but recommended.

## Helm and k8


## Docker

`docker build dev-machine:1 .`
`docker run -p 8022:22 dev-machine:1`

If you want to customize the zsh / keep history
Mount a directory to /home/user/userfiles

# Assumptions

* We all wanted ZSH and oh-my-zsh
* You use git
* You want to dev in node / go

# TODO

 * [ ] Add User to SUDOERS
 * [ ] Add Persistence to Directories
   * [ ] ~/.ssh
   * [x] ~/userfiles
   * [ ] ~/Code
 * [ ] Add `~/userfiles/bin` to path
 * [ ] Harden SSH
 * [x] Install Python2 & Python3
 * [ ] Go Version Manager
 * [ ] Node Version Manager
 * [x] GCP Cli
   * [x] All GCP Optional Components
 * [ ] AWS CLI

# Nice to have
 * [ ] Add Dynamic Language Support
   * [ ] Java 
   * [ ] Go 
   * [ ] Node
 * [ ] Port Forwarding as needed
 * [ ] Auto Auth Key Generation based on user
 * [ ] GCP Cli
   * [ ] Customize GCP Optional Components
