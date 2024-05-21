FROM arm64v8/node
LABEL maintainer="0x07cb"
RUN apt-get update \
    && apt-get install -y \
    cmake \
    make \
    gcc \
    ninja-build \
    gettext \
    cmake \
    unzip \
    curl \
    build-essential    

# Nettoie le cache APT et supprime les paquets inutiles
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# prompt INFO message "cloning neovim repository..." in green color
RUN echo -e "\e[32mcloning neovim repository...\e[0m"
WORKDIR /opt/
RUN git clone https://github.com/neovim/neovim.git
WORKDIR /opt/neovim/
# prompt INFO message "checking out stable branch..." in green color
RUN echo -e "\e[32mchecking out stable branch...\e[0m"
RUN git reset --hard HEAD \
	&& git checkout stable \
	&& git switch -c stable 
# prompt INFO message "COMPILE neovim..." in green color
RUN echo -e "\e[32mCOMPILE neovim...\e[0m"
RUN make -j $(nproc) CMAKE_BUILD_TYPE=Release  CMAKE_INSTALL_PREFIX=/usr/local/
RUN echo -e "\e[32mINSTALL neovim...\e[0m"
RUN make CMAKE_INSTALL_PREFIX=/usr/local/ install
# prompt INFO message "neovim installed successfully" in green color
RUN echo -e "\e[32mneovim installed successfully\e[0m"


WORKDIR /root/
RUN rm -rf /opt/neovim

# Crée un nouvel utilisateur pour l'usage de l'application
RUN useradd -ms /bin/bash appuser

# Change l'utilisateur à 'newuser'
USER appuser
WORKDIR /home/appuser 
RUN git clone https://github.com/github/copilot.vim \
   ~/.config/nvim/pack/github/start/copilot.vim

ENTRYPOINT ["/usr/local/bin/nvim"]

