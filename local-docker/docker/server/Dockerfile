FROM php:7.4-apache

RUN a2enmod rewrite
RUN a2enmod ssl

RUN apt-get update && apt-get install -y git unzip zip nano nodejs

WORKDIR /var/www/local

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions gd pdo_mysql bcmath zip intl opcache mbstring exif

COPY --from=composer:2.2 /usr/bin/composer /usr/local/bin/composer

EXPOSE 80
EXPOSE 443