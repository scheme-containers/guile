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

RUN git clone --depth 1 git://git.sv.gnu.org/guile.git && cd guile && \
    git reset --hard a0fdb4efc18ce47520170a8ccfd7f3f4c5c99d47
WORKDIR guile
RUN ./autogen.sh
RUN ./configure
RUN make -j`nproc`
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
       libltdl7
COPY --from=build /usr/local/ /usr/local/
COPY scheme-script /usr/local/bin/
RUN ln -s /usr/local/bin/guile /usr/local/bin/scheme-banner
RUN ldconfig || true
CMD ["scheme-banner"]
