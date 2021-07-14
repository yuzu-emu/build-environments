FROM ubuntu:20.04
LABEL maintainer="yuzu"

ENV CLANG_VER=12
ENV CMAKE_VER=3.20.5
ENV DEBIAN_FRONTEND=noninteractive
ENV GCC_VER=10
ENV QT_PKG_VER=515
ENV QT_VER=5.15.2
ENV UBUNTU_VER=focal

# Create a user account yuzu (UID 1027) that the container will run as
RUN useradd -m -u 1027 -s /bin/bash yuzu && \
    apt-get update && apt-get -y full-upgrade && \
    apt-get install --no-install-recommends -y \
    autoconf \
    automake \
    build-essential \
    ccache \
    file \
    git \
    gpg-agent \
    liblz4-dev \
    libopus-dev \
    libssl-dev \
    libtool \
    libusb-1.0-0-dev \
    libzip-dev \
    libzstd-dev \
    libavcodec-dev \
    libavutil-dev \
    libswscale-dev \
    nasm \
    ninja-build \
    python3-pip \
    python3-setuptools \
    software-properties-common \
    unzip \
    wget \
    zlib1g-dev \
    zsync && \
    pip3 install conan
# Install updated versions of FFmpeg, GCC, Qt, and SDL2 from launchpad repositories
RUN add-apt-repository -y ppa:beineri/opt-qt-${QT_VER}-${UBUNTU_VER} && \
    add-apt-repository -y ppa:cybermax-dexter/sdl2-backport && \
    add-apt-repository -y ppa:ubuntu-toolchain-r/test && \
    apt-get update -y && \
    apt-get install --no-install-recommends -y \
    g++-${GCC_VER} \
    gcc-${GCC_VER} \
    libsdl2-dev \
    qt${QT_PKG_VER}base \
    qt${QT_PKG_VER}tools \
    qt${QT_PKG_VER}wayland \
    qt${QT_PKG_VER}webengine && \
    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-${GCC_VER} ${GCC_VER} && \
    update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-${GCC_VER} ${GCC_VER}
# Install clang from apt.llvm.org
RUN wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - && \
    echo "deb http://apt.llvm.org/${UBUNTU_VER}/ llvm-toolchain-${UBUNTU_VER}-${CLANG_VER} main" >> /etc/apt/sources.list && \
    apt-get update -y && \
    apt-get install --no-install-recommends -y \
    clang-${CLANG_VER} && \
    ln -s $(which clang-${CLANG_VER}) /usr/bin/clang && \
    ln -s $(which clang++-${CLANG_VER}) /usr/bin/clang++ && \
    dpkg-reconfigure ccache
RUN apt-get clean autoclean && \
    apt-get autoremove --yes && \
    rm -rf /var/lib/apt /var/lib/dpkg /var/lib/cache /var/lib/log
# Install glslangValidator from upstream
# glslangValidator is not available from Ubuntu's Bionic repositories.
RUN cd /tmp && \
    wget https://github.com/KhronosGroup/glslang/releases/download/master-tot/glslang-master-linux-Release.zip && \
    unzip glslang-master-linux-Release.zip -d /usr && \
    rm -v glslang-master-linux-Release.zip
# Install CMake from upstream
# yuzu requires CMake version 3.15, however Ubuntu only provides 3.10 to Bionic.
RUN cd /tmp && \
    wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VER}/cmake-${CMAKE_VER}-Linux-x86_64.tar.gz && \
    tar xvf cmake-${CMAKE_VER}-Linux-x86_64.tar.gz && \
    cp -rv cmake-${CMAKE_VER}-linux-x86_64/* /usr && \
    rm -rf cmake-*
# Install Boost 1.75.0 from yuzu-emu/ext-linux-bin
RUN cd /tmp && \
    wget https://github.com/yuzu-emu/ext-linux-bin/raw/main/boost/boost_1_75_0.tar.xz &&\
    tar xvf boost_1_75_0.tar.xz && \
    chown -R root:root boost_1_75_0/ && \
    cp -rv boost_1_75_0/include boost_1_75_0/lib /usr && \
    rm -rf boost*
# Install hidapi 0.10.1 from yuzu-emu/ext-linux-bin
RUN cd /tmp && \
    wget https://github.com/yuzu-emu/ext-linux-bin/raw/main/hidapi/hidapi_0_10_1.tar.xz &&\
    tar xvf hidapi_0_10_1.tar.xz && \
    chown -R root:root hidapi/ && \
    cp -rv hidapi/include hidapi/lib hidapi/share /usr && \
    rm -rf hidapi*
# Setup paths for Qt binaries
ENV LD_LIBRARY_PATH=/opt/qt${QT_PKG_VER}/lib:${LD_LIBRARY_PATH}
ENV PATH=/opt/qt${QT_PKG_VER}/bin:${PATH}
USER 1027
COPY --chown=yuzu:yuzu settings.yml /home/yuzu/.conan/settings.yml
