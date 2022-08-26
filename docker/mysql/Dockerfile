FROM mysql:latest

# コンテナ内の環境変数を指定。詳細はDockerHubを確認
ENV TZ=Asia/Tokyo \
  MYSQL_DATABASE=docker-database \
  MYSQL_USER=docker \
  MYSQL_PASSWORD=docker \
  MYSQL_ROOT_PASSWORD=rootpass

# 設定ファイルをコンテナにコピー
COPY ./my.cnf /etc/mysql/conf.d/my.cnf
COPY ./init/* /docker-entrypoint-initdb.d/