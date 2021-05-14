# maas

参考) https://maas.io/docs/snap/2.9/ui/installation

## セットアップ

* snapでmaasインストール
  * sudo snap install --channel=2.9/stable maas
* snap未使用の場合、.bashrcへsnap/binへパス通す記載追加
  * export PATH=$PATH:/snap/bin
  * . ~/.bashrc
* OSデプロイ対象PCとLAN接続
  * ルーター側のLANとは別のLANを指す
  * 下記のような構成になる
    * ルーター --- maas --- OSデプロイ対象PC
  * maasのOSデプロイ対象PC側IPはmaasが接続している他のネットワークのIPと重複を避けて設定
    * OSデプロイ対象PCのネットワークinterfaceがアクティブになるまでは実際にmaasPCの当該LANにIPは割り当てられない

### テスト設定でインストール

* snap install maas-test-db
* postgresがインストールできていることをログインして確認
  * maas-test-db.psql
  * \l
* DB初期化コマンド実行
  * sudo maas init region+rack --database-uri maas-test-db:///
    * MAAS URLの入力を求められるが、defaultのままEnter

### production設定でインストール

* postgresをinstall
  * sudo apt update -y
  * sudo apt install -y postgresql
* .bashrcへ下記を一時的に記載して読み込み
  ```
  export MAAS_DBUSER=maas
  export MAAS_DBPASS=password
  export MAAS_DBNAME=maas
  export HOSTNAME=localhost
  ```
  * . ~/.bashrc
* 以降の操作でエラーが出るので、cdで/rootから適当なユーザーのhomeディレクトリへ移動
* polstgresのユーザー作成
  * sudo -u postgres psql -c "CREATE USER \"$MAAS_DBUSER\" WITH ENCRYPTED PASSWORD '$MAAS_DBPASS'"
    * パラメータ
      * MAAS_DBUSER: DB user名
      * MAAS_DBPASS: DB pass
    * 例) sudo -u postgres psql -c "CREATE USER \"maas_admin\" WITH ENCRYPTED PASSWORD 'password'"
* postgresのDB作成
  * sudo -u postgres createdb -O "$MAAS_DBUSER" "$MAAS_DBNAME"
    * パラメータ
      * MAAS_DBUSER: DB user名
      * MAAS_DBNAME: DB名
    * 例) sudo -u postgres createdb -O "maas_admin" "maas"
* postgres設定
  * sudo vim /etc/postgresql/10/main/pg_hba.conf
    * 下記記載
      * host    $MAAS_DBNAME    $MAAS_DBUSER    0/0     md5
* MAAS初期化
  * sudo maas init region+rack --database-uri "postgres://$MAAS_DBUSER:$MAAS_DBPASS@$HOSTNAME/$MAAS_DBNAME"
    * 例) sudo maas init region+rack --database-uri "postgres://maas_admin:password@localhost/maas"

### インストール後の確認

* 状態確認
  * sudo maas status
* 現状の設定確認
  * sudo maas config
* 管理ユーザー作成
  * sudo maas createadmin
    * メールアドレスは適当に指定
    * sshは未入力のままEnter
* API keyが必要な場合下記で取得
  * sudo maas apikey --username yourusername
* ブラウザから設定画面参照する場合は下記へ
  * http://(IP):5240/MAAS/

## ブラウザから設定

* ログイン
  * http://(IP):5240/MAAS/
* 初回ログイン時設定
  * region name: 任意に設定
  * DNS forwarder: 任意に設定。8.8.8.8等
  * Ubuntu archive: パッケージ取得元。デフォでOK
  * Ubuntu extra architectures: intel以外向けのパッケージ取得元。デフォでOK
* ReleasesでデプロイするOSを設定
* Update selection押下し、"queued for download"と表示されるまで待機
* Continue
* Source => upload
* 鍵ペア生成
  * ssh-keygen -t rsa -b 4096 -N "" -m PEM -f id_rsa
* 公開鍵をブラウザに貼り付けてImport
* "Go to dashboard"押下
* image downloadを待機
  * Menu => Images で、pxe bootの際に使用されるimageのdownload完了を待機
  * 他の作業を行っても問題ないと思われるが、downloadが途中で進まなくなるケースがあったため、安全策で待機
    * 完了するまではPXE bootが成功しない
* DHCP有効化
  * Subnets選択
  * maas - OSデプロイ対象PC 間networkの"untagged"選択
  * "Enable DHCP"
  * "Configure DHCP"
    * 割当対象のIP範囲は基本的にデフォルト値でOK

## OSデプロイ対象PC側を設定

* BIOSからPXE boot有効化
* bootの優先順位指定でPXE bootを最上位に設定
* 可能であればIPMI等も有効化し、ネットワークからboot可能にする
  * 用語
    * BMC(Baseboard Management Controller)
      * 監視用マイコンチップ
    * IPMI(Intelligent Platform Management Interface)
      * BMCを用いた監視のプロトコル
  * セキュリティ要件に合わせて検討
  * IPMIは法人向けPC等でないとサポートされていない事も多いとのこと
  * IPMIとBMCは恐らく同様の機能を指す
  * Wake on LANはサポート外なのでonにする必要なし
    * maasは電源のon/off/状態確認 の機能が必要だが、Wake on LANは電源onだけ可能であるため
    * 参考) https://discourse.maas.io/t/no-wol-on-maas/341
