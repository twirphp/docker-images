FROM php:7.2-cli

RUN apt-get update \
    && apt-get install -y --no-install-recommends git zip unzip zlib1g-dev \
    && docker-php-ext-install -j$(nproc) bcmath zip

WORKDIR /app

RUN curl --silent --show-error https://getcomposer.org/installer | php
