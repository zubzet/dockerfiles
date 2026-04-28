FROM php:8.3-apache

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN apt-get -o Acquire::Retries=3 update \
 && apt-get -o Acquire::Retries=3 install -y --no-install-recommends \
      ca-certificates git curl unzip \
 && install-php-extensions \
      bcmath bz2 calendar curl exif gd intl mbstring \
      mysqli pdo_mysql sockets iconv session zip \
      xmlreader xmlwriter simplexml \
 && a2enmod rewrite \
 && curl -sS https://getcomposer.org/installer | php -- \
      --install-dir=/usr/local/bin --filename=composer \
 && rm -rf /var/lib/apt/lists/*

COPY config-apache/site-000-default.conf /etc/apache2/sites-available/000-default.conf

WORKDIR /var/www/html
