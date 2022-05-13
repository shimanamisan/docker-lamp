FROM php:7.4.13-apache
COPY ./php/php.ini /usr/local/etc/php/
# COPY ./apache/*.conf /etc/apache2/sites-enabled/

# パッケージの更新及び必要ソフトのインストール
RUN apt-get update \
&& apt-get install -yqq zlib1g-dev libpq-dev mariadb-client unzip libzip-dev zip unzip \
&& docker-php-ext-install zip pdo_mysql mysqli \
&& docker-php-ext-enable mysqli \
# mod_rewriteを有効化
&& a2enmod rewrite \
# Apacheのプロキシ機能と、httpでのプロキシ機能を有効化
&& a2enmod proxy && a2enmod proxy_http \
# Apacheでヘッダーを編集する機能を有効
&& a2enmod headers \
# URLを都度置き換える機能を有効化
&& a2enmod rewrite

# Composer Install
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# Node.js Install
RUN apt-get install -y nodejs npm && npm install n -g && n 14.17.6

WORKDIR /var/www/html

RUN composer global require "laravel/installer"