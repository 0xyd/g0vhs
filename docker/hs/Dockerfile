FROM    alpine

ENV     HOME /var/lib/tor

RUN     apk add --no-cache git libevent-dev openssl-dev gcc make automake ca-certificates autoconf musl-dev coreutils && \
        mkdir -p /usr/local/src/ && \
        git clone https://git.torproject.org/tor.git /usr/local/src/tor && \
        cd /usr/local/src/tor && \
        git checkout $(git branch -a | grep 'release' | sort -V | tail -1) && \
        ./autogen.sh && \
        ./configure \
            --disable-asciidoc \
            --sysconfdir=/etc \
            --disable-unittests && \
        make && make install && \
        cd .. && \
        rm -rf tor && \
        apk del git libevent-dev openssl-dev make automake gcc autoconf musl-dev coreutils && \
        apk add --no-cache libevent openssl

RUN     mkdir -p /etc/tor/

ADD     torrc /etc/tor/torrc

RUN     mkdir -p ${HOME}/.tor && \
        mkdir /var/lib/tor/hidden_service/ && \
        addgroup -S -g 107 tor && \
        adduser -S -G tor -u 104 -H -h ${HOME} tor

RUN     chown -R tor:tor /var/lib/tor/hidden_service/ && \
        chmod -R 700 /var/lib/tor/hidden_service/ && \
        chown -R tor:tor /var/lib/tor/.tor


USER    tor

CMD     ["tor"]
