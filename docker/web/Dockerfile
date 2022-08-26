FROM php:8.1.9-apache-bullseye

# apacheの設定ファイルをコンテナにコピー
COPY ./apache/000-default.conf /etc/apache2/sites-available/000-default.conf
# phpの設定ファイルをコンテナにコピー
COPY ./php/php.ini /usr/local/etc/php/php.ini

# パッケージの更新及び必要ソフトのインストール
RUN apt-get update \
  && apt-get install -yqq \
  zlib1g-dev \
  libpq-dev \
  mariadb-client \
  unzip \
  libzip-dev \
  zip \
  && docker-php-ext-install \
  pdo_mysql \
  mysqli \
  && docker-php-ext-enable mysqli \
  # xdebug install
  && pecl install xdebug \
  && docker-php-ext-enable xdebug

# jwilder/nginx-proxyからこのコンテナにルーティングされる様に指定
ENV VIRTUAL_HOST=test.example.com

# Composer Install
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# Node.js Install
RUN apt-get install -y nodejs \
  npm \
  && npm install n -g \ 
  && n 14.17.6

WORKDIR /var/www/html

RUN composer global require "laravel/installer"