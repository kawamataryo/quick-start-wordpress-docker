# Quick-start-wordpress-docker
DockerによるWordpressのローカル環境構築を行うためのリポジトリ

# Features
* Dockerによるwordpressのローカル環境構築
* wordmoveへの本番環境デプロイ, バックアップ

# How to start

### 0. 事前準備
Macにdocker, docker-composeをインストール  
以下参考: 
 [Docker Compose - インストール - Qiita](https://qiita.com/zembutsu/items/dd2209a663cae37dfa81)

### 1. 環境変数の設定
このリポジトリをクローン

```
$ git clone https://github.com/kawamataryo/quick-start-wordpress-docker.git project-dir
```

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

参考：  
docker-compose downを行うと、DBのデータも初期化されてしまうので、注意。  
もし永続化したい場合は以下を参考に設定を追加する。  
[DockerでMySQLのデータを保存する方法
](https://qiita.com/TakashiOshikawa/items/11316ffd2146b36b0d7d)

```
$ docker-compose stop
```
# How to use wordmove ?
wordmoveを使えば容易に本番環境とローカル環境の同期が可能です。

### 1. wordmoveのコンテナに接続
wordmoveのコンテナが起動した状態で以下コマンドを実行します。

```
$ docker exec -w /home/ -it wordmoveのコンテナ名 /bin/bash
```

### 2. sshの設定
コンテナから本番環境に同期するためssh-agentの設定を行います。  
接続先サーバーとのssh接続設定、ローカルのssh/configの設定は終わっていることを前提としてます。  
またid_rsa(秘密鍵)までのパスは接続するサーバーに合わせて記載してください。  
参考:  
[エックスサーバーにSSHで接続してみよう！ | vdeep](http://vdeep.net/xserver-ssh)  
[ssh-agentを利用して、安全にSSH認証を行う - Qiita](https://qiita.com/naoki_mochizuki/items/93ee2643a4c6ab0a20f5)

```
# ssh-agentの起動
$ ssh-agent bash

# ssh-agentの登録。
$ ssh-add /home/.ssh/id_rsa

# wordmoveのmysql dumpで使うrubyのエンコーディング
export RUBYOPT=-EUTF-8
```

### 3. 同期・デプロイ
あとはコマンド一発で同期が可能です。  
本番のデータをローカルにバックアップしたい場合は、、  

```
$ wordmove pull --all
```

ローカルのデータを本番にアップロードしたい場合は、、

```
$ wordmove push --all
```

さらにオプション部分の指定で、DBのみthemesのみなど同期内容を変更可能です。  
参考：  
[Wordmoveの基本操作 - Qiita](https://qiita.com/mrymmh/items/c644934cac386d95b7df)


# TODO
* wordmoveで毎回ssh-addの設定を行う手間を削減（docker-composeのcommandで行う？）
* MySQLのデータ永続化の設定追加
