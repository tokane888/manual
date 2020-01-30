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
* [dotfiles](https://github.com/tokane888/dotfiles)実行で前述の余計なファイルはほぼ削除される