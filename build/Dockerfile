FROM ubuntu:bionic

# see https://askubuntu.com/a/705893/168529
ENV LC_ALL C.UTF-8

ENV PHP_VERSION 7.3

RUN set -xe \
    && export DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
    && echo "tzdata tzdata/Areas select Europe" | debconf-set-selections \
    && echo "tzdata tzdata/Zones/Europe select Budapest" | debconf-set-selections \
    && apt-get update \
    && apt-get install -y --no-install-recommends software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y --no-install-recommends git ssh zip unzip zlib1g-dev wget \
    && apt-get install -y --no-install-recommends php${PHP_VERSION}-cli php${PHP_VERSION}-bcmath php${PHP_VERSION}-xml php${PHP_VERSION}-mbstring \
    && rm -rf /var/lib/apt/lists/*

ENV GOLANG_VERSION 1.12.4
ENV GOLANG_SHA256SUM d7d1f1f88ddfe55840712dc1747f37a790cbcaa448f6c9cf51bbe10aa65442f5

RUN set -xe \
    && wget -O go.tar.gz https://golang.org/dl/go${GOLANG_VERSION}.linux-amd64.tar.gz \
    && echo "${GOLANG_SHA256SUM} *go.tar.gz" | sha256sum -c - \
    && tar -C /usr/local -xzf go.tar.gz \
    && rm go.tar.gz

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN go version

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

# Install Composer
COPY composer-installer.sh .
RUN set -xe \
    && ./composer-installer.sh \
    && rm composer-installer.sh

# Install protoc
ENV PROTOC_VERSION 3.7.1

RUN set -xe \
    && mkdir -p /tmp/protoc \
    && wget -O /tmp/protoc/protoc.zip https://github.com/google/protobuf/releases/download/v${PROTOC_VERSION}/protoc-${PROTOC_VERSION}-linux-x86_64.zip \
    && unzip /tmp/protoc/protoc.zip -d /usr/local -x readme.txt \
    && rm -rf /tmp/protoc

# Install goreleaser
ENV GORELEASER_VERSION 0.105.0

RUN set -xe \
    && mkdir -p /tmp/goreleaser \
    && wget -O /tmp/goreleaser/goreleaser.tar.gz https://github.com/goreleaser/goreleaser/releases/download/v${GORELEASER_VERSION}/goreleaser_Linux_x86_64.tar.gz \
    && tar -C /tmp/goreleaser -xzf /tmp/goreleaser/goreleaser.tar.gz \
    && mv /tmp/goreleaser/goreleaser /usr/bin \
    && rm -rf /tmp/goreleaser

# Install client compatibility test suite
# TODO: install a specific commit/version
RUN go get github.com/twitchtv/twirp/clientcompat
