ARG PHP_VERSION_SELECTED

FROM php:${PHP_VERSION_SELECTED}-apache

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


RUN docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-enable mysqli

ENV ACCEPT_EULA=Y
RUN apt-get update && apt-get install -y gnupg2
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get -y --no-install-recommends install msodbcsql17 unixodbc-dev
RUN pecl install sqlsrv
RUN pecl install pdo_sqlsrv
RUN docker-php-ext-enable sqlsrv pdo_sqlsrv

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions gd pdo_mysql bcmath zip intl opcache mbstring exif

# install xdebug based on the PHP version
RUN if [ ${PHP_VERSION_SELECTED} = "8.1" ]; then \
    pecl install xdebug-3.1.0; \
    docker-php-ext-enable xdebug; \
    else \
    pecl install xdebug-3.1.0; \
    docker-php-ext-enable xdebug; \
    fi

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY --from=composer:2.2 /usr/bin/composer /usr/local/bin/composer

EXPOSE 80
EXPOSE 443