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
* TODO: guest OSからテキストコピペ可能に

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
    * メモリ: 4GB以上で任意に設定
      * 少ないとOSインストール中にハングアップとの情報あり
    * "仮想ハードディスクを作成する" => "VDI" => "可変サイズ"
    * ファイルの場所とサイズ: 20GB程度で設定
  * 左メニューからUbuntuを選択して"起動"
  * "起動ハードディスクを選択"ウィンドウでUbuntu 20.04を選択
    * 表示されない場合、右端のディレクトリアイコンから選択
  * "起動"
  * 実機へのインストールと同様にOSインストール

* CentOS 8インストール
  * 参考) https://linuxhint.com/install_centos8_virtualbox/
  * 公式サイトからisoダウンロード
    * https://centos.org/download/
  * Virtual Box起動
  * "新規" => 下記設定で仮想ハードディスク作成
    * 名前: CentOS 8
    * タイプ: Linux
    * バージョン: Red Hat (64-bit)
    * メモリ: 4GB以上で任意に設定
      * 少ないとOSインストール中にハングアップとの情報あり
    * "仮想ハードディスクを作成する" => "VDI" => "可変サイズ"
    * ファイルの場所とサイズ: 20GB以上で設定
  * 左メニューからCentOS8を選択して"起動"
  * "起動ハードディスクを選択"ウィンドウでCentOS8を選択
    * 表示されない場合、右端のディレクトリアイコンから選択
  * "起動"
  * 実機へのインストールと同様にOSインストール
