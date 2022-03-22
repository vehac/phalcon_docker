FROM php:5.6-apache

# install modules
RUN apt-get update && apt-get install -y \
    git zlib1g-dev libxml2-dev libmcrypt-dev curl libcurl4-gnutls-dev libssl-dev \
    mariadb-client openssl libmemcached11 libmemcachedutil2 build-essential libmemcached-dev libz-dev vim nano zip unzip wget

# install mongodb ext
RUN pecl install mongo && docker-php-ext-enable mongo
RUN pecl install mongodb-1.7.4 && docker-php-ext-enable mongodb

# install ext php
RUN docker-php-ext-configure intl && docker-php-ext-install intl \
&& docker-php-ext-install zip pdo pdo_mysql mysqli mbstring xml \
&& docker-php-ext-install soap && docker-php-ext-install mcrypt \
&& docker-php-ext-install mbstring && docker-php-ext-install curl \
&& docker-php-ext-install bcmath && docker-php-ext-install sockets

# active a2enmod and install composer
RUN a2enmod rewrite && a2enmod ssl && a2enmod headers \
    && curl -sS https://getcomposer.org/installer \
    | php -- --install-dir=/usr/local/bin --filename=composer

# install ext memcached and clean the image
RUN pecl install memcached-2.2.0 \
&& echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini
RUN rm -rf /var/lib/apt/lists/* /tmp/*

# install phalcon
ENV PHALCON_VERSION=3.4.2

RUN curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
&& cd cphalcon-${PHALCON_VERSION}/build \
&& ./install \
&& cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
&& cd ../../ \
&& rm -r cphalcon-${PHALCON_VERSION}

WORKDIR /home

RUN wget -O phalcon-devtools-3.4.8.zip https://github.com/phalcon/phalcon-devtools/archive/refs/tags/v3.4.8.zip \
&& unzip phalcon-devtools-3.4.8.zip && rm phalcon-devtools-3.4.8.zip \
&& cd phalcon-devtools-3.4.8 \
&& ln -sf /home/phalcon-devtools-3.4.8/phalcon /usr/bin/phalcon

ADD ./localhost.conf /etc/apache2/sites-enabled/000-default.conf

RUN useradd -ms /bin/bash developer
USER developer

EXPOSE 80

WORKDIR /var/www/html
