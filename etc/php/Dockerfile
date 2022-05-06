FROM php:8.1-fpm-alpine

# RUN apk update && apk upgrade  \
#         && apk add php8-intl 
RUN set -xe \
    && apk add --update \
        $PHPIZE_DEPS \
        icu \
        freetype-dev \
		git \
		libjpeg-turbo-dev \
		libpng-dev \
        libzip-dev \
        imagemagick \
		imagemagick-libs \
		imagemagick-dev \
		php7-imagick \
        php7-zip \
        php-ldap \
    && apk add --no-cache --virtual .php-deps \
        make \
    && apk add --no-cache --virtual .build-deps \
        zlib-dev \
        icu-dev \
        g++ \        
    && docker-php-ext-configure gd --with-jpeg --with-freetype \
	&& docker-php-ext-install gd \
	&& docker-php-ext-install zip \
    && docker-php-ext-install mysqli pdo pdo_mysql \
    && docker-php-ext-configure intl  \
    && docker-php-ext-install \
        intl \
    && docker-php-ext-enable intl \
     &&    apk add --no-cache --virtual .build-dependencies-in-virtual-world openldap-dev \
    && docker-php-ext-install ldap \
    && docker-php-ext-enable  ldap \
    && apk del .build-dependencies-in-virtual-world \
    && { find /usr/local/lib -type f -print0 | xargs -0r strip --strip-all -p 2>/dev/null || true; } \
    && apk del .build-deps \
    && rm -rf /tmp/* /usr/local/lib/php/doc/* /var/cache/apk/*

RUN printf "\n" | pecl install \
		imagick && \
		docker-php-ext-enable --ini-name 20-imagick.ini imagick

# RUN pecl install imagick \
#     && docker-php-ext-enable imagick