* ネットワークアダプタに電源供給を確実に行うため、ErP等省電力設定無効化
* 設定を反映して再起動
  * SSDのOSが読み込まれず、PXEで起動される
  * 起動後はログイン？を促すようなプロンプトが出るが、しばらく放置で消え、PCシャットダウン
  * PCシャットダウン後、maasのMachineタブに当該PCが表示される

## OSデプロイ

* ブラウザメニューから"Machines"画面へ
  * http://(maas IP):5240/MAAS/r/machines
  * machineは、最初にmaasのネットワークに接続した際にMachines画面に登録される
* 当該Machine選択
* 必要に応じて左上のMachine名をクリックして名前変更
* 
* 右上から"Take action" => "Commission" => "Commission machine"
* 

## トラブルシューティング

### CLIセットアップ

* ログインしないとmaasコマンドの最低限の機能以外は使用不可
* API key取得
  * sudo maas apikey --username=admin > api-key-file
* ログイン
  * maas login admin http://(IP):5240/MAAS < api-key-file
    * 例) maas login admin http://localhost:5240/MAAS < api-key-file
    * API keyの手入力が求められるので、コピペして貼り付け

### ケース

* 共通
  * postgres内容確認
    * su - postgres
    * psql -U postgres
    * DB一覧表示
      * \l
    * 指定したDBへ接続
      * \c (DB名)
        * 例) \c maas
    * table一覧表示
      * \d
    * 終了
      * \q
  * ログファイル確認
    * /var/snap/maas/common/log/
      * maas.log, rackd.log等
* Imagesのdownloadが完了しない
  * 参考) https://maas.io/docs/cli-image-management
    * TODO: 下記対応で一定%でdownloadが停止はしなくなったが、download自体されなくなったので調査
  * 手動でdownload開始指示
    * maas admin boot-resources import
      * download進捗が一定の%で停止している場合はこれでは解決しなかった
  * image削除+再download
    * image(boot resource)一覧表示
      * maas (user名) boot-resources read | jq -r '.[] | "\(.id)\t\(.architecture)"'
        * 例) maas admin boot-resources read | jq -r '.[] | "\(.id)\t\(.architecture)"'
    * id指定でdownloadが完了しないboot resource削除
      * maas (user名) boot-resource delete (id)
        * 例) maas admin boot-resource delete 29
    * boot sourceを削除していた場合はデフォルト値復帰
      * maas (user名) boot-sources create url=https://images.maas.io/ephemeral-v3/stable/ keyring_filename=/usr/share/keyrings/ubuntu-cloudimage-keyring.gpg
        * 例) maas admin boot-sources create url=https://images.maas.io/ephemeral-v3/stable/ keyring_filename=/usr/share/keyrings/ubuntu-cloudimage-keyring.gpg
    * boot source一覧読み取り

      ```
      # maas admin boot-sources read
      Success.
      Machine-readable output follows:
      [
          {
              "created": "2021-05-02T11:12:58.620",
              "updated": "2021-05-02T11:12:58.620",
              "url": "http://images.maas.io/ephemeral-v3/stable/",
              "keyring_filename": "/snap/maas/current/usr/share/keyrings/ubuntu-cloudimage-keyring.gpg",
              "keyring_data": "",
              "id": 1,
              "resource_uri": "/MAAS/api/2.0/boot-sources/1/"
          }
      ]
      ```
    * boot source IDを指定して取得可能なresource一覧取得

      ```
      # maas admin boot-source-selections read 1
      Success.
      Machine-readable output follows:
      [
          {
              "os": "ubuntu",
              "release": "focal",
              "arches": [
                  "amd64"
              ],
              "subarches": [
                  "*"
              ],
              "labels": [
                  "*"
              ],
              "id": 1,
              "boot_source_id": 1,
              "resource_uri": "/MAAS/api/2.0/boot-sources/1/selections/1/"
          },
          {
              "os": "ubuntu",
              "release": "bionic",
              "arches": [
                  "amd64"
              ],
              "subarches": [
                  "*"
              ],
              "labels": [
                  "*"
              ],
              "id": 2,
              "boot_source_id": 1,
              "resource_uri": "/MAAS/api/2.0/boot-sources/1/selections/2/"
          }
      ]
      ```

    * 削除したboot resourceを指定して復帰
      * maas (user名) boot-source-selections create (source ID) os="ubuntu" release="bionic" arches="amd64" labels="*"
        * source IDは重複しないように任意に指定
        * os, release, arches, labelsは、resource一覧取得結果のものを使用
        * 例) maas admin boot-source-selections create 1 os="ubuntu" release="bionic" arches="amd64" labels="*"
        * TODO: 既にimageはあることになっている。import指示しても動かず。DBの問題？っぽいので調査
* maas完全リセット
  * sudo snap remove maas-cli --purge
  * sudo snap remove maas --purge
  * sudo snap remove maas-test-db --purge
  * DB削除+DB role削除
    * postgresパッケージのpurgeだけでは消えないため
    * su - postgres
    * psql -U postgres
    * DB一覧確認
      * \l
    * DB削除
      * drop database maas;
    * role一覧確認
      * \du
    * role削除
      * drop role maas;
  * .conf末尾の記載削除
    * sudo vim /etc/postgresql/10/main/pg_hba.conf
  * sudo apt purge -y postgresql
