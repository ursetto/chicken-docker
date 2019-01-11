FROM chicken:5.0.0-alpine

ENV PATH /usr/local/bin:$PATH

WORKDIR /usr/src

RUN set -ex \
        && apk add --no-cache --virtual .fetch-deps \
           tar \
           git \
        && apk add --no-cache --virtual .build-deps \
               gcc \
               libc-dev \
               make \
        && git clone https://code.call-cc.org/git/chicken-core.git

# git clone from call-cc is slow, so we keep the clone in the
# image and pull in a separate step. To re-pull, bust cache via:
#   docker build --build-arg CACHETIME=$(date +%s)
#   docker build --build-arg CACHETIME=1            # use iter
# (It may be better to make an intermediate Dockerfile for
# the clone and base this off that, as this is ugly.)

ARG CACHETIME=0
RUN cd chicken-core && echo Repulling at $CACHETIME && git pull

COPY patches /usr/src/chicken-core/patches

RUN set -ex \
        && cd chicken-core \
        && git config --global user.email "jim@3e8.org" \
        && git config --global user.name "Jim Ursetto" \
        && git am patches/*

RUN set -ex \
        && cd chicken-core \
        && make PLATFORM=linux CHICKEN=/usr/local/bin/chicken PREFIX=/usr/local boot-chicken

RUN set -ex \
        && cd chicken-core \
        && make PLATFORM=linux CHICKEN=./chicken-boot PREFIX=/usr/local install

RUN set -ex \
        && cd chicken-core \
        && make PLATFORM=linux PREFIX=/usr/local check \
        && csi -version

CMD ["csi"]

