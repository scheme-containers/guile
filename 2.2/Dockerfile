# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential \
      libffi-dev \
      libgc-dev \
      libgmp-dev \
      libltdl-dev \
      libncurses-dev \
      libreadline-dev \
      libtinfo-dev \
      libunistring-dev \
      pkg-config \
      xz-utils \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://ftp.gnu.org/gnu/guile/guile-2.2.7.tar.xz guile.tar.xz
RUN sha256sum guile.tar.xz && sha256sum -c checksum
RUN mkdir guile && tar -C guile --strip-components 1 -xf guile.tar.xz
WORKDIR /build/guile
RUN ./configure
RUN make
RUN make check
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      libffi6 \
      libgc1c2 \
      libgmp10 \
      libltdl7 \
      libncurses6 \
      libreadline7 \
      libtinfo6 \
      libunistring2 \
 && rm -rf /var/lib/apt/lists/*
COPY scheme-script /usr/local/bin/
RUN ln -s /usr/bin/guile /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
