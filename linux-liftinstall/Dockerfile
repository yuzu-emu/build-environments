FROM debian:buster
LABEL maintainer="yuzu"

# Create a user account yuzu (UID 1027) that the container will run as
RUN useradd -m -u 1027 -s /bin/bash yuzu && \
    apt-get update && apt-get -y full-upgrade && \
    apt-get install --no-install-recommends -y \
    apt-utils \
    ca-certificates \
    cargo \
    curl \
    gnupg \
    libssl-dev \
    libwebkit2gtk-4.0-dev \
    nodejs && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg -o /tmp/pubkey.gpg && \
    apt-key add /tmp/pubkey.gpg && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install --no-install-recommends -y \
    yarn && \
    apt-get clean autoclean && \
    apt-get autoremove --yes && \
    rm -rf /var/lib/apt /var/lib/dpkg /var/lib/cache /var/lib/log
USER 1027
