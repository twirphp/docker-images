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
    && apt-get install -y --no-install-recommends php${PHP_VERSION}-cli php${PHP_VERSION}-bcmath \
    && rm -rf /var/lib/apt/lists/*

RUN set -xe \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "copy('https://composer.github.io/installer.sig', 'composer-setup.sig');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === trim(file_get_contents('composer-setup.sig'))) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');"


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
