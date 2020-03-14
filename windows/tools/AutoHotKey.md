## AutoHotKey

### 環境構築

* (インストール)
* Windows起動時に管理者権限で起動するように設定
  * Task Schedulerで新規タスク作成
    * Actions
      * Program/script
        * "C:\Program Files\AutoHotkey\AutoHotkey.exe"
      * Add arguments
        * "C:\tool\original\AHK\keyAllocation.ahk"
    * (他諸々設定)