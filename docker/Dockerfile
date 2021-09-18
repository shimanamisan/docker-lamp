FROM php:7.4.13-apache
COPY ./php/php.ini /usr/local/etc/php/
COPY ./apache/*.conf /etc/apache2/sites-enabled/
<<<<<<< HEAD
=======
COPY ./mysql/init/* /docker-entrypoint-initdb.d/
RUN mkdir /var/www/html/sample_laravel
>>>>>>> cb83504f63835da42bcc9a7d9a658287b18bb7d5

# パッケージの更新及び必要ソフトのインストール
RUN apt-get update \
&& apt-get install -yqq zlib1g-dev libpq-dev mariadb-client unzip libzip-dev zip unzip \
&& docker-php-ext-install zip pdo_mysql mysqli \
&& docker-php-ext-enable mysqli

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www/html

RUN composer global require "laravel/installer"