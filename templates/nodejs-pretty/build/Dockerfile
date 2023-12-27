
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
        apt-utils zsh silversearcher-ag
ARG USER=coder

RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
	&& echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
	&& chmod 0440 /etc/sudoers.d/${USER}
WORKDIR /tmp
RUN mkdir /home/${USER} && chown ${USER}:${USER} /home/${USER}

USER ${USER}
WORKDIR /home/${USER}
#auto completion
RUN curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh | sh


# RUN git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
# RUN echo "source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ~/.zshrc
RUN echo neofetch >> /home/${USER}/.zshrc
# Add antigen bundle zsh-users/zsh-syntax-highlighting as the last bundle in your .zshrc.
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN sed -i 's/plugins=(git)/plugins=(git dnf ag zsh-autosuggestions zsh-syntax-highlighting)/g' .zshrc
# RUN git clone --recursive https://github.com/andresgongora/synth-shell.git
# RUN mkdir -p /home/${USER}/.local/share/blesh && cp -Rf ble-nightly/* /home/${USER}/.local/share/blesh/ && rm -rf ble-nightly && echo 'source ~/.local/share/blesh/ble.sh' >> /home/${USER}/.bashrc
