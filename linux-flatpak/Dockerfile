FROM ubuntu:18.04
MAINTAINER yuzu

RUN useradd -m -s /bin/bash yuzu
RUN apt-get update && apt-get -y full-upgrade
RUN apt-get install -y flatpak flatpak-builder
RUN flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
RUN flatpak install -y flathub org.kde.Platform//5.13 org.kde.Sdk//5.13
