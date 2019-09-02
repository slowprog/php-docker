FROM php:7.3-cli-alpine

ENV PHP_REDIS_VERSION=3.1.6
ENV PHP_APCU_VERSION=5.1.8

RUN apk add --no-cache --virtual .persistent-deps \
    git \
    icu-libs \
    libzip-dev \
    postgresql-dev

RUN set -xe \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        icu-dev \
        zlib-dev \
        zip \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install \
        intl \
        mbstring \
        bcmath \
        pcntl \
        zip \
        pdo_pgsql \
    && yes | pecl install \
        apcu-$PHP_APCU_VERSION \
        igbinary \
        redis-$PHP_REDIS_VERSION \
        xdebug \
    && docker-php-ext-enable \
        apcu \
        igbinary \
        redis \
    && apk del .build-deps

WORKDIR /app

COPY ./common/root/ /

RUN rm /usr/local/etc/php/conf.d/xdebug.ini \
    && rm /entrypoint.sh \
    && rm /run_nginx.sh \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer