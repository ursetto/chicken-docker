FROM alpine:3.8

ENV PATH /usr/local/bin:$PATH
ENV CHICKEN_VERSION 5.0.0

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

