# Dev Machine

The goal of this project is to create a workable - reusable ephemeral development environment.

The idea is to use an IDE which allows for remove development through SSH.

# How to use:
At the moment you need to have docker, k8 is optional but recommended.

`docker build dev-machine:1 .`
`docker run -p 8022:22 dev-machine:1`

# TODO

 * [ ] Harden SSH
 * [ ] Node Version Manager

# Nice to have
 * [ ] Add Dynamic Language Support
 * [ ] Port Forwarding as needed
