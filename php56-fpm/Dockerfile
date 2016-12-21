FROM alpine:edge
MAINTAINER Andy Postnikov <andypost@ya.ru>

ENV PHPRUN_DEPS \
  curl \
  mariadb-client \
  openssl \
  patch \
  sqlite

RUN set -e \
  && mkdir -p /var/www \
  && adduser -u 1000 -D -h /var/www/html www-data \
  && apk add --no-cache \
  php5 \
  php5-fpm \
  php5-apcu \
  php5-bcmath \
  php5-ctype \
  php5-curl \
  php5-dom \
  php5-gd \
  php5-gmp \
  php5-json \
  php5-mcrypt \
  php5-opcache \
  php5-openssl \
  php5-pcntl \
  php5-pdo_mysql \
  php5-pdo_sqlite \
  php5-phar \
  php5-xdebug \
  php5-xml \
  php5-zlib \
  $PHPRUN_DEPS \
  && rm -fr /var/cache/apk/* \
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('SHA384', 'composer-setup.php') === 'aa96f26c2b67226a324c27919f1eb05f21c248b987e6195cad9690d5c1ff713d53020a02ac8c217dbf90a7eacc9d141d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
  --install-dir=/usr/bin \
  --filename=composer \
  && php -r "unlink('composer-setup.php');" \
  && php -r "copy('http://files.drush.org/drush.phar', '/usr/bin/drush');" \
  && chmod +x /usr/bin/drush

COPY php-fpm.conf /etc/php5/
COPY php.ini /etc/php5/conf.d/xx-drupal.ini

#USER www-data
WORKDIR /var/www/html
#VOLUME /var/www/html

EXPOSE 9000

CMD ["php-fpm", "-F"]