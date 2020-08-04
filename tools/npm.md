# npm

* 参考: https://qiita.com/kouki_tsuji/items/bf4951a76d221e874fdd
* nodeの複数バージョン管理を行うパッケージは以下
  * nvm: ユーザー毎のバージョンを設定
  * n: 全ユーザー共通のバージョンを設定


## 関連パッケージ

* 複数verのnodejsを切り替えて使用可能に
  * npm: ユーザーごとの設定
  * n: 全体の設定

## 導入順序

### Ubuntuの場合

* apt install -y npm
  * npmインストールによってnodejsは自動でインストールされる
  * 下記でnpm, nodejsのバージョン確認可能
    * npm -v
    * nodejs -v
* npm install n -g
  * 下記でnのバージョン確認可能
    * n --version
* nで、nodejsの安定版をインストール
  * n stable
  * aptとnのnodejsをぞれぞれ実行するコマンド
    * apt: nodejs
    * n: node
  * 下記でnpm, nodejsのバージョン確認可能
    * npm -v
      * apt install直後とはnpmのパスが変化している
    * nodejs -v
* TODO: apt purge するべきか検討。実際実行したところnpmコマンドが/usr/bin/npm: file not found に。。
  * apt purge -y npm nodejs
    * nで導入した方と競合するため、aptで導入した方は削除

## uninstall手順

* n uninstall
* rm -rf /usr/local/n
* npm uninstall -g n