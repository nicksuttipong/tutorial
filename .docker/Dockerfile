FROM php:5-apache

LABEL MAINTAINER Suttipongr <nick.suttipong@gmail.com>

COPY php.ini /usr/local/etc/php/

RUN docker-php-source extract \
  && apt-get update \
  && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libmagickwand-dev libxml2-dev git zip \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install pdo_mysql mysqli mbstring gd iconv mcrypt \ 
  && a2enmod rewrite \ 
  && docker-php-source delete \
  && docker-php-ext-install zip

RUN pecl install imagick && docker-php-ext-enable imagick

RUN apt-get install -y libmemcached11 libmemcachedutil2 build-essential libmemcached-dev libz-dev
RUN pecl install memcached-2.2.0
RUN echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ssl-cert-snakeoil.key -out /etc/ssl/certs/ssl-cert-snakeoil.pem -subj "/C=AT/ST=Vienna/L=Vienna/O=Security/OU=Development/CN=example.com"

RUN a2enmod rewrite
RUN a2ensite default-ssl
RUN a2enmod ssl

EXPOSE 80
EXPOSE 443