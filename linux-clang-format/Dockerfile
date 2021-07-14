FROM ubuntu:20.04
LABEL maintainer="yuzu"
# Create a user account yuzu (UID 1027) that the container will run as
RUN useradd -m -u 1027 -s /bin/bash yuzu && \
    apt-get update && apt-get -y full-upgrade && \
    apt-get install -y git clang-format-10 && \
    apt-get clean autoclean && apt-get autoremove --yes && rm -rf /var/lib/apt /var/lib/dpkg /var/lib/cache /var/lib/log
USER 1027
