# Set ARG variables for PHP version and extensions
ARG PHP_VERSION
ARG PHP_EXTENSIONS

# Use Alpine base image with specified PHP version
FROM php:${PHP_VERSION}-fpm-alpine AS builder

# Install dependencies and PHP extensions
RUN apk add --no-cache \
   bash \
   curl \
   freetype-dev \
   libjpeg-turbo-dev \
   libpng-dev \
   libzip-dev \
   icu-dev \
   oniguruma-dev \
   gmp-dev \
   libxml2-dev \
   postgresql-dev \
   && docker-php-ext-configure gd --with-freetype --with-jpeg \
   && docker-php-ext-install -j$(nproc) \
      bcmath \
      gd \
      intl \
      mbstring \
      pdo \
      pdo_mysql \
      pdo_pgsql \
      xml \
      zip \
      soap \
      gmp \
      opcache \ 
      && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install specified PHP extensions
RUN if [ -n "$PHP_EXTENSIONS" ]; then \
   IFS=','; \
   for ext in $PHP_EXTENSIONS; do \
      docker-php-ext-install $ext; \
   done; \
fi

WORKDIR /var/www/html

# run composer install
RUN [ -e ./composer.json ] && [ -e ./composer.lock ] && composer install --prefer-dist --no-dev --no-autoloader --no-scripts --no-progress --no-interaction || true

# Install application dependencies
# RUN composer dump-autoload --optimize