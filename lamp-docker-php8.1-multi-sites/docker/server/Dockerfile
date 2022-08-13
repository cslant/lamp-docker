FROM php:8.1-apache

RUN apt-get update && apt-get install -y git unzip zip nano nodejs

WORKDIR /var/www

RUN apt-get update && apt-get install -y \
    libmcrypt-dev \
    openssl \
    vim cron \
    --no-install-recommends \
    && rm -r /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    jpegoptim optipng pngquant gifsicle \
    curl

RUN a2enmod rewrite
RUN a2enmod ssl

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g n \
    && n stable

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions gd pdo_mysql bcmath zip intl opcache mbstring exif

RUN pecl install -o xdebug-3.2.0alpha2

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY --from=composer:2.2 /usr/bin/composer /usr/local/bin/composer

EXPOSE 80
EXPOSE 443