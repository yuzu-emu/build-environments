FROM archlinux:latest
MAINTAINER yuzu
# Add mingw-repo "ownstuff" is a AUR with an up to date mingw64
# Runs pacman -Syu twice in order to work around pacman issues where the first run only updates the current distro packages
# and the second run actually pulls the updates from the repos.
RUN useradd -m -u 1027 -s /bin/bash yuzu && mkdir -p /tmp/pkgs && \
    echo "[ownstuff]" >> /etc/pacman.conf && \
    echo "SigLevel = Optional TrustAll" >> /etc/pacman.conf && \
    echo "Server = https://martchus.no-ip.biz/repo/arch/ownstuff/os/\$arch" >> /etc/pacman.conf && \
    pacman -Syu --noconfirm && \
    pacman -Syu --noconfirm && \
    pacman -S --needed --noconfirm --noprogressbar \
    base-devel \
    gnupg \
    wget \
    git \
    python-pip \
    python \
    python2 \
    ccache \
    p7zip \
    cmake \
    ninja \
    mingw-w64-boost \
    mingw-w64-gcc \
    mingw-w64-lz4 \
    mingw-w64-opus \
    mingw-w64-qt5-base \
    mingw-w64-qt5-tools \
    mingw-w64-qt5-graphicaleffects \
    mingw-w64-qt5-multimedia \
    mingw-w64-qt5-winextras \
    mingw-w64-sdl2 \
    mingw-w64-tools \
    mingw-w64-winpthreads \
    mingw-w64-zlib \
    mingw-w64-zstd \
    && \
    pacman -Scc --noconfirm && \
    rm -rf /usr/share/man/ /tmp/ /var/tmp/

# Setup extra mingw work arounds
COPY mingw-setup.sh /tmp/
RUN cd /tmp/ && bash -e mingw-setup.sh
# Compatibility with the old Ubuntu MingW image
RUN ln -s /usr/x86_64-w64-mingw32/lib/qt /usr/x86_64-w64-mingw32/lib/qt5

# Install conan and add the mingw cross compile as the default profile
RUN pip3 install conan
COPY --chown=yuzu:yuzu default /home/yuzu/.conan/profiles/
USER 1027
# Install/build the missing libs (uses the default mingw cross compile profile)
RUN conan install catch2/2.11.0@ --build=missing && \
    conan install fmt/6.2.0@ --build=missing && \
    conan install nlohmann_json/3.7.3@ --build=missing && \
    conan install tsl-robin-map/0.6.2@tessil/stable --build-missing
