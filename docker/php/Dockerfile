FROM php:8.3-fpm-alpine

LABEL org.opencontainers.image.authors="alexweb.fi@gmail.com"

COPY --from=composer:lts /usr/bin/composer /usr/bin/composer

# dependencies required for running "phpize"
# these get automatically installed and removed by "docker-php-ext-*" (unless they're already installed)
ENV PHPIZE_DEPS \
    autoconf \
    dpkg-dev dpkg \
    file \
    g++ \
    gcc \
    libc-dev \
    make \
    pkgconf \
    re2c

ENV COMPOSER_ALLOW_SUPERUSER=1


RUN set -eux; \
	apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		argon2-dev \
		coreutils \
		curl-dev \
		gnu-libiconv-dev \
		libsodium-dev \
		libxml2-dev \
		linux-headers \
		oniguruma-dev \
		openssl-dev \
		readline-dev \
		sqlite-dev \
    zlib-dev \
    libpng-dev \
    jpeg-dev \
    freetype-dev \
    libwebp-dev

# enable Drupal required minimal extensions

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && \
    docker-php-ext-install gd && docker-php-ext-enable gd
RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache
RUN docker-php-ext-install pdo_mysql && \
    docker-php-ext-enable pdo_mysql
RUN docker-php-ext-install bcmath && \
    docker-php-ext-enable bcmath

# remove packages not needed for runtime

RUN set -eux; \
    apk del --no-network .build-deps;

# add packages needed for runtime

RUN set -eux; \
	apk add zlib libpng libjpeg freetype libwebp
