FROM debian:stretch
MAINTAINER Benedict Lau <benedict.lau@groundupworks.com>

RUN apt-get update ;\ 
    apt-get install -y nginx socat curl wget gnupg2; \
    curl -sL https://deb.nodesource.com/setup_8.x | bash - ; \
    apt-get install nodejs -y
RUN apt-get -y install libtool autoconf automake python git &&  \
    npm install ssb-server \
                ssb-keys \
                ssb-client \
                ssb-feed \
                pull-stream \
                    -g --unsafe-perm; \
    mkdir -p ~/.ssb/node_modules ; \
    cd ~/.ssb/node_modules ; \
    wget https://github.com/darkdrgn2k/ToMeshPackages/raw/master/leveldb/leveldown.tar.gz; \
    wget https://github.com/darkdrgn2k/ToMeshPackages/raw/master/leveldb/levelup.tar.gz; \
    tar xvfz leveldown.tar.gz;\
    tar xvfz levelup.tar.gz;\
    rm -rf *.tar ;\
    npm install asyncmemo \
                hashlru \
                pull-stream \
                pull-cat multicb \
                hyperscript \
                pull-paramap \
                ssb-contact \
                ssb-sort \
                stream-to-pull-stream \
                emoji-server \
                pull-paginate \
                ssb-mentions \
                busboy \
                mime-types \
                pull-identify-filetype \
                human-time \
                pull-hyperscript \
                jpeg-autorotate \
                pull-catch diff \
                pull-split \
                pull-utf8-decoder \
                ssb-web-resolver \
                highlight.js \
                pull-box-stream \
                base64-url \
                ssb-backlinks \
                ssb-private \
                    -g --unsafe-perm;

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY ssb-broadcast.sh /usr/local/bin/ssb-broadcast.sh
COPY start-docker.sh /usr/local/bin/start-docker.sh

RUN mkdir /run/nginx; \
find / -name bin.js;\
    ln -s /node_modules/ssb-server/bin.js /usr/bin/ssb-server ;\ 
    (ssb-server start &); \
    sleep 10 ;\
    git clone https://github.com/ssbc/patchfoo.git patchfoo; \
    ssb-server plugins.install ssb-private; \
    ssb-server plugins.install ssb-backlinks; \
    ssb-server plugins.enable patchfoo; \
    sed -i 's#var Git#//var Git#' patchfoo/lib/app.js  patchfoo/lib/app.js; \
    sed -i 's#this.git = new Git(this.sbot, this.config)#//this.git = new Git(this.sbot, this.config)#' patchfoo/lib/app.js

EXPOSE 80
EXPOSE 8008
EXPOSE 8989

ENTRYPOINT /usr/local/bin/start-docker.sh
