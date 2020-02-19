## grub2

### 表示方法

* Ubuntuの場合
  * OS起動時にesc押下でgrubメニュー表示
  * もう一度esc押下でgrubコンソール表示
    * grubメニューを表示しようとして、連打でこちらを表示してしまうことが多い

### コマンド

* シャットダウン
  * halt
* OS再起動
  * exit
* OS起動オプション更新
  * ubuntuから更新する場合
    * vim /etc/default/grub
    * 主な設定項目
      * GRUB_DEFAULT=0: 何番目のエントリをデフォルトで起動するか
      * GRUB_TIMEOUT=5: GRUBメニュー画面を何秒表示するか
      * GRUB_CMDLINE_LINUX: 起動オプション指定
      * GRUB_DEFAULT=saved: デフォルトのメニューエントリを前回選択したものにする
      * GRUB_TIMEOUT_STYLE=hidden: 起動時の待ち時間表示形式(hidden/menu/countdown のいずれか)
    * 上記設定ファイル等を元に、実際に読み込まれる設定ファイルを生成
      * grub-mkconfig -o /boot/grub/grub.cfg
        * /etc/grub.d/ 配下の設定ファイルも統合して1つにしているっぽい
        * CentOSだとgrub-mkconfig2？とか言うのになるっぽい