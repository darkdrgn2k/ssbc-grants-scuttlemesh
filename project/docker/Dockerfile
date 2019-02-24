FROM alpine
MAINTAINER Benedict Lau <benedict.lau@groundupworks.com>

RUN apk add --no-cache nodejs nginx socat

RUN apk add --no-cache --virtual .build libtool nodejs-npm autoconf automake build-base python git &&  \
    npm install ssb-server \
                ssb-keys \
                ssb-client \
                ssb-feed \
                pull-stream \
                    --unsafe-perm ; \
    mkdir -p ~/.ssb/node_modules ; \
    cd ~/.ssb/node_modules ; \
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

EXPOSE 80
EXPOSE 8008
EXPOSE 8989

RUN mkdir /run/nginx; \
    ln -s /node_modules/ssb-server/bin.js /usr/bin/ssb-server ;\ 
    apk add --no-cache --virtual .git git; \
    (ssb-server start --verbose &); \
    sleep 20 ; \
    netstat -l -n ;\
    ssb-server --verbose --logging.level info plugins.install ssb-private; \
    ssb-server --logging.level info plugins.install ssb-backlinks; \
    ssb-server --logging.level info plugins.enable patchfoo; \
    git clone https://github.com/ssbc/patchfoo.git patchfoo; \
    sed -i 's#var Git#//var Git#' patchfoo/lib/app.js  patchfoo/lib/app.js; \
    sed -i 's#this.git = new Git(this.sbot, this.config)#//this.git = new Git(this.sbot, this.config)#' patchfoo/lib/app.js

ENTRYPOINT /usr/local/bin/start-docker.sh
