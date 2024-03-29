# Virtual Box

## Windowsへの導入時注意点

* Virtual BoxはHyper-V有効状態では正常に動作しない
  * Virtual Box ver 6.1.12で試したところ、下記OSのインストール中にエラー終了
    * Cent OS 8
    * Ubuntu 20.04
    * 共存可能になったとの情報もあるが、上述のようにまだ不安定
  * Docker for Windowsは、WSL2をベースとして使用する設定にしていない場合、Hyper-V必須となり競合する

## Windows10へのVirtual Box導入手順

* chocolateyでインストール
  * cinst --yes virtualbox

## Ubuntu 22.04でのVirtual Box導入手順

* 下記でのインストールは失敗
  * apt install -y virtualbox
  * oracleのdeb repository追加してのinstall
* 直接下記から7系をdeb downloadしてインストールしたところ成功
  * https://www.virtualbox.org/wiki/Linux_Downloads

## 各種設定

* ハードウェア仮想化が有効であることを確認
  * ctrl+shift+esc => パフォーマンス
  * "仮想化"が"有効"であることを確認
    * 無効である場合は、BIOS/UEFIからAMD-V、Intel VT-xのいずれかを有効化
* Hyper-V無効化
  * win menuで"Windowsの機能の有効化および無効化"選択
  * Hyper-V無効化
  * 補足
    * Virtual boxはHyper-V有効状態でもすぐにはゲストOSが落ちないが、、OSインストール中に落ちる
      * 無効化は必須
    * Docker for Windowsは、WSL2をベースとして使用する設定にしていない場合、Hyper-V必須となり競合する
* ホストキー設定
  * 上部のmenuから"ファイル" => "環境設定"
  * "入力" => "仮想マシン"
  * "ホストキーの組み合わせ"をWinキーに設定
* ファイル => 環境設定 => アップデート => "アップデートを確認"を無効化
  * 毎回ホップアップを押下するのが面倒なので
* TODO: マウスをシームレスに移動可能にするマウス統合機能について調査

## OSインストール例

* Ubuntu 20.04インストール
  * 参考) https://qiita.com/pyon_kiti_jp/items/0be8ac17439abf418e48
  * 公式サイトからisoダウンロード
    * http://www.releases.ubuntu.com/16.04/
  * Virtual Box起動
  * "新規" => 下記設定で仮想ハードディスク作成
    * 名前: Ubuntu20.04
    * タイプ: Linux
    * バージョン: Ubuntu (64-bit)
    * メモリ: 3GB以上で任意に設定
      * 少ないとOSインストール中にハングアップとの情報あり
      * 2GBで試したところ、非常に遅く実用に耐えないレベル
    * "仮想ハードディスクを作成する" => "VDI" => "可変サイズ"
    * ファイルの場所とサイズ: 20GB程度で設定
  * host-guest間の双方向コピペとドラッグ&ドロップ有効化
    * OS右クリック => 設定
    * 一般 => 高度
    * クリップボードの共有 => 双方向
    * ドラッグ & ドロップ => 双方向
    * OK
    * これは初回起動時は有効にならず、2回目以降に有効になる
  * 左メニューからUbuntuを選択して"起動"
  * "起動ハードディスクを選択"ウィンドウでUbuntu 20.04を選択
    * 表示されない場合、右端のディレクトリアイコンから選択
  * "起動"
  * 初回起動時に表示されるダイアログ、もしくは起動後の画面上のメニューから下記選択
    * デバイス => 光学ドライブ => ディスクファイルを選択 => .isoファイルを選択
  * 実機へのインストールと同様にOSインストール
  * OSインストール後に、「DVDを排出してEnter押下」を促すメッセージが表示されるので、下記で排出してenter
    * デバイス => 光学ドライブ => ディスクイメージを選択/作成 => .iso選択 => 空のままにする

* CentOS 8インストール
  * 参考) https://linuxhint.com/install_centos8_virtualbox/
  * 公式サイトからisoダウンロード
    * https://centos.org/download/
  * Virtual Box起動
  * "新規" => 下記設定で仮想ハードディスク作成
    * 名前: CentOS 8
    * タイプ: Linux
    * バージョン: Red Hat (64-bit)
    * メモリ: 3GB以上で任意に設定
      * 少ないとOSインストール中にハングアップとの情報あり
    * "仮想ハードディスクを作成する" => "VDI" => "可変サイズ"
    * ファイルの場所とサイズ: 20GB以上で設定
  * 左メニューからCentOS8を選択して"起動"
  * "起動ハードディスクを選択"ウィンドウでCentOS8を選択
    * 表示されない場合、右端のディレクトリアイコンから選択
  * "起動"
  * 初回起動時に表示されるダイアログ、もしくは起動後の画面上のメニューから下記選択
    * デバイス => 光学ドライブ => ディスクファイルを選択 => .isoファイルを選択
  * 実機へのインストールと同様にOSインストール

## OSインストール後の環境設定

* 外部からssh接続可能に設定
  * virtual box左上から ファイル => 環境設定 => ネットワーク
  * 右上のアイコンを押下してネットワーク追加 => OK
  * 当該guest OSが起動していればシャットダウン
  * OS一覧で当該OSを右クリック => 設定 => ネットワーク
  * アダプター2が有効になっているので選択
  * "ネットワークアダプターを有効化"を選択
  * 割り当て => ホストオンリーアダプター
  * guest OS起動
  * guest OSに、ssh接続に必要なパッケージインストール
    * ubuntuの場合: openssh-server
  * guest OSのIP確認
    * ip addr
  * 上記のIPは複数あるのでhost OSからpingが通るIPを確認
  * host OSから、guest OSのpingが通るIPへssh接続可能に
    * ssh user@(ip)
