# gitlab環境構築

* raspbian OS(buster)上で検証を行った
  * raspberry Pi4(メモリ8GB)

## 事前準備

* メールでのイベント通知にSMTPサーバを使用する
  * 事前にSMTPサーバから必要なドメインへメールを送信できるように設定を済ませる

## docker上で動かす場合

* 小規模環境及びテスト用
* 下記でvolume用のディレクトリ作成

```
mkdir -p /srv/gitlab/config
mkdir -p /srv/gitlab/logs
mkdir -p /srv/gitlab/data
```

* host側でvolume用の環境変数設定
  * GITLAB_HOME=/srv/gitlab
* 下記実行

```
sudo docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  gitlab/gitlab-ce:13.11.1-ce.0
```

