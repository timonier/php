FROM debian:jessie-slim
MAINTAINER Morgan Auchede <morgan.auchede@gmail.com>

ENV \
    GOSU_VERSION=1.7 \
    PHP_VERSION=7.0.14 \
    PHP_APCU_VERSION=5.1.7 \
    PHP_GNUPG_VERSION=1.4.0 \
    PHP_IGBINARY_VERSION=2.0.0 \
    PHP_REDIS_VERSION=3.0.0 \
    PHP_XDEBUG_VERSION=1:2.5.0

RUN set -x \

    # Prepare system

    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
           ca-certificates \
           curl \
           wget \

    # Prepare sources

    && echo "deb http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list.d/dotdeb.org.list \
    && echo "deb-src http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list.d/dotdeb.org.list \
    && wget -O- http://www.dotdeb.org/dotdeb.gpg | apt-key add - \

    && echo "deb http://packages.blackfire.io/debian any main" > /etc/apt/sources.list.d/blackfire.list \
    && wget -O - https://packagecloud.io/gpg.key | apt-key add - \

    && apt-get update \

    # Install gosu

    && curl -sLo /usr/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64" \
    && chmod +x /usr/bin/gosu \

    # Install packages

    && BUILD_DEPS="libgpgme11-dev php7.0-dev=$PHP_VERSION* pkg-php-tools" \

    && apt-get install -y --no-install-recommends \
           $BUILD_DEPS \
           blackfire-php \
           libgpgme11 \
           php7.0-apcu=$PHP_APCU_VERSION* \
           php7.0-bcmath=$PHP_VERSION* \
           php7.0-cli=$PHP_VERSION* \
           php7.0-curl=$PHP_VERSION* \
           php7.0-fpm=$PHP_VERSION* \
           php7.0-gd=$PHP_VERSION* \
           php7.0-igbinary=$PHP_IGBINARY_VERSION* \
           php7.0-intl=$PHP_VERSION* \
           php7.0-mbstring=$PHP_VERSION* \
           php7.0-mysql=$PHP_VERSION* \
           php7.0-pgsql=$PHP_VERSION* \
           php7.0-readline=$PHP_VERSION* \
           php7.0-redis=$PHP_REDIS_VERSION* \
           php7.0-sqlite=$PHP_VERSION* \
           php7.0-xdebug=$PHP_XDEBUG_VERSION* \
           php7.0-xml=$PHP_VERSION* \
           php7.0-zip=$PHP_VERSION* \

    # Compile php7.0-gnupg

    && pecl install gnupg-$PHP_GNUPG_VERSION \
    && echo "extension=gnupg.so" > /etc/php/7.0/mods-available/gnupg.ini \
    && ln -s /etc/php/7.0/mods-available/gnupg.ini /etc/php/7.0/cli/conf.d/20-gnupg.ini \
    && ln -s /etc/php/7.0/mods-available/gnupg.ini /etc/php/7.0/fpm/conf.d/20-gnupg.ini \

    # Disable xdebug

    && rm \
          /etc/php/7.0/cli/conf.d/20-xdebug.ini \
          /etc/php/7.0/fpm/conf.d/20-xdebug.ini \

    # Clean

    && apt-get remove -y --purge \
           $BUILD_DEPS \
    && apt-get autoremove -y --purge \

    && rm -rf \
           /tmp/* \
           /var/lib/apt/lists/*
