FROM php:7.1-fpm

RUN apt-get update \
  && apt-get install -y zlib1g-dev libicu-dev g++ libzip-dev git libldap2-dev libxml2-dev \
  && docker-php-ext-configure intl && docker-php-ext-install intl \
  && docker-php-ext-configure pcntl  && docker-php-ext-install pcntl \
  && docker-php-ext-install pdo_mysql \
  && pecl install xdebug && docker-php-ext-enable xdebug \
  && docker-php-ext-install bcmath \
  && pecl install zip && docker-php-ext-enable zip \
  && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && docker-php-ext-install ldap \
  && docker-php-ext-install soap \
  && docker-php-ext-install sockets \
  && pecl install redis && docker-php-ext-enable redis

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


COPY php-fpm.conf /etc/php-fpm.conf
COPY php.ini /usr/local/etc/php/php.ini

WORKDIR /var/www/app

USER root

RUN chown -R www-data:www-data /var/www/app
RUN chmod -R 777  /var/www/app



