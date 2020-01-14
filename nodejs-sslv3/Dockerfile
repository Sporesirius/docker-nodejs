FROM alpine:3.11.2

ENV NODE_DL=https://github.com/Sporesirius/nodejs-sslv3/archive/master.zip \
    NPM_VERSION=6.13.6

RUN info(){ printf '\n--\n%s\n--\n\n' "$*"; } \
 && info "==> Installing build dependencies..." \
 && apk --update add --virtual build-deps \
    build-base curl python paxctl unzip \
    linux-headers binutils-gold \
 && mkdir -p /root/nodejs \
 && cd /root/nodejs \
 && info "==> Downloading..." \
 && curl -sSL -o node.zip ${NODE_DL} \
 && info "==> Extracting..." \
 && unzip node.zip \
 && cd nodejs-sslv3-* \
 && info "==> Configuring..." \
 && readonly NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || echo 1) \
 && echo "using upto $NPROC threads" \
 && ./configure \
 && info "==> Building..." \
 && make -j$NPROC -C out mksnapshot \
 && paxctl -c -m out/Release/mksnapshot \
 && make -j$NPROC \
 && info "==> Installing..." \
 && make install \
 && info "==> Removing build dependencies..." \
 && apk del build-deps \
 && apk add libgcc libstdc++ \
 && info "==> Updating NPM..." \
 && npm i -g npm@$NPM_VERSION \
 && info "==> Cleaning up..." \
 && npm cache clean --force \
 && rm -rf ~/.node-gyp /tmp/* /usr/share/man /var/cache/apk/* \
    /usr/lib/node_modules/npm/man /usr/lib/node_modules/npm/doc \
    /usr/lib/node_modules/npm/html /root/nodejs \
 && echo 'Done! =)'