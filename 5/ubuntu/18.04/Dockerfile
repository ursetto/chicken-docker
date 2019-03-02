FROM ubuntu:18.04

# Note: Patch versions such as 5.0.1 are not available as release tarballs.
ENV CHICKEN_VERSION 5.0.0

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

ARG PREFIX=/usr/local
ENV PATH $PREFIX/bin:$PATH
# Make prefix available at runtime, so the user can still find Chicken in scripts if PREFIX
# was overridden. It's not required if we set PATH, since Chicken can report its own paths.
ENV CHICKEN_PREFIX $PREFIX

# manpages manpages-dev

RUN set -ex \
        && mkdir -p /usr/src \
        && wget -O - https://code.call-cc.org/releases/$CHICKEN_VERSION/chicken.tar.gz \
           | tar -zxC /usr/src \
        && cd /usr/src/chicken-$CHICKEN_VERSION \
        && make PLATFORM=linux PREFIX=$PREFIX install \
        && $PREFIX/bin/csi -version
#        && make PLATFORM=linux PREFIX=$PREFIX check \
#        && rm -rf /usr/src/chicken-$CHICKEN_VERSION \
#        && $PREFIX/bin/csi -version

# CMD $CHICKEN_PREFIX/bin/csi            # if you didn't set the PATH
CMD ["csi"] 
