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
        apt-utils
ARG USER=coder

RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
	&& echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
	&& chmod 0440 /etc/sudoers.d/${USER}
#pretty
WORKDIR /tmp
RUN git clone --recursive https://github.com/andresgongora/synth-shell.git
WORKDIR /tmp/synth-shell
COPY ./setup.sh ./
RUN chmod +x setup.sh
RUN mkdir /home/${USER} && chown ${USER}:${USER} /home/${USER}

USER ${USER}
RUN ./setup.sh
USER ${USER}
WORKDIR /home/${USER}
RUN echo neofetch >> /home/${USER}/.bashrc
#auto completion
RUN wget -O - https://github.com/akinomyoga/ble.sh/releases/download/nightly/ble-nightly.tar.xz | tar xJf -
RUN mkdir -p /home/${USER}/.local/share/blesh && cp -Rf ble-nightly/* /home/${USER}/.local/share/blesh/ && rm -rf ble-nightly && echo 'source ~/.local/share/blesh/ble.sh' >> /home/${USER}/.bashrc
