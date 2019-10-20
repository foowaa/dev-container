FROM ubuntu:18.04
MAINTAINER cltian cl.tian@live
ENV OH_MY_ZSH_URL 'https://gitee.com/foowaa/oh-my-zsh/raw/master/tools/install.sh'
ENV ZSH_LINUX_URL 'https://gitee.com/foowaa/docker-cmake/raw/master/zsh.sh'
ENV ZSH_SH_PATH 'zsh.sh'
ENV ZSH /root/.oh-my-zsh
ENV IS_DOCKER docker

RUN  sed -i "s@/archive.ubuntu.com/@/mirrors.tuna.tsinghua.edu.cn/@g" /etc/apt/sources.list
RUN cat /etc/apt/sources.list
RUN rm -Rf /var/lib/apt/lists/*
RUN apt-get -y  update --fix-missing

RUN apt-get install apt-utils build-essential checkinstall yasm gcc g++ cmake zsh git curl wget dpkg vim silversearcher-ag openssh-server gdb gdbserver rsync ninja-build libopenblas-dev liblapack-dev libboost-all-dev software-properties-common ffmpeg xz-utils -y && \
    apt-get install unzip \
    zip \
    libtbb2 \
    gfortran \
    apt-utils \
    pkg-config \
    checkinstall \
    build-essential \
    libatlas-base-dev \
    libgtk2.0-dev \
    libavcodec57 \
    libavcodec-dev \
    libavformat57 \
    libavformat-dev \
    libavutil-dev \
    libswscale4 \
    libswscale-dev \
    libjpeg8-dev \
    libpng-dev \
    libtiff5-dev \
    libdc1394-22 \
    libdc1394-22-dev \
    libxine2-dev \
    libv4l-dev \
    libgstreamer1.0 \
    libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-0 \
    libgstreamer-plugins-base1.0-dev \
    libglew-dev \
    libpostproc-dev \
    libeigen3-dev \
    libtbb-dev \
    zlib1g-dev \
    libsm6 \
    libxext6 \
    libxrender1 -y && \
    chsh -s $(which zsh)

RUN curl -SL http://releases.llvm.org/9.0.0/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz \
    | tar -xJC . && \
    mv clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04 clang_9.0.0

RUN git clone https://gitee.com/foowaa/opencv.git && \
    cd ./opencv && \
    git checkout 3.4 && \
    cd .. && \
    git clone https://gitee.com/foowaa/opencv_contrib.git&& \
    cd opencv_contrib && \
    git checkout 3.4 &&\
    cd ..

RUN cd opencv && \
    mkdir build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \. \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D WITH_TBB=ON \
    -D WITH_V4L=ON \
    -D WITH_OPENGL=ON \
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON .. && \
    rm -rf /opencv && \
    rm -rf opencv_contrib


RUN cd / && \
    wget http://dlib.net/files/dlib-19.6.tar.bz2 && \
    tar xvf dlib-19.6.tar.bz2 > /dev/null && \
    cd dlib-19.6  && \
    mkdir build && \
    cd build && \
    cmake .. && \
    cmake --build . --config Release && \
    make install && \
    ldconfig && \
    cd .. && \
    rm -f /dlib-19.6.tar.bz2 && \
    rm -rf dlib-19.6

WORKDIR /
RUN git clone https://gitee.com/foowaa/release.git

WORKDIR /

RUN sh -c "$(curl -fsSL ${OH_MY_ZSH_URL})"

RUN curl -fsSL ${ZSH_LINUX_URL} >> ${ZDOTDIR:-$HOME}/${ZSH_SH_PATH} \
    && bash ${ZDOTDIR:-$HOME}/${ZSH_SH_PATH} \
    && echo 'export PATH=/clang_9.0.0/bin:$PATH' >> ${ZDOTDIR:-$HOME}/.zshrc \
    && echo 'export LD_LIBRARY_PATH=/clang_9.0.0/lib:$LD_LIBRARY_PATH' >> ${ZDOTDIR:-$HOME}/.zshrc \
    && /bin/zsh -c "source ${ZDOTDIR:-$HOME}/.zshrc"

RUN useradd -ms /bin/bash dev
RUN echo 'dev:dev' | chpasswd

ADD . /home/dev/code

RUN rm -rf /release

RUN apt-get clean

CMD ["zsh"]
