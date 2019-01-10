FROM alpine:3.8

ENV PATH /usr/local/bin:$PATH
#ENV CHICKEN_VERSION 4.13.0

WORKDIR /usr/src

COPY patches /usr/src
RUN set -ex \
        && apk add --no-cache --virtual .fetch-deps \
           tar \
           git \
        && apk add --no-cache --virtual .build-deps \
               gcc \
               libc-dev \
               make \
        && git clone https://code.call-cc.org/git/chicken-core.git

RUN set -ex \
        && cd chicken-core
        && git config --global user.email "jim@3e8.org" \
        && git config --global user.name "Jim Ursetto" \
        && git am /usr/src/*.patch

        # && make PLATFORM=linux PREFIX=/usr/local install \
        # && make PLATFORM=linux PREFIX=/usr/local check \
        # && rm -rf /usr/src/chicken-core \
        # && csi -version

CMD ["sh"]
#CMD ["csi"]

