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
  # rewriteモジュールを有効化
  && a2enmod rewrite \
  && docker-php-ext-enable mysqli \
  # xdebug install
  && pecl install xdebug \
  && docker-php-ext-enable xdebug

# 指定したドメインでコンテナにルーティングされるように設定
# nginx-proxyに関連した設定
ENV VIRTUAL_HOST=test.example.com

# Composer Install
# 公式の Composer イメージを利用
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
# rootユーザー実行した際に警告が出ないようにする
ENV COMPOSER_ALLOW_SUPERUSER 1
# Composerのホームディレクトリを変更
ENV COMPOSER_HOME /composer
# コマンドが実行できるようにパスを通す
ENV PATH $PATH:/composer/vendor/bin

# Laravelインストーラをダウンロード
RUN composer global require "laravel/installer"

# Node.jsをインストール
RUN apt-get install -y nodejs \
  npm \
  && npm install n -g \
  && n latest