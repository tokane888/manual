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