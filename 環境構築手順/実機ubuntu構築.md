## 実機へubuntuインストール

* [download image](https://ubuntu.com/download)
  * ubuntu server, ubuntu desktopの違いに注意
  * 古いimageのdownloadは下記から可能
    * http://old-releases.ubuntu.com/releases/18.04.3/
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
        * thunderbird, libreofficeは入る。削除スクリプトは後述
    * OSインストール完了後、OS再起動
      * **再起動後に、USBを抜いてもう一度再起動**
        * live OS上で作業してしまうため
          * live OSで無い場合、USBを刺したままだとデスクトップにアイコンが表示される
            * df -hなどでも確認可能
            * live OSだとアマゾンとかのアイコンが左メニューに出る
* [ubuntu実機用dotfiles](https://github.com/tokane888/dotfiles_ubuntu)実行で前述の余計なファイルは削除される
  * 手動でのパッケージ削除時は慎重に
    * GUIが起動しなくなる場合がある
      * NG例）apt purge -y thunderbird* libre*
    * 左メニューの下記はaptから削除せず、右クリックでお気に入りから削除に留める
      * ubuntu software center => ファイルのアイコン右クリックで"詳細を表示"する機能が使えなくなる
      * help => 他の複数のソフトがこれに依存していそうなので
* [IP固定](../linux/ubuntu/固定IP.md)
* [sshでroot login可能に設定](../ssh_prepare.md)
* [MyDNS登録](../linux/web_tools/mydns/MyDNS.md)
* 日本語入力
  * OSインストール時に日本語設定を選択していても、実機のキーボードの全角/半角キーは無効になっている
  * winキー+space で全角/半角キー有効化