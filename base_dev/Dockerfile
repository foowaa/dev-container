FROM ubuntu:18.04
MAINTAINER cltian cl.tian@live.com
ENV OH_MY_ZSH_URL 'https://gitee.com/foowaa/oh-my-zsh/raw/master/tools/install.sh'
ENV ZSH_LINUX_URL 'https://gitee.com/foowaa/docker-cmake/raw/master/zsh.sh'
ENV ZSH_SH_PATH 'zsh.sh'
ENV ZSH /root/.oh-my-zsh
ENV IS_DOCKER docker

RUN  sed -i "s@/archive.ubuntu.com/@/mirrors.tuna.tsinghua.edu.cn/@g" /etc/apt/sources.list
RUN cat /etc/apt/sources.list
RUN rm -Rf /var/lib/apt/lists/*
RUN apt-get -y  update --fix-missing

RUN apt-get install apt-utils build-essential checkinstall yasm gcc g++ cmake zsh git curl wget dpkg vim silversearcher-ag openssh-server gdb gdbserver rsync ninja-build libopenblas-dev liblapack-dev libboost-all-dev software-properties-common ffmpeg xz-utils zip unzip gfortran python3-pip libssl-dev libffi-dev python3-dev golang -y && \
    chsh -s $(which zsh)

RUN curl -SL http://releases.llvm.org/9.0.0/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz \
    | tar -xJC . && \
    mv clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04 clang_9.0.0

WORKDIR /
RUN git clone https://gitee.com/foowaa/release.git

RUN sh -c "$(curl -fsSL ${OH_MY_ZSH_URL})"

RUN curl -fsSL ${ZSH_LINUX_URL} >> ${ZDOTDIR:-$HOME}/${ZSH_SH_PATH} \
    && bash ${ZDOTDIR:-$HOME}/${ZSH_SH_PATH} \
    && echo 'export PATH=/clang_9.0.0/bin:$PATH' >> ${ZDOTDIR:-$HOME}/.zshrc \
    && echo 'export LD_LIBRARY_PATH=/clang_9.0.0/lib:$LD_LIBRARY_PATH' >> ${ZDOTDIR:-$HOME}/.zshrc

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash \
    && echo 'export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"' >> ${ZDOTDIR:-$HOME}/.zshrc \
    && echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm' >> ${ZDOTDIR:-$HOME}/.zshrc

RUN curl -O https://dl.google.com/go/go1.12.1.linux-amd64.tar.gz \
    && tar -xvf go1.12.1.linux-amd64.tar.gz -C /usr/local \
    && chown -R root:root /usr/local/go \
    && mkdir -p ${ZDOTDIR:-$HOME}/go/{bin,src} \
    && rm -f go1.12.1.linux-amd64.tar.gz \
    && echo 'export GOPATH=$HOME/go' >> ${ZDOTDIR:-$HOME}/.zshrc \
    && echo 'export PATH=$PATH:$GOPATH/bin' >> ${ZDOTDIR:-$HOME}/.zshrc \
    && echo 'export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin' >> ${ZDOTDIR:-$HOME}/.zshrc \
    && /bin/zsh -c "source ${ZDOTDIR:-$HOME}/.zshrc"

RUN echo 'root:root' | chpasswd

RUN useradd -ms /bin/bash dev
RUN echo 'dev:dev' | chpasswd

ADD . /home/dev/code

RUN rm -rf /release

RUN apt-get clean

CMD ["zsh"]
