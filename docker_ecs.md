# MacでECSを利用してWordPressを公開する流れ
WordPress を Docker で管理する場合、 
* WordPress本体用のコンテナ
* データベース用のコンテナ
を起動する -> docker-compose.yml を使ってコンテナを作成・起動する  
Amazon ECS で docker-compose.yml を実行しようと思うと、専用の Amazon ECS CLI というソフトが必要になる
## MacでAmazon ECSにdocker-compose.ymlするために必要なソフト群
* Amazon ECS CLI
* GnuPG
* Python 2.6.5以上 or Python 3.3以上
* Amazon AWS CLI

## MacにAmazon ECS CLIをインストール
`sudo curl -o /usr/local/bin/ecs-cli https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-darwin-amd64-latest`

`curl -s https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-darwin-amd64-latest.md5 && md5 -q /usr/local/bin/ecs-cli`

### MacにGPG(GnuPG)インストール
GPGとはOpenPGP から派生した、企業が使用できるもう1つの無料の暗号化規格

`brew install gnupg`

```
Yilin:docker zhouyilin$ gpg --keyserver hkp://keys.gnupg.net --recv BCE9D9A42D51784F
gpg: /Users/zhouyilin/.gnupg/trustdb.gpg: 信用データベースができました
gpg: 鍵BCE9D9A42D51784F: 公開鍵"Amazon ECS <ecs-security@amazon.com>"をインポートしました
gpg:           処理数の合計: 1
gpg:             インポート: 1
```
鍵を設定していない場合
```
Yilin:docker zhouyilin$ gpg --verify ecs-cli.asc /usr/local/bin/ecs-cli
gpg: ディレクトリ'/Users/zhouyilin/.gnupg'が作成されました
gpg: keybox'/Users/zhouyilin/.gnupg/pubring.kbx'が作成されました
gpg: 水  7/ 8 07:29:27 2020 JSTに施された署名
gpg:                RSA鍵DE3CBD61ADAF8B8Eを使用
gpg: 署名を検査できません: No public key
```

```
Yilin:docker zhouyilin$ curl -o ecs-cli.asc https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-darwin-amd64-latest.asc
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   821  100   821    0     0    843      0 --:--:-- --:--:-- --:--:--   843
```
鍵を設定して、`gpg --verify ecs-cli.asc /usr/local/bin/ecs-cli`
```
Yilin:docker zhouyilin$ gpg --verify ecs-cli.asc /usr/local/bin/ecs-cli
gpg: 水  7/ 8 07:29:27 2020 JSTに施された署名
gpg:                RSA鍵DE3CBD61ADAF8B8Eを使用
gpg: "Amazon ECS <ecs-security@amazon.com>"からの正しい署名 [不明の]
gpg: *警告*: この鍵は信用できる署名で証明されていません!
gpg:       この署名が所有者のものかどうかの検証手段がありません。
 主鍵フィンガープリント: F34C 3DDA E729 26B0 79BE  AEC6 BCE9 D9A4 2D51 784F
 副鍵フィンガープリント: EB3D F841 E2C9 212A 2BD4  2232 DE3C BD61 ADAF 8B8E
```

`sudo chmod +x /usr/local/bin/ecs-cli`  
上記コマンドで ECS CLI にアクセス権限を付与 -> ecs-cli コマンドが使える

`ecs-cli --version` -> ECS CLI のバージョンが返ってくる

これで Mac への Amazon ECS CLI のインストールは完了 -> Amazon AWS CLI の設定

## MacにAmazon AWS CLIをインストール
AWS CLI は、 AWS の操作をブラウザではなく、Macのパソコンから実行できるようにしてくれるソフト  
[チュートリアル: Amazon ECS CLI を使用して EC2 タスクのクラスターを作成する](https://docs.aws.amazon.com/ja_jp/AmazonECS/latest/developerguide/ecs-cli-tutorial-ec2.html)

`curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"`
今回は AWS インストーラーを利用して AWS CLI をインストールしていく。上記コマンドを実行すると AWS CLI 本体がダウンロード。現在の作業ディレクトリに .zip ファイルが登場する。

ダウンロードが終わったら `unzip awscli-bundle.zip` -> .zip ファイルを展開（解凍）  
`sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws`

`aws --version` -> バージョン確認

### dockerからインストール
`docker run --rm -it amazon/aws-cli:latest`

[【AWS&Docker初心者向け】WordPressをAWSのDockerで公開　~Mac編~](https://blog.codecamp.jp/wordpress-aws-docker-mac)