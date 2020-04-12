## windows10

* windows10関連の設定

### windows menuでのEdge検索無効化

* ctrl+r => "regedit.exe" => enter
* "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Search"へ移動
* 右クリック => "New" => "DWORD (32-bit)" => "BingSearchEnabled" => value0でenter
* 同じディレクトリ内の"CortanaConsent"のValueを0に設定
* 再起動
