## 実機へubuntuインストール

* [download image](https://ubuntu.com/download)
  * ubuntu server, ubuntu desktopの違いに注意
  * 古いimageのdownloadは下記から可能
    * http://old-releases.ubuntu.com/releases/18.04.3/
* bootable usb作成
  * windows上で作成する場合
    * https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows
  * ubuntu上で作成する場合
    * dd if=ubuntu-22.04.2-desktop-amd64.iso of=/dev/sda1 conv=fdatasync
      * data dumpコマンド
      * if: input file, of: output fileは適宜変更
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
* ctrl+.でなぜかアンダーバー付きのeが入力されるので下記で解消
  * 参考) https://askubuntu.com/questions/1404448/key-combination-ctrlperiod-not-working-ubuntu-22-04
  * ibus-setup
  * GUI開くので`Emoji`タブ => Emoji annotationで`<Control>period`削除
* 参考) https://hirooka.pro/ubuntu-22-04-lts-japanese-input-ibus-fcitx-mozc/
* settings => Resion & Language => Manage Installed Languages => install
* logout
* autokeyをコンソールなどでも有効化するため、Xorgでログイン
  * password入力前に右下歯車アイコン押下 => Ubuntu on Xorg
* ロック無効化
  * settings => Keyboard => View and Customize Shortcuts => Lock screen => 選択 => Backspace => Set
* ランチャーのバーを右端へ
  * settings => Appearance => Dock => Position on screen => right
* ctrl + alt + iによるスクリーンロック無効化
  * win => settings => Keyboard
* vscode
  * autokey関連設定後、無変換押下でメニューバーの"file"にフォーカスが当たる問題
  * ctrl + , => "menu bar"で検索 => Menu Bar Visivilityをhiddenに変更
* terminal
  * ディレクトリ名が青で見にくいのでtheme変更
    * 右上3段バーガーアイコン => preferences => profilesのunnamed
    * colors => Built-in schemes => Solarized
* super + d無効化
  * settings => Keyboard => View and Customize Shortcuts => Hide all normal windows => 選択 => Backspace => Set

## shell-gpt導入

* api key確認
  * https://platform.openai.com/account/api-keys
  * apiは使った分だけ課金され、chatGPTの課金とは別
* sgpt設定
  * sgpt
    * api key入力求められるので入力
  * sgpt hello
    * 何か返事が返って来ればok
* shell補完機能追加
  * sgpt --install-integration
  * ctrl + lで補完

## ショートカット設定

* setting => keyboard => custom shortcuts
  * ctrl+alt+dでダウンロードディレクトリを開くよう設定
    * name: open download directory
    * command: nautilus Downloads
    * shortcut: ctrl + alt + d
