FROM ubuntu:20.04 as base

RUN apt update && apt install  openssh-server sudo -y
# This is to avoid "sshd: no hostkeys available -- exiting"
RUN ssh-keygen -A
# This is to avoid "Missing privilege separation directory: /run/sshd"
RUN mkdir -p -m0755 /var/run/sshd

FROM base as layer2
RUN apt install aria2 -y

FROM layer2 as layer3
RUN apt install software-properties-common -y
RUN add-apt-repository ppa:apt-fast/stable -y
RUN apt-get update

FROM layer3 as layer4
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y apt-fast

FROM layer4 as layer5
RUN apt-fast install git htop byobu python2 python3 -y


RUN useradd -rm -d /home/user -s /bin/bash -g root -G sudo -u 1000 user
RUN echo 'user:user' | chpasswd
RUN echo "user	ALL=(ALL:ALL) ALL" >> /etc/sudoers

#RUN service ssh start

FROM layer5 as layer6
USER root
RUN apt-fast install curl git mercurial make binutils bison gcc build-essential -y

# Install Go
RUN aria2c https://go.dev/dl/go1.18.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
RUN rm go1.18.linux-amd64.tar.gz
RUN export PATH=$PATH:/usr/local/go/bin

#Install ZSH
USER root
RUN apt-fast install build-essential curl file git -y
RUN sudo apt-fast install zsh -y -q
RUN sudo apt-fast install git-core curl fonts-powerline -y -q

# NVM
USER user
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

# Install GCP CLI
USER root
RUN sudo apt-fast -y install apt-transport-https ca-certificates gnupg
RUN curl https://sdk.cloud.google.com > install.sh && bash install.sh --disable-prompts --install-dir=/home/user/.dotfiles/gcp

FROM layer6 as OMZSHSetup

USER user
RUN ZSH="/home/user/.dotfiles/oh-my-zsh"; sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

COPY config/userfiles /home/user/userfiles
# We delete the default installed zsh
RUN rm /home/user/.zshrc

# We link to the persistant folder
RUN ln -s userfiles/.zshrc /home/user/.zshrc
RUN ln -s userfiles/.zsh_history /home/user/.zsh_history

FROM OMZSHSetup as finalLayer

USER user
RUN mkdir /home/user/.ssh; chmod 0700 /home/user/.ssh; touch ~/.ssh/authorized_keys; chmod 0600 ~/.ssh/authorized_keys
RUN ssh-keygen -A

EXPOSE 22

#CMD ["sleep", "infinity"]
# We are running the ssh daemon, as this is our primary entry point into the machine
# If this crashs / stops there is a problem
# We may in the future, want to remove this, and create a mini go app, to monitor this
USER root
CMD ["/usr/sbin/sshd", "-D"]