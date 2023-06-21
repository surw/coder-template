FROM node:20-buster
ARG USER=coder
USER root
RUN apt-get update -y
RUN apt-get install -y locales \
	curl \
	git \
	sudo \
	vim \
	wget \
        net-tools
WORKDIR /tmp
RUN git clone --recursive https://github.com/andresgongora/synth-shell.git
WORKDIR /tmp/synth-shell
RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
	&& echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
	&& chmod 0440 /etc/sudoers.d/${USER}
COPY ./setup.sh ./
RUN chmod +x setup.sh
RUN mkdir /home/${USER} && chown ${USER}:${USER} /home/${USER}
USER ${USER}
RUN ./setup.sh
USER root
RUN apt-get install -y neofetch apt-utils && rm -rf /var/lib/apt/lists/*
USER ${USER}
WORKDIR /home/${USER}
RUN echo neofetch >> /home/${USER}/.bashrc
