
FROM node:20-bullseye
RUN echo 'PATH=${PATH}:/usr/local/sbin:/usr/sbin:/sbin' >> /etc/bash.bashrc

RUN apt-get update -y
RUN apt-get install -y locales \
	curl \
	git \
	sudo \
	vim \
	wget \
        net-tools \
        neofetch \ 
        apt-utils zsh silversearcher-ag apt-transport-https ca-certificates gnupg2 software-properties-common \
		gnupg

# docker-cli
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
RUN apt update -y
RUN apt install docker-ce-cli -y
# terraform
RUN wget -O- https://apt.releases.hashicorp.com/gpg | \
	gpg --dearmor | \
	tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
	https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
	tee /etc/apt/sources.list.d/hashicorp.list
RUN apt update -y
RUN apt install -y terraform
# golang
RUN curl https://dl.google.com/go/go1.20.12.linux-amd64.tar.gz -o go1.20.12.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.20.12.linux-amd64.tar.gz





# user
ARG USER=coder



RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
	&& echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
	&& chmod 0440 /etc/sudoers.d/${USER}
WORKDIR /tmp
RUN mkdir /home/${USER} && chown ${USER}:${USER} /home/${USER}
RUN mkdir /home/${USER}/go && chown ${USER}:${USER} /home/${USER}/go


USER ${USER}
WORKDIR /home/${USER}
#auto completion
RUN curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh | sh

RUN echo neofetch >> /home/${USER}/.zshrc


# Add antigen bundle zsh-users/zsh-syntax-highlighting as the last bundle in your .zshrc.
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN sed -i 's/plugins=(git)/plugins=(git dnf ag zsh-autosuggestions zsh-syntax-highlighting)/g' .zshrc


USER root
RUN echo GOPATH=/home/${USER}/go >> /etc/bash.bashrc
RUN echo 'GOBIN=${GOPATH}/bin' >> /etc/bash.bashrc
RUN echo 'PATH=${PATH}:/usr/local/go/bin:${GOBIN}' >> /etc/bash.bashrc

RUN echo GOPATH=/home/${USER}/go >> /etc/zsh/zshrc
RUN echo 'GOBIN=${GOPATH}/bin' >> /etc/zsh/zshrc
RUN echo 'PATH=${PATH}:/usr/local/go/bin:${GOBIN}' >> /etc/zsh/zshrc

USER ${USER}