# Virtual Box操作

* 時刻変更
  * OS上での時刻同期停止
    * ubuntuの場合
      * systemctl stop systemd-timesyncd
      * systemctl disable systemd-timesyncd
  * host側との時刻同期停止
    * 参考) https://askubuntu.com/questions/35566/how-do-i-manually-set-the-system-time-in-virtualbox
    * VirtualBoxマネージャー左端のOS名確認
      * 例) Ubuntu 18.04.5
    * コマンドプロンプトを開き、下記実行
      * cd "C:\Program Files\Oracle\VirtualBox"
      * VBoxManage setextradata "(OS名)" "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled" 1
  * guest OS再起動
  * OS時刻変更  
    * date -s (日時)
      * 例) date -s 20220520