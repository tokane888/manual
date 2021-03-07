# ansible導入+基本

## Ubuntuへの導入

* Ubuntu 20.04では下記でインストール可能なことを確認

```
apt install software-properties-common
apt apt-add-repository --yes --update ppa:ansible/ansible
apt install -y ansible
```

## 導入後共通作業

* ansible運用ユーザー作成
  * "ansible"ユーザー等
* 鍵ペアを生成し、ssh-copy-idで連携
  * ターゲットノードへ配布
    * 管理方法は要検討
  * この鍵の管理方法についても要検討
    * 踏み台サーバへのsshと合わせてgitlabで管理するのが良いと思われる
    * 現地でデプロイ作業を行う場合に対応が必要
* 環境変数にパスを設定
  * ANSIBLE_CONFIG=/etc/ansible/ansible.cfg
  * デフォルトだと下記の優先度で読まれる
    * "ANSIBLE_CONFIG"環境変数のパス
    * カレントディレクトリ(./ansible.cfg)
    * ホームディレクトリ($HOME/ansible.cfg)
    * /etc/ansible/ansible.cfg
* サンプル参考に諸々設定
  * https://github.com/tokane888/ansible-sample

## ansible.cfg

* 主な設定項目
  * forks: ターゲットノードの並列処理を行うプロセス数
  * log_path: ログ出力先
  * host_key_checking: ターゲットノード接続時に、公開鍵のfingerprint確認
  * gathering: ターゲットノード詳細情報取得に関する設定
    * implicit: キャッシュ無視して情報収集
    * explicit: キャッシュを使用し、情報収集を行わない
    * smart: 新規接続時のみ情報収集を行い、キャッシュがある場合は情報収集を行わない
  * gather_subset: ターゲットノードの詳細情報取得を制限
    * all: すべての情報を取得
    * network: 最小限の情報とネットワーク情報
    * hardware: ハードウェア情報
    * virtual: 最小限の情報と、仮想マシン関連情報

## 主なコマンド例

* ping送信
  * ansible -i inventory.ini test_servers -m ping
* ファイル作成
  * ansible -i inventory.ini test_servers -m file -a 'path=$HOME/test.txt state=absent'
    * ディレクトリ作成時は上記をstate=directoryに変更
* ファイル削除
  * ansible -i inventory.ini test_servers -m file -a 'path=$HOME/test.txt state=absent'
* user作成
  * ansible -i inventory.ini test_servers -b -K -m user -a 'user=user01 groups="wheel" append=yes comment="Test User01"'
    * groupは事前に作成が必要
* user削除
  * ansible -i inventory.ini test_servers -b -K -m user -a 'user=user01 state=absent'
* module仕様確認
  * ansible-doc (module名)
    * 例: ansible-doc apt
* オプション
  * -b: sudo権限で実行
  * -K: 認証時に手動でパス入力

## 疑問点

* virtual boxでのテスト方法は？
* 環境をパラメータとして指定するには？
  * group_vars適用
* 標準入出力をログに残す方法
