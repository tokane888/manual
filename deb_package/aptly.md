## aptly

### 各種コマンド

* repository一覧表示
  * aptly repo list

### s3へupload

* 設定記載
  * ~/.aptly.conf
    ```
    ...
    "S3PublishEndpoints": {
      "test": {
        "region": "ap-northeast-1",
        "bucket": "aptly-test-okane8",
        "awsAccessKeyID": "",
        "awsSecretAccessKey": "",
        "acl": "public-read"
      }
    },
    ...
    ```
* リポジトリからsnapshot作成
  * aptly snapshot create (snapshot名) from repo (リポジトリ名)
* snapshotをs3へpublish
  * aptly publish (snapshot名) -distribution=xenial (リポジトリ名)

### mirror作成

* Ubuntuのdebパッケージ日本向け公式リポジトリのmirrorを作成する例
  * 公式リポジトリは以下(sources.listより)
    * http://ap-northeast-1.ec2.archive.ubuntu.com/ubuntu/
* ubuntuリポジトリの公開鍵をgpgの信頼済み鍵に登録
  * sudo gpg --no-default-keyring --keyring /usr/share/keyrings/ubuntu-archive-keyring.gpg --export | sudo gpg --no-default-keyring --keyring trustedkeys.gpg --import
* mirror作成
  * aptly mirror create (aptlyがrepo参照する際に使用する任意の名前) (mirror元repo) (distribution名)
    * ex) aptly mirror create ubuntu_test http://ap-northeast-1.ec2.archive.ubuntu.com/ubuntu/ bionic main
  * ダウンロード
    * aptly mirror update ubuntu_test
      * 何故かarm64をダウンロードしようとしはじめて失敗
        * TODO: あとで原因調査