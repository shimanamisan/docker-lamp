# docker-lamp for Laravel

# インストール手順

- 作業はこのプロジェクトのルートディレクトリで行います

## jwilder/nginx-proxy を導入

[jwilder/nginx-proxy](https://hub.docker.com/r/jwilder/nginx-proxy/)という、簡単に nginx で（動的に）リバースプロキシを設定し、任意のサブドメイン等からコンテナにアクセス可能にさせてくれる Docker イメージを導入しました。

これによって、`localhost:80xx`の用にポート番号を指定しなくても良くなりました。

サンプルのプロジェクトでは、`test.example.com`と設定してあるのでこのドメインでポート番号を指定することなくアクセス可能です。

## 新規で Laravel をインストールする場合

- 以下の作業を行い`test.example.com`でブラウザからアクセスして Laravel8 のトップ画面が出てきたら正しくインストールできています

```bash
# コンテナを起動する
docker-compose up -d

# Webサーバのコンテナへシェルで入る
docker-compose exec web bash

# Laravelをインストールする
laravel new sample_laravel # sample_laravelの名前は任意で指定して下さい

# プロジェクトディレクトリへ移動
cd sample_laravel

# Webサーバの書き込み権限を与える
chmod -R 777 storage
chmod -R 777 bootstrap/cache
```

- Larvel のプロジェクト名を任意の名前にするときは、`000-default.conf`の内容を修正する必要があります
- ファイルの存在場所は、コンテナ内の`/etc/apache2/sites-enabled/000-default.conf`に存在しています

```ini
<VirtualHost *:80>

  ServerAdmin webmaster@example.com

  DocumentRoot /var/www/html/[任意のプロジェクト名]/public
  ServerName test.example.com
  DirectoryIndex index.php index.html

  <Directory /var/www/vhosts/[任意のプロジェクト名]/public>
    AllowOverride All
    Require all granted
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

## 起動時に作成されるデータベースを任意の名前にする場合

- `docker/db/init/ini.sql`を以下のように修正します

```sql
CREATE DATABASE IF NOT EXISTS [作成したいデータベース名]
```

## .env ファイルの設定

- データベースへ接続するための設定を行います

```ini
DB_CONNECTION=mysql
# container_name: container_db を指定
DB_HOST=container_db
DB_PORT=3306
# 初回起動時に作成するDB名を指定
DB_DATABASE=sample_laravel
DB_USERNAME=root
# docker-compose.ymlで指定しているrootユーザーのパスワードを指定
DB_PASSWORD=root
```

- Web サーバへ接続してマイグレーションを実行します

```bash
docker-compose exec web bash

cd sample_laravel

php artisan migrate
```

- 正しく実行されたか確認します

```bash
mysql -h db-host -p
Enter password: [rootユーザーのパスワード]

show databases;
+--------------------+
| Database           |
+--------------------+
| docker-database    |
| information_schema |
| mysql              |
| performance_schema |
| sample_laravel     |
| sys                |
+--------------------+

use sample_laravel;

show tables;
+--------------------------+
| Tables_in_sample_laravel |
+--------------------------+
| failed_jobs              |
| migrations               |
| password_resets          |
| personal_access_tokens   |
| users                    |
+--------------------------+

# DBから抜けます
exit;

# コンテナから抜けます
exit
```

## mailhog の動作確認をする場合

-

## すでに作成済みの Laravel プロジェクトを GitHub からクローンする場合

- htdocs ディレクトリ配下へ`git clone`を実行して下さい
- その他、`000-default.conf`や`docker/db/init/ini.sql`を任意の設定値へ修正してコンテナを起動して下さい
