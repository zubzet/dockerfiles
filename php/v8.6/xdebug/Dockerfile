FROM ghcr.io/zubzet/php:8.6-apache

# php:8.6 ships without pecl, so install-php-extensions cannot build xdebug.
# Build the first 8.6-compatible release from source until that is fixed.
ARG XDEBUG_VERSION=3.6.0alpha1

RUN apt-get update \
 && apt-get install -y --no-install-recommends $PHPIZE_DEPS \
 && curl -fsSL "https://github.com/xdebug/xdebug/archive/refs/tags/${XDEBUG_VERSION}.tar.gz" | tar -xz -C /tmp \
 && cd "/tmp/xdebug-${XDEBUG_VERSION}" \
 && phpize && ./configure && make -j"$(nproc)" && make install \
 && docker-php-ext-enable xdebug \
 && cd / && rm -rf "/tmp/xdebug-${XDEBUG_VERSION}" \
 && apt-get purge -y --auto-remove $PHPIZE_DEPS \
 && rm -rf /var/lib/apt/lists/*

COPY config-xdebug/xdebug.ini /usr/local/etc/php/conf.d/99-xdebug.ini
