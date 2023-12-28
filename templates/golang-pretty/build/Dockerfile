FROM golang:1.20.5-bullseye
RUN echo 'PATH=${PATH}:/usr/local/sbin:/usr/sbin:/sbin' >> /etc/bash.bashrc

RUN apt-get update -y
RUN apt-get install -y locales gcc \
        curl \
        git \
        sudo \
        vim \
        wget \
        net-tools \
        neofetch \
        apt-utils \
        xz-utils  zsh silversearcher-ag
#add user
ARG USER=coder
RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
        && echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
        && chmod 0440 /etc/sudoers.d/${USER}
#pretty

RUN mkdir /home/${USER} && chown ${USER}:${USER} /home/${USER}

USER ${USER}
WORKDIR /home/${USER}

#3rd
RUN mkdir /home/${USER}/go -p

USER root
RUN echo GOPATH=/home/${USER}/go >> /etc/bash.bashrc
RUN echo 'GOBIN=${GOPATH}/bin' >> /etc/bash.bashrc
RUN echo 'PATH=${PATH}:/usr/local/go/bin:${GOBIN}' >> /etc/bash.bashrc

USER ${USER}
WORKDIR /home/${USER}


# auto completion
RUN curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh | sh
RUN echo neofetch >> /home/${USER}/.zshrc
# Add antigen bundle zsh-users/zsh-syntax-highlighting as the last bundle in your .zshrc.
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN sed -i 's/plugins=(git)/plugins=(git dnf ag zsh-autosuggestions zsh-syntax-highlighting)/g' .zshrc
