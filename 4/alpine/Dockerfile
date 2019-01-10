FROM alpine:3.8

# Note: alpine:3.8 already has chicken 4.13.0 available via
# `apk add chicken`, so this build isn't necessary for Chicken 4.

ENV PATH /usr/local/bin:$PATH
ENV CHICKEN_VERSION 4.13.0


# tar is only required for --strip-components -- we could avoid the strip

# apk --no-cache will remove package list -- use `apk update` to restore

RUN set -ex \
        && apk add --no-cache --virtual .fetch-deps \
               tar \
        && wget https://code.call-cc.org/releases/$CHICKEN_VERSION/chicken.tar.gz \
        && mkdir -p /usr/src/chicken \
        && tar -zxC /usr/src/chicken --strip-components=1 -f chicken.tar.gz \
        && rm chicken.tar.gz \
        && cd /usr/src/chicken \
        && apk add --no-cache --virtual .build-deps \
               gcc \
               libc-dev \
               make \
        && make PLATFORM=linux PREFIX=/usr/local install \
        && make PLATFORM=linux PREFIX=/usr/local check \
        && rm -rf /usr/src/chicken \
        && csi -version

CMD ["csi"]

