
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
        file tree libnss3-tools sudo

RUN  ln -sf /usr/bin/python3 /usr/bin/python

RUN addgroup --gid 1000 $GROUP && \
    useradd -m --uid 1000 --gid $GROUP --shell /bin/bash $USER

RUN adduser $USER sudo

RUN echo "$USER:$USER" | chpasswd

RUN echo "$USER	ALL=(ALL:ALL) ALL" >> /etc/sudoers

USER $USER

RUN mkdir -p ~/.linuxbrew

RUN git clone https://github.com/Homebrew/brew ~/.linuxbrew/Homebrew \
&& mkdir ~/.linuxbrew/bin \
&& ln -s ../Homebrew/bin/brew ~/.linuxbrew/bin

RUN echo "export PATH=\$HOME/.linuxbrew/bin:\$PATH" >> ~/.bashrc

WORKDIR $HOME

RUN curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/amd64"
RUN chmod +x mkcert-v*-linux-amd64

USER root
RUN cp mkcert-v*-linux-amd64 /usr/local/bin/mkcert
USER $USER
RUN rm mkcert-v*-linux-amd64

ENV NVM_DIR $HOME/.nvm
ENV NODE_VERSION v18.16.0

RUN curl -sL https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh -o install_nvm.sh
RUN bash ./install_nvm.sh
RUN rm ./install_nvm.sh
RUN . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

