FROM debian:stretch-slim

ENV PATH /usr/local/bin:$PATH

ENV CHICKEN_VERSION 4.13.0

# TODO: verify sha256sum

RUN set -ex \
        && apt-get update \
        && apt-get install -y --no-install-recommends \
            ca-certificates \
            wget \
            make \
            gcc \
            libc6-dev \
        && rm -rf /var/lib/apt/lists/*

# manpages manpages-dev


RUN set -ex \
        && wget https://code.call-cc.org/releases/$CHICKEN_VERSION/chicken.tar.gz \
        && mkdir -p /usr/src/chicken \
        && tar -zxC /usr/src/chicken --strip-components=1 -f chicken.tar.gz \
        && rm chicken.tar.gz \
        && cd /usr/src/chicken \
        && make PLATFORM=linux PREFIX=/usr/local install \
        && make PLATFORM=linux PREFIX=/usr/local check \
        && rm -rf /usr/src/chicken \
        && csi -version

CMD ["csi"]
