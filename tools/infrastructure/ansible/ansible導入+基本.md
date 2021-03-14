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
* sshでパスを使用してログインする場合、下記インストール
  * sudo apt install -y sshpass

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

## role

* playbookから呼ばれるモジュールのようなもの
* ミドルウェア単位で作成するのが良い


## テスト

* 基本的に最初に手動テストを一度行えば、その後はテスト不要
  * debパッケージのインストール等は、ansibleが毎回実行結果確認する
* スクリプト実行結果などはツールで確認が必要
  * serverspec: ruby製のテストツール

## 基本方針

* role
  * ミドルウェア1つ導入など、再利用可能なレベルで分ける
  * なるべくtagを打つ
  * include_tasksで、役割ごとにファイル分割
    * 例
      * check_install.yml
      * configure.yml
      * install.yml
      * main.yml
* 権限
  * 基本的に特権必要になるので、プレイ全体でbecome: true

## インベントリ管理

* スタティックインベントリ
  * .ini, .yml形式
* ダイナミックインベントリ
  * ターゲットノードを管理するクラウドや、特定のプロダクトが提供するAPIから、機器情報を動的に取得するインベントリ
  * script形式のものもある

## 高速化

* ファクトキャッシュ有効化
  * ansible.cfgに下記記載で、メモリ上にキャッシュ保存
    * ANSIBLE_CACHE_PLUGIN = memory

## デバッグ

* ansible-playbookのオプション
  * -v: 詳細ログ出力
  * -C: インストールなどの変更は行わず、条件式、シンタックスの確認を行う
* ターゲットノードの配置した実行ファイル保存
  * ANSIBLE_KEEP_REMOTE_FILES = 1
    * ターゲットノードの実行ユーザーの下記パスに実行スクリプトが削除されずに残る
      * $HOME/.ansible/tmp/

## 関連ツール

* AWX Project
  * プレイブック実行を管理するwebベースのインターフェース
  * OSS
* Ansible Tower
  * AWX Projectの特定バージョンをRedHatがサポートする商用製品
  