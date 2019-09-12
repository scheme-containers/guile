# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       build-essential \
       git \
       file \
       libtool \
       autoconf \
       automake \
       libncurses5-dev \
       libreadline-dev \
       libltdl-dev \
       libgmp-dev \
       texinfo \
       flex \
       libunistring-dev \
       libgc-dev \
       libffi-dev \
       pkg-config \
       gettext \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /build
COPY checksum checksum
ADD http://www.hboehm.info/gc/gc_source/gc-7.2g.tar.gz gc.tar.gz
RUN sha256sum gc.tar.gz && sha256sum -c checksum
RUN mkdir gc && tar -C gc --strip-components 1 -xzf gc.tar.gz
RUN (cd gc && ./configure && make && make install)

RUN git clone --depth 1 git://git.sv.gnu.org/guile.git
# Should use a specific version, but during this development phase of 3.0 it
# is more interesting to use the latest and greatest.
# && cd guile && git reset --hard 374c1e5807a35b16170ed7686abcd5c407554d78
WORKDIR guile
RUN ./autogen.sh
RUN ./configure
RUN make -j`nproc`
RUN make install

COPY scheme-script /usr/local/bin/
RUN ln -s /usr/local/bin/guile /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
