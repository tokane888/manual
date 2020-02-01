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
    * "Minimal installation"
      * 余計なパッケージインストールしないため
        * 何故か言語で英語選択するとamazonリンクとか余計なのが作成されるが後述の方法で削除
          * AmazonリンクとかThunderbirdとか不要なもの大量に突っ込まれる
            * apt list --installed | wc -l で1797行出力された
    * OSインストール完了後、OS再起動
      * **再起動時に、必ずUSBを抜く**
        * live OS上で作業してしまう可能性があるため
          * live OSで無い場合、USBを刺したままだとデスクトップにアイコンが表示される
            * df -hなどでも確認可能
            * live OSだとアマゾンとかのアイコンが左メニューに必ず出るっぽい
* [dotfiles](https://github.com/tokane888/dotfiles)実行で前述の余計なファイルはほぼ削除される