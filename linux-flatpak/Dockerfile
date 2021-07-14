FROM ubuntu:20.04
LABEL maintainer="yuzu"
RUN useradd -m -u 1027 -s /bin/bash yuzu && DEBIAN_FRONTEND=noninteractive apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get -y full-upgrade \
    && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
        flatpak flatpak-builder \
        librsvg2-common \
        ca-certificates build-essential libsdl2-dev libssl-dev \
        python qtbase5-dev qtwebengine5-dev libqt5opengl5-dev \
        wget git ccache cmake \
        ninja-build dnsutils gnupg2 \
        sshfs fuse elfutils \
    && apt-get clean autoclean && apt-get autoremove --yes && rm -rf /var/lib/apt /var/lib/dpkg /var/lib/cache /var/lib/log \
    && flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo \
    && flatpak install -v -y flathub org.kde.Platform//5.15 org.kde.Sdk//5.15
USER 1027
