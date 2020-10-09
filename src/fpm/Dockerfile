FROM php:7.4-fpm-alpine

LABEL \
    maintainer="Morgan AUCHEDE <morgan.auchede@gmail.com>"

ENV \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_KEEP_ENV=1 \
    S6_READ_ONLY_ROOT=1

RUN set -e -u -x \
\
    # Install packages
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS ${PHPIZE_DEPS} \
    && apk add --no-cache --no-progress bash ca-certificates git openssh-client shadow tzdata \
\
    # Install dockerize
\
    && curl --location "https://github.com/jwilder/dockerize/releases/download/v0.6.1/dockerize-linux-amd64-v0.6.1.tar.gz" | tar --directory /usr/local/bin --extract --gzip \
    && chmod +x /usr/local/bin/dockerize \
\
    # Install php-ext-apfd
\
    && pecl install apfd \
    && docker-php-ext-enable apfd \
\
    # Install php-ext-amqp
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_AMQP rabbitmq-c-dev \
    && apk add --no-cache --no-progress rabbitmq-c \
    && pecl install amqp \
    && docker-php-ext-enable amqp \
\
    # Install php-ext-apcu
\
    && pecl install apcu \
    && docker-php-ext-enable apcu \
\
    # Install php-ext-bcmath
\
    && docker-php-ext-install bcmath \
\
    # Install php-ext-blackfire
\
    && PHP_VERSION="$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;")" \
    && curl --location --user-agent Docker "https://blackfire.io/api/v1/releases/probe/php/alpine/amd64/${PHP_VERSION}" | tar --directory /tmp --extract --gzip \
    && mv /tmp/blackfire-*.so "$(php -r "echo ini_get('extension_dir');")/blackfire.so" \
    && docker-php-ext-enable blackfire \
\
    # Install php-ext-exif
\
    && docker-php-ext-install exif \
\
    # Install php-ext-gd
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_GD freetype-dev libjpeg-turbo-dev libpng-dev \
    && apk add --no-cache --no-progress freetype libjpeg-turbo libpng \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
\
    # Install php-ext-gmp
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_GMP gmp-dev \
    && apk add --no-cache --no-progress gmp \
    && docker-php-ext-install gmp \
\
    # Install php-ext-gnupg
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_GNUPG gpgme-dev \
    && apk add --no-cache --no-progress gpgme \
    && pecl install gnupg \
    && docker-php-ext-enable gnupg \
\
    # Install php-ext-igbinary
\
    && pecl install igbinary \
    && docker-php-ext-enable igbinary \
\
    # Install php-ext-imagick
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_IMAGICK imagemagick-dev \
    && apk add --no-cache --no-progress imagemagick \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
\
    # Install php-ext-intl
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_INTL icu-dev \
    && apk add --no-cache --no-progress icu \
    && docker-php-ext-install intl \
\
    # Install php-ext-mysql
\
    && docker-php-ext-install pdo_mysql \
\
    # Install php-ext-opcache
\
    && docker-php-ext-install opcache \
\
    # Install php-ext-pgsql
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_PGSQL postgresql-dev \
    && apk add --no-cache --no-progress postgresql-libs \
    && docker-php-ext-install pdo_pgsql pgsql \
\
    # Install php-ext-redis
\
    && pecl install redis \
    && docker-php-ext-enable redis \
\
    # Install php-ext-uuid
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_UUID util-linux-dev \
    && apk add --no-cache --no-progress libuuid \
    && pecl install uuid \
    && docker-php-ext-enable uuid \
\
    # Install php-ext-xdebug
\
    && pecl install xdebug \
\
    # Install php-ext-xml
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_XML libxml2-dev \
    && apk add --no-cache --no-progress libxml2 \
    && docker-php-ext-install xml \
\
    # Install php-ext-zip
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS_PHP_ZIP libzip-dev \
    && apk add --no-cache --no-progress libzip \
    && docker-php-ext-install zip \
\
    # Install s6-overlay
\
    && curl "https://keybase.io/justcontainers/key.asc" | gpg --import \
    && curl --location --output /tmp/s6-overlay.tar.gz "https://github.com/just-containers/s6-overlay/releases/download/v2.1.0.0/s6-overlay-amd64.tar.gz" \
    && curl --location --output /tmp/s6-overlay.tar.gz.sig "https://github.com/just-containers/s6-overlay/releases/download/v2.1.0.0/s6-overlay-amd64.tar.gz.sig" \
    && gpg --verify /tmp/s6-overlay.tar.gz.sig /tmp/s6-overlay.tar.gz \
    && tar --directory / --extract --file /tmp/s6-overlay.tar.gz --gzip \
\
    # Clean
\
    && apk del --no-progress BUILD_DEPS BUILD_DEPS_PHP_AMQP BUILD_DEPS_PHP_GD BUILD_DEPS_PHP_GMP BUILD_DEPS_PHP_GNUPG BUILD_DEPS_PHP_INTL BUILD_DEPS_PHP_PGSQL BUILD_DEPS_PHP_XML BUILD_DEPS_PHP_ZIP \
    && rm -f -r /root/.gnupg /tmp/* /usr/local/etc/php-fpm.d/* /var/www

COPY rootfs/ /

ENTRYPOINT [ "/init" ]
