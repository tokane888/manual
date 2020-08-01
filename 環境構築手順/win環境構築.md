# Windowsセットアップ手順

## 旧PC作業

* 下記をデータ移行用のdropboxのディレクトリへ移動
  * ~/.ssh ディレクトリ
  * 下記ディレクトリのExcel個人用マクロ
    * C:\Users\[User]\Appdata\Roaming\Microsoft\Excel\XlStart
  * tampermonkeyのjs
  * clibor設定ファイル
  * vscode設定ファイル(必要に応じて)
  * その他個人情報を移動

## chocolateyによるソフトインストール

* chocolateyインストール
  * (参考) https://chocolatey.org/install
  * 下記で管理者権限のpowershell起動
    * win+x => a
  * 下記実行でpolicyがrestrictedになっていないか確認
    * Get-ExecutionPolicy
  * restrictedになっている場合、無制限のBypassに変更
    * Set-ExecutionPolicy Bypass
  * 下記実行でpolicyがBypassになっていることを確認
    * Get-ExecutionPolicy
  * install
    * Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
  * 下記でhelpを表示し、install成功を確認
    * choco -

## 各種設定

* レジストリでcaps lock => ctrlに変更
  * http://www.itmonologue.com/blog/from-capslock-to-ctrl/
* explorerのグループ化解除
  * explorer開く => 表示 => グループ化 => なし
* 隠しファイル、拡張子表示
  * explorer開く => 表示 => オプション => 表示
    * 隠しフォルダ表示、拡張子も表示 => OK
* 仮想ディスプレイ移動時のエフェクト無効化
  * win+iで設定表示
  * 簡単操作押下
  * "Windowsにアニメーションを表示する"をオフに
* StudyTimerインストール
* cliborインストール
* clibor設定
  * タスクトレイから設定表示
  * ホットキー => メイン画面の呼び出し => Altキーを二回で呼び出し
* インストールスクリプト実行
  * TODO: WSL2のpackageがchocolateyのrepoに追加された後に、インストール処理追加
  * TODO: ubuntuディストリビューションのインストールについても追記
* sakura editor設定
  * スクリーン => レイアウト => 折り返し方法 => 折り返さない
  * ウィンドウ => デフォルトの文字コード => 改行はLF, 文字コードはUTF-8
* chrome private mode無効化
  * REG ADD HKLM\SOFTWARE\Policies\Google\Chrome /v IncognitoModeAvailability /t REG_DWORD /d 1
* pico viewerインストール
* teams設定
  * タスクトレイから起動
  * 左メニューから"最新情報" => 左上の歯車アイコン押下 => 通知をoffに設定
  * 設定画面で"一般" => "バックグラウンドでアプリケーションを開く"
* 共通マクロ移動
  * 下記へ個人用マクロを配置
    * C:\Users\[User]\Appdata\Roaming\Microsoft\Excel\XlStart
* git-bash設定
  * git statusでの文字化け対策
    * git config --global core.quotepath false
  * docker exec時にwinpty入力不要に
    * 管理者権限でgit-bash実行
    * vim /etc/profile.d/aliases.sh
    * 当該箇所に下記のようにdocker追加
    ```
    - for name in node ipython php php5 psql python2.7
    - for name in node ipython php php5 psql python2.7 docker
    ```
    * git-bash再起動
