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

WORKDIR /var/www/app

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php && \
  mv composer.phar /usr/local/bin/composer

# Install Node.js
ENV NODE_VERSION 6.1.0
RUN set -ex \
      && for key in \
        9554F04D7259F04124DE6B476D5A82AC7E37093B \
        94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
        0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \
        FD3A5288F042B6850C66B31F09FE44734EB7990E \
        71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
        DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
        B9AE9905FFD7803F25714661B63B535A4C206CA9 \
        C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
      ; do \
        gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
      done
RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
      && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
      && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
      && grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
      && tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
      && rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
      && apt-get install -y build-essential

# yarn package manager
RUN curl -o- -L https://yarnpkg.com/install.sh | bash

COPY php-fpm.conf /etc/php-fpm.conf
COPY php.ini /usr/local/etc/php/php.ini



USER root

RUN chown -R www-data:www-data /var/www/app
RUN chmod -R 777  /var/www/app



