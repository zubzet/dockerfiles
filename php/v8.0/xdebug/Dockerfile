FROM ghcr.io/zubzet/php:8.0-apache

RUN install-php-extensions xdebug

COPY config-xdebug/xdebug.ini /usr/local/etc/php/conf.d/99-xdebug.ini
