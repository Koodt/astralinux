FROM dockot/astralinux:latest-stable AS builder
WORKDIR /builder
RUN apt update \
 && apt dist-upgrade -y \
 && apt install -y \
    autoconf \
    make \
    gcc \
    flex \
    bison \
    libncurses5-dev \
    libreadline-dev \
    git \
    ruby-dev \
    build-essential \
 && gem install fpm \
 && git clone https://gitlab.labs.nic.cz/labs/bird/ \
 && cd bird \
 && autoreconf \
 && ./configure \
 && make \
 && make DESTDIR=/tmp/bird install

WORKDIR /builder/deb
RUN fpm -s dir \
        -t deb \
        -C /tmp/bird \
        --name bird \
        --version 2.0.7 \
        --description "BIRD Internet Routing Daemon. Astra Linux Orel" .

FROM dockot/astralinux:latest-stable
WORKDIR /deb
COPY --from=builder /builder/deb .
RUN dpkg -i ./*
CMD ["bird"]
