FROM debian:wheezy-slim
MAINTAINER Morgan Auchede <morgan.auchede@gmail.com>

ENV \
    GOSU_VERSION=1.7 \
    PHP_VERSION=5.5.38

RUN set -x \

    # Prepare system

    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
           ca-certificates \
           curl \
           wget \

    # Prepare sources

    && echo "deb http://packages.dotdeb.org wheezy all" >> /etc/apt/sources.list.d/dotdeb.org.list \
    && echo "deb http://packages.dotdeb.org wheezy-php55 all" >> /etc/apt/sources.list.d/dotdeb.org.list \
    && echo "deb-src http://packages.dotdeb.org wheezy all" >> /etc/apt/sources.list.d/dotdeb.org.list \
    && echo "deb-src http://packages.dotdeb.org wheezy-php55 all" >> /etc/apt/sources.list.d/dotdeb.org.list \
    && wget -O- http://www.dotdeb.org/dotdeb.gpg | apt-key add - \

    && echo "deb http://packages.blackfire.io/debian any main" > /etc/apt/sources.list.d/blackfire.list \
    && wget -O - https://packagecloud.io/gpg.key | apt-key add - \

    && apt-get update \

    # Install gosu

    && curl -sLo /usr/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64" \
    && chmod +x /usr/bin/gosu \

    # Install packages

    && BUILD_DEPS="libgpgme11-dev php5-dev=$PHP_VERSION* pkg-php-tools" \

    && apt-get install -y --no-install-recommends \
           $BUILD_DEPS \
           blackfire-php \
           libgpgme11 \
           php5-apcu=$PHP_VERSION* \
           php5-cli=$PHP_VERSION* \
           php5-curl=$PHP_VERSION* \
           php5-fpm=$PHP_VERSION* \
           php5-gd=$PHP_VERSION* \
           php5-intl=$PHP_VERSION* \
           php5-mysql=$PHP_VERSION* \
           php5-pgsql=$PHP_VERSION* \
           php5-readline=$PHP_VERSION* \
           php5-redis=$PHP_VERSION* \
           php5-sqlite=$PHP_VERSION* \
           php5-xdebug=$PHP_VERSION* \

    # Compile php5-gnupg

    && pecl install gnupg \
    && echo "extension=gnupg.so" > /etc/php5/mods-available/gnupg.ini \
    && ln -s /etc/php5/mods-available/gnupg.ini /etc/php5/cli/conf.d/20-gnupg.ini \
    && ln -s /etc/php5/mods-available/gnupg.ini /etc/php5/fpm/conf.d/20-gnupg.ini \

    # Disable xdebug

    && rm \
          /etc/php5/cli/conf.d/20-xdebug.ini \
          /etc/php5/fpm/conf.d/20-xdebug.ini \

    # Clean

    && apt-get remove -y --purge \
           $BUILD_DEPS \
    && apt-get autoremove -y --purge \

    && rm -rf \
           /tmp/* \
           /var/lib/apt/lists/*
