# docker
* イメージの取得(pull)
* イメージの一覧表示(images)
* コンテナのライフサイクル管理
* 作成(create)
* 起動(start)
* 停止(stop)
* 削除(rm)
* コンテナの一覧表示(ps)

## dockerとは
```
docker -> プレイステーション
image = class -> ゲームソフト
コンテナ = instance -> セーブデータ
```

### Docker イメージの取得
[Docker Hub](https://hub.docker.com/) から最新の Docker イメージを取得

`docker pull nginx:latest`

取得済みのイメージは docker images で確認可能

`docker images`

### コンテナの作成と実行
`docker run` でコンテナを作成して実行

`docker run --name my-nginx -d -p 8080:80 nginx:latest`

* my-nginx -> コンテナ名(任意)
* 8080 -> ポート番号

http://localhost:8080 でアクセス可能

### コンテナの停止
`docker stop my-nginx`
my-nginx -> コンテナ名

### 確認
`docker ps`

`-a` オプションを付けると終了したコンテナを確認することができる。

`docker ps -a`

### コンテナの再実行
`docker start my-nginx`

`docker ps`で確認

### コンテナの削除
`docker rm my-nginx`

my-nginx -> 自分で設定したコンテナ名

確認 -> `docker ps -a`

### Docker イメージの削除
`docker images`　イメージを確認

`docker rmi nginx:latest`

[nginx](https://registry.hub.docker.com/_/nginx/)
[docker コマンド 基本のキ（nginx のコンテナを実行してみる）](https://qiita.com/thirota/items/dcfc43cb042448a8f8aa)