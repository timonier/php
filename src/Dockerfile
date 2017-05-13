FROM php:fpm

LABEL \
    maintainer="Morgan Auchede <morgan.auchede@gmail.com>"

RUN set -eux \

    # Prepare system

    && export DEBIAN_FRONTEND=noninteractive \

    && apt-get update \

    # Install packages

    && apt-get install --no-install-recommends --yes \
           git \
           locales \

    # Install php-acpu

    && export BUILD_DEPS_PHP_APCU="libpcre3-dev" \

    && apt-get install --no-install-recommends --yes \
           ${BUILD_DEPS_PHP_APCU} \
           libpcre3 \

    && pecl install apcu \
    && docker-php-ext-enable apcu \

    # Install php-bcmath

    && docker-php-ext-install bcmath \

    # Install php-blackfire

    && export PHP_VERSION="$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;")" \

    && curl --location --user-agent Docker "https://blackfire.io/api/v1/releases/probe/php/linux/amd64/${PHP_VERSION}" | tar --directory /tmp --extract --gzip \
    && mv /tmp/blackfire-*.so "$(php -r "echo ini_get('extension_dir');")/blackfire.so" \
    && docker-php-ext-enable blackfire \

    # Install php-gd

    && export BUILD_DEPS_PHP_GD="libfreetype6-dev libjpeg62-turbo-dev libpng12-dev" \

    && apt-get install --no-install-recommends --yes \
            ${BUILD_DEPS_PHP_GD} \
            libfreetype6 \
            libjpeg62-turbo \
            libpng12-0 \

    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \

    # Install php-gnupg

    && export BUILD_DEPS_PHP_GNUPG="libgpgme11-dev" \

    && apt-get install --no-install-recommends --yes \
            ${BUILD_DEPS_PHP_GNUPG} \
            libgpgme11 \

    && pecl install gnupg \
    && docker-php-ext-enable gnupg \

    # Install php-igbinary

    && pecl install igbinary \
    && docker-php-ext-enable igbinary \

    # Install php-intl

    && export BUILD_DEPS_PHP_INTL="libicu-dev" \

    && apt-get install --no-install-recommends --yes \
            ${BUILD_DEPS_PHP_INTL} \
            libicu52 \

    && docker-php-ext-install intl \

    # Install php-opcache

    && docker-php-ext-install opcache \

    # Install php-pgsql

    && export BUILD_DEPS_PHP_PGSQL="libpq-dev" \

    && apt-get install --no-install-recommends --yes \
            ${BUILD_DEPS_PHP_PGSQL} \
            libpq5 \

    && docker-php-ext-install pdo_pgsql pgsql \

    # Install php-mysql

    && docker-php-ext-install pdo_mysql \

    # Install php-redis

    && pecl install redis \
    && docker-php-ext-enable redis \

    # Install php-xdebug

    && pecl install xdebug \

    # Install php-xml

    && export BUILD_DEPS_PHP_XML="libxml2-dev" \

    && apt-get install --no-install-recommends --yes \
            ${BUILD_DEPS_PHP_XML} \

    && docker-php-ext-install xml \

    # Install php-zip

    && docker-php-ext-install zip \

    # Install timonier/dumb-entrypoint

    && curl --location "https://github.com/timonier/dumb-entrypoint/raw/master/bin/installer" | INSTALL_DIRECTORY="/usr/sbin" sh -s install \

    # Clean

    && apt-get remove -y --purge \
           ${BUILD_DEPS_PHP_APCU} \
           ${BUILD_DEPS_PHP_GD} \
           ${BUILD_DEPS_PHP_GNUPG} \
           ${BUILD_DEPS_PHP_INTL} \
           ${BUILD_DEPS_PHP_PGSQL} \
           ${BUILD_DEPS_PHP_XML} \
    && apt-get autoremove --purge --yes \

    && rm -rf \
           /tmp/* \
           /var/lib/apt/lists/*

ENTRYPOINT [ "dumb-init" , "--" , "dumb-entrypoint" ]
