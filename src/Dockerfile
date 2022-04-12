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
USER user
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"


RUN bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
# NVM

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]