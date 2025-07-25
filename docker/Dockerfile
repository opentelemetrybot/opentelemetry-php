FROM composer:2 AS composer
FROM debian:bullseye
WORKDIR /usr/src/myapp

RUN apt-get update  \
    && apt-get install -y --no-install-recommends git wget gnupg2 \
    && rm -rf /var/lib/apt/lists/* \
    && groupadd --gid 1000 php \
    && useradd --system --uid 1000 --gid php --shell /bin/bash --create-home php

RUN apt-get update && apt-get install -y lsb-release apt-transport-https ca-certificates \
  && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list \
  && wget -qO - https://packages.sury.org/php/apt.gpg | apt-key add - \
  && apt-get update

ARG PHP_VERSION=8.3

RUN apt-get install -y \
    php${PHP_VERSION}-ast \
    php${PHP_VERSION}-cli \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-dev \
    php${PHP_VERSION}-grpc \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-opcache \
    php${PHP_VERSION}-opentelemetry \
    php${PHP_VERSION}-protobuf \
    php${PHP_VERSION}-simplexml \
    php${PHP_VERSION}-sockets \
    php${PHP_VERSION}-xdebug \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-mongodb \
    php${PHP_VERSION}-amqp \
    php${PHP_VERSION}-rdkafka \
    php${PHP_VERSION}-mysqli \
    unzip

COPY --from=composer /usr/bin/composer /usr/local/bin/composer

RUN echo ";grpc.enable_fork_support = 1" > $(php-config --ini-dir)/40-otel-dev.ini \
  && echo "grpc.poll_strategy = epoll1" >> $(php-config --ini-dir)/40-otel-dev.ini \
  && echo "zend.assertions = 1" >> $(php-config --ini-dir)/40-otel-dev.ini

USER php
