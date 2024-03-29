# apt

## トラブルシューティング

* apt update失敗
  * 下記のようなエラーの場合
    ```
      E: Failed to fetch store:/var/lib/apt/lists/partial/jp.archive.ubuntu.com_ubuntu_dists_bionic-backports_Contents-amd64.gz  Hash Sum mismatch
    Hashes of expected file:
      - Filesize:812322 [weak]
      - SHA256:2ce4eb67e93c4c16b674f0ddd7307714b9b417ffde41148d52a162d1238f955a
      - SHA1:fbebe708303e6dc5b1d2551cb2a7186450d6f03a [weak]
      - MD5Sum:a7f67cb1dbc7b0baaeebf71f59013f6b [weak]
    Hashes of received file:
      - SHA256:39e7821cd5f14717fdceadbc80e26937647aee74a54bd3cff4863fd75428b5c4
      - SHA1:39b1743765625fd7d9680f93846dc4063bd68908 [weak]
      - MD5Sum:2e2b63f90233194bd655c367d37e816a [weak]
      - Filesize:97810 [weak]
    ```
    * rm -rf /var/lib/apt/lists
      * aptは/var/lib/apt/lists配下にdeb形式のcacheを格納しており、これがエラー原因となっている可能性がある
      * rm実行後に再度apt update実行すると成功する場合がある

### 依存関係問題

* 依存関係に問題が生じていないか確認
  * apt-get check
    * 問題出力例) xserver-xorg : Depends: xserver-xorg-core (>= 2:1.17.2-2)
      * 多くの場合、左右どちらかのパッケージが古いので、下記のようにdownloadしてインストール
        ```
        apt download xserver-xorg
        dpkg -i xserver-xorg-[バージョン番号].deb
        ```
        * 依存関係に問題が生じているときはapt installは基本的に失敗する
      * 依存関係に問題が生じている状態でパッケージを削除する場合は下記
        * dpkg --remote [パッケージ名]
* パッケージの直接の依存先確認
  * apt depends [パッケージ名]
* パッケージの依存先を再帰的に確認
  * apt-rdepends [パッケージ名]
* パッケージの依存先を再帰的に確認してdownload
  * apt download $(apt-rdepends [パッケージ名])
    * 仮想パッケージ等は上記でインストールできない場合がある
  