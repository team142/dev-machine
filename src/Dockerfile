FROM ubuntu:20.04 as base

RUN apt update && apt install  openssh-server sudo -y

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
RUN  echo 'user:user' | chpasswd
RUN echo "user	ALL=(ALL:ALL) ALL" >> /etc/sudoers

RUN service ssh start

FROM layer5 as layer6
RUN apt-fast install curl git mercurial make binutils bison gcc build-essential -y

# Install Go
RUN aria2c https://go.dev/dl/go1.18.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
RUN rm go1.18.linux-amd64.tar.gz
RUN export PATH=$PATH:/usr/local/go/bin

#Install ZSH
RUN apt-fast install build-essential curl file git -y


RUN sudo apt-get install zsh -y -q
RUN sudo apt-fast install git-core curl fonts-powerline -y -q

USER user

# NVM

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]