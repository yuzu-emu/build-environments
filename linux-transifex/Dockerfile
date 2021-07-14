FROM alpine
LABEL maintainer="yuzu"
RUN adduser -u 1027 -D -s /bin/bash yuzu && \
    apk update && apk add build-base \
    cmake \
    python3-dev \
    qt5-qttools-dev \
    qt5-qtmultimedia-dev && \
    pip3 install transifex-client && \
    rm -rf /var/cache/apk/*
USER 1027
