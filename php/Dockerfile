ARG PHP_VERSION=""
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine
RUN apk add libzip-dev
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev && \
  docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} gd && \
  apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev
RUN docker-php-ext-install pdo pdo_mysql
RUN docker-php-ext-install mysqli
RUN set -xe \
    && apk add --update \
        icu \
    && apk add --no-cache --virtual .php-deps \
        make \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        zlib-dev \
        icu-dev \
        g++ \
    && docker-php-ext-configure intl \
    && docker-php-ext-install \
        intl \
    && docker-php-ext-enable intl \
    && { find /usr/local/lib -type f -print0 | xargs -0r strip --strip-all -p 2>/dev/null || true; } \
    && apk del .build-deps \
    && rm -rf /tmp/* /usr/local/lib/php/doc/* /var/cache/apk/*
RUN mkdir /home/storage/
RUN rm /usr/local/etc/php-fpm.d/www.conf
COPY www.conf /usr/local/etc/php-fpm.d/www.conf
RUN apk --no-cache update \
    && apk --no-cache upgrade \
    && apk add --no-cache \
    mysql-client \
    php7-mysqli \
    php7-pdo \
    freetype \
    libpng \
    freetype-dev \
    libpng-dev \
    jpeg-dev \
    libjpeg \
    libjpeg-turbo-dev \
    wget \
    zlib-dev \
    ttf-freefont \
    fontconfig \
    xvfb \
    libxrender-dev \
    gettext \
    gettext-dev \
    libxml2-dev \
    gnu-libiconv-dev \
    autoconf \
    g++ \
    git \
    bash \
    wkhtmltopdf
RUN cp /usr/bin/wkhtmltopdf /usr/local/bin