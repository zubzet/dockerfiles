FROM php:8.3-cli

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      ca-certificates git curl unzip \
 && install-php-extensions \
      bcmath bz2 calendar curl exif gd intl mbstring \
      mysqli pdo_mysql sockets iconv session zip \
      xmlreader xmlwriter simplexml \
 && curl -sS https://getcomposer.org/installer | php -- \
      --install-dir=/usr/local/bin --filename=composer \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html
