FROM php:5.5.35-cli
MAINTAINER Morgan Auchede <morgan.auchede@gmail.com>

ENV \
    PHP_PEAR_PHP_BIN=/usr/bin/php

RUN set -x \

    # Prepare system

    && export DEBIAN_FRONTEND=noninteractive \

    && apt-get update \

    && mv /usr/local/bin/* /usr/bin/ \
    && mv /usr/local/etc/php /etc/php \
    && ln -s /etc/php /usr/local/etc/php \

    # Install packages

    && apt-get install -y --no-install-recommends \
           git \
           openssh-client \

    # Install php-bcmath

    && docker-php-ext-install bcmath \
    && mv /etc/php/conf.d/docker-php-ext-bcmath.ini /etc/php/conf.d/00_bcmath.ini \

    # Install php-bz2

    && apt-get install -y --no-install-recommends \
           libbz2-dev \
    && docker-php-ext-install bz2 \
    && mv /etc/php/conf.d/docker-php-ext-bz2.ini /etc/php/conf.d/00_bz2.ini \

    # Install php-curl

    && apt-get install -y --no-install-recommends \
           libcurl4-openssl-dev \
    && docker-php-ext-install curl \

    # Install php-gd

    && apt-get install -y --no-install-recommends \
           libfreetype6-dev \
           libjpeg62-turbo-dev \
           libpng12-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && mv /etc/php/conf.d/docker-php-ext-gd.ini /etc/php/conf.d/00_gd.ini \

    # Install php-gnupg

    && apt-get install -y --no-install-recommends \
           libgpgme11-dev \
    && pecl install gnupg \
    && echo "extension=gnupg.so" > /etc/php/conf.d/00_gnupg.ini \

    # Install php-intl

    && apt-get install -y --no-install-recommends \
           libicu-dev \
    && docker-php-ext-install intl \
    && mv /etc/php/conf.d/docker-php-ext-intl.ini /etc/php/conf.d/00_intl.ini \

    # Install php-json

    && docker-php-ext-install json \

    # Install php-mbstring

    && docker-php-ext-install mbstring \

    # Install php-mysql

    && docker-php-ext-install pdo_mysql mysql \
    && mv /etc/php/conf.d/docker-php-ext-mysql.ini /etc/php/conf.d/00_mysql.ini \
    && mv /etc/php/conf.d/docker-php-ext-pdo_mysql.ini /etc/php/conf.d/00_pdo_mysql.ini \

    # Install php-opcache

    && docker-php-ext-install opcache \
    && echo "zend_extension=opcache.so" > /etc/php/conf.d/docker-php-ext-opcache.ini \
    && mv /etc/php/conf.d/docker-php-ext-opcache.ini /etc/php/conf.d/00_opcache.ini \

    # Install php-pgsql

    && apt-get install -y --no-install-recommends \
           libpq-dev \
    && docker-php-ext-install pdo_pgsql pgsql \
    && mv /etc/php/conf.d/docker-php-ext-pdo_pgsql.ini /etc/php/conf.d/00_pdo_pgsql.ini \
    && mv /etc/php/conf.d/docker-php-ext-pgsql.ini /etc/php/conf.d/00_pgsql.ini \

    # Install php-xdebug

    && pecl install xdebug \

    # Install php-zip

    && apt-get install -y --no-install-recommends \
           zlib1g-dev \
    && docker-php-ext-install zip \
    && mv /etc/php/conf.d/docker-php-ext-zip.ini /etc/php/conf.d/00_zip.ini \

    # Clean

    && rm -rf \
           /tmp/* \
           /var/lib/apt/lists/*

ENTRYPOINT [ "/usr/bin/php" ]
