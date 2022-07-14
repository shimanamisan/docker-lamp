# 【WSL2】Docker-lamp for Laravel

この開発環境は WSL2 上で動かすことを前提としています。

# インストール手順

作業は WSL2 上の適当作業ディレクトリで行ってください。

# 新規で Laravel をインストールする場合

以下の作業を行い`test.example.com`でブラウザからアクセスして Laravel のトップ画面が出てきたら正しくインストールできています

```bash
$ git clone git@github.com:shimanamisan/docker-lamp.git

$ cd docker-lamp

# コンテナを起動するビルド
$ docker-compose build

# コンテナを起動する
$ docker-compose up -d

# Webサーバのコンテナへシェルで入る
$ docker-compose exec web bash

# Laravelをインストールする
$ laravel new sample_laravel # sample_laravelの名前は任意で指定して下さい

# プロジェクトディレクトリへ移動
$ cd sample_laravel

# Webサーバの書き込み権限を与える
$ chmod -R 777 storage
$ chmod -R 777 bootstrap/cache
```

# .env ファイルの設定

データベースの接続設定は以下の用に設定します。

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

# 参考記事

以下の記事で詳細な手順を記載していますので参考にしてください。

<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://testblog.hn-pgtech.com/2021-10-02/" data-iframely-url="//iframely.net/Z1ekjMU?card=small"></a></div></div><script async src="//iframely.net/embed.js" charset="utf-8"></script>
