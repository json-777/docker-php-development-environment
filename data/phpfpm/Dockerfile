FROM php:7-fpm-alpine
RUN apk add curl tar
RUN set -x && \
  apk update && \
  apk add --no-cache icu-libs && \
  apk add --no-cache --virtual build-dependencies icu-dev && \
  apk add --no-cache autoconf build-base ssmtp && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} intl && \
  docker-php-ext-install -j${NPROC} pdo_mysql && \
  docker-php-ext-install -j${NPROC} mysqli && \
  apk del --no-cache --purge build-dependencies && \
  pecl channel-update pecl.php.net && \
  pecl install xdebug-2.6.0 && \
  docker-php-ext-enable xdebug && \
  rm -rf /tmp/pear
RUN set -x && \
  curl -sf -o composer_install.php https://getcomposer.org/installer  && \
  chmod u+x composer_install.php && \
  php composer_install.php --install-dir=/usr/bin --filename=composer && \
  rm composer_install.php
RUN set -x && \
  deluser www-data && \
  addgroup -g 50 -S www-data && \
  adduser -u 1000 -D -S -G www-data www-data
WORKDIR /var/www/html
COPY ./docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./php.ini /usr/local/etc/php/php.ini
COPY ./ssmtp.conf /etc/ssmtp/ssmtp.conf
