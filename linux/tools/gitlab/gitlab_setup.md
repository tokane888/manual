# gitlab環境構築

* raspbian OS(buster)上で検証を行った
* raspberry Pi4(メモリ8GB)
* 参考) https://about.gitlab.com/install/#raspberry-pi-os

## 事前準備

* メールでのイベント通知にSMTPサーバを使用する
* 事前にSMTPサーバから必要なドメインへメールを送信できるように設定を済ませる

## docker上に環境構築する場合

* 小規模環境及びテスト用
* Docker for Windowsはサポート外
* https://docs.gitlab.com/ee/install/docker.html
* 下記でvolume用のディレクトリ作成

```
mkdir -p /srv/gitlab/config
mkdir -p /srv/gitlab/logs
mkdir -p /srv/gitlab/data
```

* host側でvolume用の環境変数設定
* GITLAB_HOME=/srv/gitlab
* 下記実行
* 多少時間がかかる

```
sudo docker run --detach \
--hostname localhost \
--publish 443:443 --publish 80:80 --publish 22:22 \
--name gitlab \
--restart always \
--volume $GITLAB_HOME/config:/etc/gitlab \
--volume $GITLAB_HOME/logs:/var/log/gitlab \
--volume $GITLAB_HOME/data:/var/opt/gitlab \
gitlab/gitlab-ce:13.11.1-ce.0
```

* 上記実行後、ブラウザからlocalhostにアクセス可能になる
* アクセス可能にならない場合、下記実行
* コンテナログ確認
  * docker logs gitlab
* WSL2上でdocker実行しており、コンテナ外のブラウザからアクセスする場合
  * WSL2上でip addrを実行し、出力されたipを指定してブラウザから遷移
* 管理画面初回表示時に"Register Now"を押下し、adminパス登録
* パス登録時に承認依頼メールが飛ぶようだが、WSL2上の場合は正常に飛ばない
* /etc/gitlab/gitlab.rb

## ubuntu 20.04上に環境構築する場合

* 基本的に公式手順に従う
* 参考) https://about.gitlab.com/install/#ubuntu
* 依存パッケージインストール
  ```
  sudo apt-get update
  sudo apt-get install -y curl openssh-server ca-certificates tzdata perl
  ```
* 通知用のpostfixインストール
  * sudo apt-get install -y postfix
  * インストール中にダイアログが出るので、"Internet Site"を選択
  * ホスト名はgitlabの配置するサーバと同一のドメインを設定しておけば良い
    * 違いが会っても一応大丈夫
* gitlab package repositoryを設定
  * curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
* gitlabインストール
  * 下記コマンドのドメイン部分は実際にgitlabをデプロイする先のドメインを指定
    * なおhttpsを指定すると、自動でLet's Enctyptの証明書を取得してくる
    * sudo EXTERNAL_URL="http://gitlab.example.com" apt-get install gitlab-ee
  * 下記の警告が出た場合、後述の手順に従ってpostgres更新
    ```
    === INFO ===
    You are currently running PostgreSQL 12.
    Note that PostgreSQL 13 will become the minimum required PostgreSQL version in GitLab 14.0 (May 2021).
    PostgreSQL 12 will be removed in GitLab 14.0. Please consider upgrading your PostgreSQL version soon.
    To upgrade, please see: https://docs.gitlab.com/omnibus/settings/database.html#upgrade-packaged-postgresql-server
    ```
    * 注意) 下記の公式を見るとgitlab 14.0はpostgres 11をサポートしなくなるだけで問題ない
      * https://docs.gitlab.com/omnibus/settings/database.html#gitlab-140-and-later
    * 上記の注意確認の上、必要そうならpostgres update(gitlab 14のときは下記を実行しても何もupdateされなかった)
      * 下記でエラーが出ないこと確認
        * gitlab-ctl reconfigure
      * DBを2つコピーしても問題無い程度にディスク容量に余裕があること確認
      * 下記でpostgres更新
        * gitlab-ctl pg-upgrade
* 自動生成されたデフォルトパスワード確認
  * 24時間で消えるとのことなので注意
  * cat /etc/gitlab/initial_root_password
* tutorialに従って諸々初期設定
  * https://about.gitlab.com/install/#ubuntu
* おすすめ関連設定資料確認
  * https://docs.gitlab.com/omnibus/index.html

## トラブルシューティング

* gitlabのversion確認
  * gitlab-rake gitlab:env:info
    * 何故か15秒程度待ち時間があるが確認可能
