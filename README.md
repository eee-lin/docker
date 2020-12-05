# docker
## docker-compose
### composeとwordpress
docker-compose.ymlファイルを生成 -> このファイルが WordPress ブログを起動する。
データ保存のためにボリュームマウントを使った MySQL インスタンスも生成する。
```docker-compose.yml
version: '3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
    db_data:
```

`docker-compose up -d`

デタッチモードにより docker-compose up を実行し、不足する Docker イメージがあれば取得する。

WordPress と データベースの両コンテナを起動する。

`docker-compose down`

コンテナとデフォルトネットワーク削除、WordPress データベース残る。

`docker-compose down --volumes`

コンテナとデフォルトネットワーク、WordPress データベース削除

[参考サイト](http://docs.docker.jp/compose/wordpress.html)