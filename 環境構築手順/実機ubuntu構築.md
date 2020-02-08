## 実機へubuntuインストール

* [download image](https://ubuntu.com/download)
* [bootable usb作成](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows)
  * live imageとしての使用も、ubuntuインストールも可能
* 実機起動しつつ、F12, F2, ESC辺りを押してBIOS起動
  * Boot順でUSB最優先に
* 再起動
* [インストール](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop)
  * 途中選択肢
    * 言語 => 英語
      * 日本語だとディレクトリ名"ダウンロード"のフォルダとかができてしまうので
      * 日本語設定にすると、日本語フォントの表示は可能だが、半角/全角キーは使用不可
    * "Minimal installation"
      * 余計なパッケージインストールしないため
        * thunderbird, libreofficeは入る。後で手動削除
    * OSインストール完了後、OS再起動
      * **再起動時に、必ずUSBを抜く**
        * live OS上で作業してしまう可能性があるため
          * live OSで無い場合、USBを刺したままだとデスクトップにアイコンが表示される
            * df -hなどでも確認可能
            * live OSだとアマゾンとかのアイコンが左メニューに出る
* [dotfiles](https://github.com/tokane888/dotfiles)実行で前述の余計なファイルはほぼ削除される
  * guiが起動しなくなる場合があるので、手動でのパッケージ削除時は慎重に行う
    * NG例）apt purge -y thunderbird* libre*
      * 上記実行でGUI死亡