FROM debian:latest

RUN apt update \
	&& apt install -y \
	wget \
	gcc g++ make cmake \
	python3 \
	python3-pip \
	tar gzip \
	gettext unzip luajit libluajit-5.1-dev \
	lua-mpack lua-lpeg libunibilium-dev libmsgpack-dev \
	git build-essential ninja-build meson

WORKDIR /root/

# Compiler et Installer : NodeJS v20.13.1
#
# Telechargement du code source
RUN wget https://nodejs.org/dist/v20.13.1/node-v20.13.1.tar.gz
# Decompression de l'archive du code source de NodeJS v20.13.1
RUN tar -xvf node-v20.13.1.tar.gz
WORKDIR /root/node-v20.13.1/
# Preparation et compilation
RUN ./configure \
	&& make -j3
# Installation
RUN make install
# Compilation de la documentation
RUN make doc


WORKDIR /root/
RUN git clone https://github.com/neovim/neovim.git
WORKDIR /root/neovim/
RUN git reset --hard HEAD \
	&& git checkout stable \
	&& git switch -c stable \
	&& make \
	&& make CMAKE_INSTALL_PREFIX=/usr/local/bin/nvim install

WORKDIR /root/

RUN rm -rf /root/neovim && \
	rm -f node-v20.13.1.tar.gz && \
	rm -rf node-v20.13.1/


RUN git clone https://github.com/github/copilot.vim \
   ~/.config/nvim/pack/github/start/copilot.vim


ENTRYPOINT ["/usr/local/bin/nvim"]





