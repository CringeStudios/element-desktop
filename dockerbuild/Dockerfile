# Docker image to facilitate building Element Desktop's native bits using a glibc version (2.31)
# with broader compatibility, down to Debian bullseye & Ubuntu focal.
FROM rust:bullseye

ENV DEBIAN_FRONTEND noninteractive

RUN curl --proto "=https" -L https://yarnpkg.com/latest.tar.gz | tar xvz && mv yarn-* /yarn && ln -s /yarn/bin/yarn /usr/bin/yarn
RUN apt-get -qq update && apt-get -y -qq dist-upgrade && \
  apt-get -y -qq install --no-install-recommends \
  # tclsh is required for building SQLite as part of SQLCipher
  tcl \
  # libsecret-1-dev is required even for prebuild keytar
  libsecret-1-dev \
  # Used by seshat (when not SQLCIPHER_STATIC) \
  libsqlcipher-dev \
  rpm flatpak flatpak-builder wine && \
  apt-get purge -y --auto-remove && rm -rf /var/lib/apt/lists/*
RUN dpkg --add-architecture i386 && apt-get update && apt-get install -y wine32 mono-devel
RUN ln -s /usr/bin/python3 /usr/bin/python & ln -s /usr/bin/pip3 /usr/bin/pip

ENV DEBUG_COLORS true
ENV FORCE_COLOR true
ENV SQLCIPHER_BUNDLED 1

WORKDIR /project

ENV NODE_VERSION 22.9.0
ARG TARGETOS
ARG TARGETARCH
COPY setup.sh /setup.sh
RUN /setup.sh
