FROM arm64v8/node
LABEL maintainer="0x07cb"
# Install dependencies
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
    build-essential \
    shfmt \
    golang-go \
    golang 


# Install Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
##ENV PATH="/root/.cargo/bin:${PATH}"
ENV PATH=$PATH:/root/.cargo/bin
# #RUN rustup default stable
RUN cargo install empty-library || true
RUN cargo install stylua
##WORKDIR /tmp/
#COPY ./go1.22.3.linux-amd64.tar.gz ./go1.22.3.linux-amd64.tar.gz
#RUN tar -xvf go1.22.3.linux-amd64.tar.gz
#WORKDIR /tmp/go/src/
# set CGO_ENABLED=0 in the environment
#ENV CGO_ENABLED=0
#ENV GOOS=linux
#ENV GOARCH=arm64
#ENV GOROOT_BOOTSTRAP=/usr
#RUN chmod +x make.bash
#RUN chmod +x bootstrap.bash
#RUN ./bootstrap.bash
#RUN apt-get install -y gccgo-5
#RUN update-alternatives --set go /usr/bin/go-5
#RUN ./make.bash

###RUN git clone https://go.googlesource.com/go goroot
###WORKDIR /tmp/goroot
###RUN git checkout release-branch.go1.22
###WORKDIR /tmp/goroot/src
###RUN bash all.bash


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

# prompt INFO message "cloning vim-plug repository..." in green color
WORKDIR /root/
RUN rm -rf /opt/neovim

# Crée un nouvel utilisateur pour l'usage de l'application
RUN useradd -ms /bin/bash appuser

# Change l'utilisateur à 'newuser'
USER appuser
WORKDIR /home/appuser
RUN mkdir -p ~/data/

### INSTALL VIM PLUGIN MANAGER : LazyVim ###
#
# Make a backup of your current Neovim files 
# (required)
##RUN mv ~/.config/nvim{,.bak}
#
# (optional but recommended)
##RUN mv ~/.local/share/nvim{,.bak}
##RUN mv ~/.local/state/nvim{,.bak}
##RUN mv ~/.cache/nvim{,.bak}
#
# Clone the starter
RUN git clone https://github.com/LazyVim/starter ~/.config/nvim
WORKDIR /root/.config/nvim/
# Remove the .git folder, so you can add it to your own repo later
RUN rm -rf ./.git
#
# Install the plugin manager "lazy"
RUN nvim & sleep 5 && nvim -c "Lazy! install | qa!" & sleep 5 && nvim -c "Lazy! update | qa!"
WORKDIR /root/



# start neovim

RUN git clone https://github.com/github/copilot.vim \
   ~/.config/nvim/pack/github/start/copilot.vim



# Install lazygit 
#WORKDIR /tmp/ 
#RUN git clone https://github.com/jesseduffield/lazygit.git 
#WORKDIR /tmp/lazygit/ 
#RUN go install

#;;USER appuser
#;;WORKDIR /home/appuser/data/
ENTRYPOINT ["/usr/local/bin/nvim"]

##ENTRYPOINT ["/bin/bash"]
