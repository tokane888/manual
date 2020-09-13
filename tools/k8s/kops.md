# kop

* kopはプロダクションレベルの、クラスタ用のkubectlのようなもの

## Ubuntuへの導入手順

* 参考) https://kops.sigs.k8s.io/getting_started/install/
```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
```
* awscliインストール
  * apt install -y awscli
* awsでkops用のユーザー作成
  * https://console.aws.amazon.com/iam/home?region=ap-northeast-1#/users
  * "ユーザーを追加"から下記ユーザー作成
    * ユーザー名: kops
    * "プログラムによるアクセス": 有効
    * アクセス許可の設定
      * "既存のポリシーを直接アタッチ"
      * "AdministratorAccess"
        * 一番簡単なので。実運用時は都度検討
    * タグの追加は変更なしで次へ
* aws s3でbucket作成
  * https://s3.console.aws.amazon.com/s3/home?region=ap-northeast-1#
* aws route 53でkopsのドメイン作成？
  * 自分が保持しているドメインのサブドメインをroute53へforwardされるようにする
  * kopsはdns立ててくれる？が、route 53でzone作成が必要とかなんとか
  * https://console.aws.amazon.com/route53/v2/home#Dashboard
    * DNS管理の"ホストゾーンの作成"押下
    * ドメイン名に自分が保持しているドメインのサブドメイン記載
    * 他は変更無しで"ホストゾーンの作成"
      * kubernetes.joyhome.mydns.jp
