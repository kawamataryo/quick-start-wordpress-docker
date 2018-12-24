# What is This ?
DockerによるWordpressのローカル環境構築を行うためのリポジトリです。

# Features
* Dockerによるwordpressのローカル環境構築
* wordmoveへの本番環境デプロイ, バックアップ

# How to use ?
### 1. 環境変数の設定
.envにdocker-composeで使う環境変数を定義しています。
コメントを元に記載してください。

```
vi ./.env
```

```.env
# -------------------------------------------
# wordpress・mysqlコンテナの設定
# -------------------------------------------
# プロダクトの名前 作成されるcontainer名の接頭語として使用
PRODUCTION_NAME=
# local wordpressを紐付けるPort名（ex: 8080)
LOCAL_SERVER_PORT=
# localのmysqlを紐付けるPort名（ex: 3306)
LOCAL_DB_PORT=

# -------------------------------------------
# wordmoveコンテナの設定
# -------------------------------------------
# 全て本番環境の情報
# URL
PRODUCTION_URL=
# wordpressのディレクトリの絶対パス
PRODUCTION_DIR_PATH=
# DB名
PRODUCTION_DB_NAME=
# DBのユーザー名
PRODUCTION_DB_USER=
# DBのパスワード
PRODUCTION_DB_PASSWORD=
# DBのホスト名
PRODUCTION_DB_HOST=
# DBの接続ポート
PRODUCTION_DB_PORT=
# SSHホスト名
PRODUCTION_SSH_HOST=
# SSHユーザー名
PRODUCTION_SSH_USER=
# SSHポート名
PRODUCTION_SSH_PORT=
```

### 2. Dockerコンテナの起動
docker-composeで関連コンテナを起動します。

```
$ docker-compose up -d
```

### 3. Wordpressの初期化
.envのLOCAL_SERVER_PORTに設定したホストにアクセスし、worpdressの初期化を行います

```
$ open http://localhost:8080
```

### 4. Dockerコンテナの停止
docker-composeで関連コンテナを停止します。
※ docker-compose downを行うと、DBのデータも初期化されてしまうので、注意。もし永続化したい場合は以下を参考に設定を追加する。[DockerでMySQLのデータを保存する方法
](https://qiita.com/TakashiOshikawa/items/11316ffd2146b36b0d7d)

```
$ docker-compose stop
```
