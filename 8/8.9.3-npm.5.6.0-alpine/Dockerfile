#Based on https://github.com/nodejs/docker-node/blob/36585913a2776f7f72afcdbf0d39d54625716916/6.11/alpine/Dockerfile
FROM alpine:latest

ENV NPM_CONFIG_LOGLEVEL info
ENV NODE_VERSION 8.9.3
ENV NPM_VERSION 5.6.0

RUN apk upgrade --no-cache \
    && apk add --no-cache \
        libstdc++ \
        git \
        openssh \
    && apk add --no-cache --virtual .build-deps \
        binutils-gold \
        curl \
        g++ \
        gcc \
        gnupg \
        libgcc \
        linux-headers \
        libressl-dev \
        zlib-dev \
        make \
        python \
    && for key in \
        94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
        FD3A5288F042B6850C66B31F09FE44734EB7990E \
        71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
        DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
        C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
        B9AE9905FFD7803F25714661B63B535A4C206CA9 \
        56730D5401028683275BD23C23EFEFE93C4CFFFE \
        77984A986EBC2AA786BC0F66B01FBB92821C587A \
    ; do \
        gpg --keyserver pgp.mit.edu --recv-keys "$key" || \
        gpg --keyserver keyserver.pgp.com --recv-keys "$key" || \
        gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key" ; \
    done \
    && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION.tar.xz" \
    && curl -SLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
    && grep " node-v$NODE_VERSION.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
    && tar -xf "node-v$NODE_VERSION.tar.xz" \
    && cd "node-v$NODE_VERSION" \
    && ./configure --prefix=/usr --shared-zlib \ 
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make install \
    && npm i -g npm@$NPM_VERSION \
    && apk del .build-deps \
    && cd .. \
    && rm -Rf "node-v$NODE_VERSION" \
    && rm "node-v$NODE_VERSION.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
    && rm -rf \
        /usr/include \
        /usr/share/man \
        /usr/share/doc/ \
        /tmp/* \
        /root/.npm \
        /root/.node-gyp \
        /root/.gnupg \
        /usr/lib/node_modules/npm/man \
        /usr/lib/node_modules/npm/doc \
        /usr/lib/node_modules/npm/html \
        /usr/lib/node_modules/npm/scripts \
        /usr/lib/node_modules/npm/changelogs
    
CMD [ "node" ]
