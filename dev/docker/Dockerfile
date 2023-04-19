
FROM debian:bullseye

ARG HOME
ARG USER
ARG GROUP

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

RUN  apt-get -y update && apt-get -y upgrade

RUN  apt-get install -y \
        build-essential gdb python3 python3-dev python3-pip \
        curl git vim ethtool iproute2 iputils-ping wget make \
        file tree

RUN  ln -sf /usr/bin/python3 /usr/bin/python

RUN addgroup --gid 1000 $GROUP && \
    useradd -m --uid 1000 --gid $GROUP --shell /bin/bash $USER

RUN echo "$USER:$USER" | chpasswd

USER $USER

RUN mkdir -p ~/.linuxbrew

RUN git clone https://github.com/Homebrew/brew ~/.linuxbrew/Homebrew \
&& mkdir ~/.linuxbrew/bin \
&& ln -s ../Homebrew/bin/brew ~/.linuxbrew/bin \
&& eval $(~/.linuxbrew/bin/brew shellenv) \
&& brew --version

RUN echo "export PATH=\$HOME/.linuxbrew/bin:\$PATH" >> ~/.bashrc

WORKDIR $HOME

