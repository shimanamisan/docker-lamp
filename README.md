# 【WSL2】Docker-lamp for Laravel

この開発環境は WSL2 上で動かすことを前提としています。

## 開発環境

- Windows11
- Docker: v20.10.17, build 100c701
- Docker Compose: v2.6.0

# インストール手順

作業は WSL2 上の適当作業ディレクトリで行ってください。

# 新規で Laravel をインストールする場合

以下の作業を行い`test.example.com`でブラウザからアクセスして Laravel のトップ画面が出てきたら正しくインストールできています

```bash
$ git clone git@github.com:shimanamisan/docker-lamp.git

$ cd docker-lamp

# コンテナを起動するビルド
$ docker compose build

# コンテナを起動する
$ docker compose up -d

# Laravelのプロジェクトを作成
$ docker compose exec web laravel new sample_laravel

# バージョンの確認
$ docker compose exec web bash -c "cd sample_laravel && php artisan --version"

# Webサーバへの書き込み権限を与える
$ docker compose exec web bash -c "cd sample_laravel && chmod -R 777 storage"
$ docker compose exec web bash -c "cd sample_laravel && chmod -R 777 bootstrap/cache"
```

# .env ファイルの設定

データベースの接続設定は以下の用に設定します。

.envファイルを編集するにはローカルからでは権限が無いため、[Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)を導入してコンテナに接続する必要があります。

```ini
DB_CONNECTION=mysql
# container_name: db-container を指定
DB_HOST=db-container
DB_PORT=3306
# 初回起動時に作成するDB名を指定
DB_DATABASE=sample_laravel
DB_USERNAME=root
DB_PASSWORD=rootpass
```

# 参考記事

以下の記事で詳細な手順を記載していますので参考にしてください。

### 【WSL2】DockerでLaravel開発環境を構築する -Part1-
[![【WSL2】DockerでLaravel開発環境を構築する -Part1-](https://user-images.githubusercontent.com/49751604/197087300-d7246b01-7a7d-4085-a787-b6e867df4bef.jpeg)](https://blog.hn-pgtech.com/2021-10-02/)

### 【WSL2】DockerでLaravel開発環境を構築する -Part2-
[![【WSL2】DockerでLaravel開発環境を構築する -Part2-](https://user-images.githubusercontent.com/49751604/197087300-d7246b01-7a7d-4085-a787-b6e867df4bef.jpeg)](https://blog.hn-pgtech.com/2021-10-09/)