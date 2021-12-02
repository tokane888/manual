# systemd service

## serviceファイルverify方法

* systemd-analyze verify (serviceファイルフルパス) 
  * 下記の警告が出る場合があるが、systemdのバグなので無視してOK
    ```
    Attempted to remove disk file system, and we can't allow that.
    ```

## Unitセクション

* StartLimitBurst
  * StartLimitIntervalSec秒(デフォルト10秒)の間に許容するservice自動再起動回数
  * デフォルトは5回
  * 指定回数を超えるとservice再起動を試みなくなる
    * Restart = alwaysをServiceセクションで指定していても試みない