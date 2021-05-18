# DVD書き込み

* 参考) https://linuxconfig.org/ubuntu-20-04-burn-iso-to-dvd-from-command-line
* 書き込み可能であることと、drive nameを確認
  * cat /proc/sys/dev/cdrom/info
* apt install growisofs
* growisofs -dvd-compat -Z (/devパス)=/path/to/ubuntu-18.04.5-desktop-amd64.iso
  * 例) growisofs -dvd-compat -Z /dev/sr0=/path/to/ubuntu-18.04.5-desktop-amd64.iso