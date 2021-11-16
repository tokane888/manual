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
      * これで開くのはpowershell coreではなく、旧式のpowershell 5.x
      * 一旦powershell coreは追ってインストール
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
* TODO: パッケージ自動update設定方法確認

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
* 画面の明るさ自動調整解除
  * win+iで設定表示
  * システム => ディスプレイ => "照明が変化した場合に明るさを自動的に調整する"をoffに
  * intel CPUの場合、レジストリ編集で直る場合があるとの情報
    * https://takumi9942.net/blog/?p=1726%E2%80%B3%20title=%E2%80%9DSurface%20Pro%206%E3%81%A7%E7%94%BB%E9%9D%A2%E3%81%AE%E6%98%8E%E3%82%8B%E3%81%95%E8%87%AA%E5%8B%95%E5%A4%89%E6%9B%B4%E3%82%92%E7%84%A1%E5%8A%B9%E3%81%AB%E3%81%99%E3%82%8B
    * 今回はAMDなのでレジストリの当該項目なし
  * TODO: surface laptop 3では上記実行後も変化無し。原因調査
  * 他参考) 
    * https://support.microsoft.com/en-us/help/4548623/adaptive-brightness-and-contrast-on-surface-devices
    * https://www.reddit.com/r/Surface/comments/dw35aa/how_to_disable_adaptive_contrast_amd/
      * 結局registryはいじったが、win updateで元に戻ったっぽい
* (surfaceの場合)タッチパネル無効化
  * device manager開く
  * ヒューマンインターフェイスデバイス
  * "Surface Touch Pen Processor"右クリック => "デバイスを無効にする"
  * TODO: これ有効化したらディスプレイの明るさ自動調整が無効化できた。両方無効化する方法調査
* StudyTimerインストール
* cliborインストール
* clibor設定
  * タスクトレイから設定表示
  * 画面表示制御 => キーボードのPageUpとPageDownでページを切り替える
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
* excel設定
  * 開発タブ表示
    * ファイル => オプション => リボンのユーザー設定 => (右側の一覧から"開発"選択)
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
* visual studio code 設定同期設定
  * visual studio code左下の歯車アイコン => Turn on preference sync
* WindowsメニューからのEdge検索無効化
  * 参考) https://www.howtogeek.com/224159/how-to-disable-bing-in-the-windows-10-start-menu/
  * "regedit"でレジストリ開く
  * 下記を入力してEnterで遷移
    * HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows
    * "Windows"右クリック => "新規" => "キー" => "Explorer"と入力 => OK
  * "新規" => "DWord(32-bit)"
    * Value Name: "DisableSearchBoxSuggestions"
  * 値をダブルクリックし、下記をセット
    * Value data: 1
* manic timeの更新チェック無効化
* visual studio codeの更新チェック無効化
* Windows terminalの貼り付けコマンドをctrl+shift+vに変更
  * vimの矩形選択と競合するため
  * Windows terminal上で"ctrl+," => "keybindings"配下の"paste"設定を下記に変更
    * { "command": "paste", "keys": "ctrl+shift+v" },