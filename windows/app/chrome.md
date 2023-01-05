# chrome

## 初期に行う設定

* stackoverflow機械翻訳をgoogle検索結果から除外
  * https://github.com/arosh/ublacklist-stackoverflow-translation

## オプションの設定方法

* private mode有効/無効切り替え
  * レジストリエディタ起動
    * ctrl+r => "regedit.exe"
  * 下記へ移動
    * Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome
  * ウィンドウ右クリック => New => DWORD (32-bit) Value => "IncognitoModeAvailability"
  * "IncognitoModeAvailability" 選択
    * 以下のいずれかを入力して"OK"
      * 0: 有効
      * 1: 無効
      * 2: 常時private mode
  * Windows再起動

## ubuntuで使用する場合

* デフォルトではlocalで動かしているdnsmasqなどを使用せずに名前解決を試みるので修正
  * chrome://settings/security?search=dns
    * 設定画面から"セキュアDNSを使用する"無効化