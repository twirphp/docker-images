FROM ubuntu:xenial

# see https://askubuntu.com/a/705893/168529
ENV LC_ALL C.UTF-8

ENV PHP_VERSION 7.2

RUN set -xe \
    && apt-get update \
    && apt-get install -y --no-install-recommends python-software-properties software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y --no-install-recommends git zip unzip zlib1g-dev wget \
    && apt-get install -y --no-install-recommends php${PHP_VERSION}-cli php${PHP_VERSION}-bcmath php${PHP_VERSION}-xml php${PHP_VERSION}-mbstring \
    && rm -rf /var/lib/apt/lists/*

ENV GOLANG_VERSION 1.10.1
ENV GOLANG_SHA256SUM 72d820dec546752e5a8303b33b009079c15c2390ce76d67cf514991646c6127b

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

# Install dep
RUN INSTALL_DIRECTORY=/usr/bin wget -qO- https://raw.githubusercontent.com/golang/dep/master/install.sh | sh

# Install packr
ENV PACKR_VERSION 1.10.11

RUN set -xe \
    && mkdir -p /tmp/packr \
    && wget -O /tmp/packr/packr.tar.gz https://github.com/gobuffalo/packr/releases/download/v${PACKR_VERSION}/packr_${PACKR_VERSION}_linux_amd64.tar.gz \
    && tar -C /tmp/packr -xzf /tmp/packr/packr.tar.gz \
    && mv /tmp/packr/packr /usr/bin \
    && rm -rf /tmp/packr

# Install protoc
ENV PROTOC_VERSION 3.5.1

RUN set -xe \
    && mkdir -p /tmp/protoc \
    && wget -O /tmp/protoc/protoc.zip https://github.com/google/protobuf/releases/download/v${PROTOC_VERSION}/protoc-${PROTOC_VERSION}-linux-x86_64.zip \
    && unzip /tmp/protoc/protoc.zip -d /usr/local -x readme.txt \
    && rm -rf /tmp/protoc
