# update-alternativesコマンド

* 参考
  * https://vinelinux.org/docs/vine6/cui-guide/update-alternatives.html
* update-alternatives --display (command)
  * commandの代替を出力

```
root@ip-172-31-13-53:~/ssh/DEBIAN# update-alternatives --display vi
vi - auto mode
  link best version is /usr/bin/vim.basic
  link currently points to /usr/bin/vim.basic
  link vi is /usr/bin/vi
  slave vi.1.gz is /usr/share/man/man1/vi.1.gz
  slave vi.da.1.gz is /usr/share/man/da/man1/vi.1.gz
  slave vi.de.1.gz is /usr/share/man/de/man1/vi.1.gz
  slave vi.fr.1.gz is /usr/share/man/fr/man1/vi.1.gz
  slave vi.it.1.gz is /usr/share/man/it/man1/vi.1.gz
  slave vi.ja.1.gz is /usr/share/man/ja/man1/vi.1.gz
  slave vi.pl.1.gz is /usr/share/man/pl/man1/vi.1.gz
  slave vi.ru.1.gz is /usr/share/man/ru/man1/vi.1.gz
/usr/bin/vim.basic - priority 30
  slave vi.1.gz: /usr/share/man/man1/vim.1.gz
  slave vi.da.1.gz: /usr/share/man/da/man1/vim.1.gz
  slave vi.de.1.gz: /usr/share/man/de/man1/vim.1.gz
  slave vi.fr.1.gz: /usr/share/man/fr/man1/vim.1.gz
  slave vi.it.1.gz: /usr/share/man/it/man1/vim.1.gz
  slave vi.ja.1.gz: /usr/share/man/ja/man1/vim.1.gz
  slave vi.pl.1.gz: /usr/share/man/pl/man1/vim.1.gz
  slave vi.ru.1.gz: /usr/share/man/ru/man1/vim.1.gz
/usr/bin/vim.tiny - priority 15
  slave vi.1.gz: /usr/share/man/man1/vim.1.gz
  slave vi.da.1.gz: /usr/share/man/da/man1/vim.1.gz
  slave vi.de.1.gz: /usr/share/man/de/man1/vim.1.gz
  slave vi.fr.1.gz: /usr/share/man/fr/man1/vim.1.gz
  slave vi.it.1.gz: /usr/share/man/it/man1/vim.1.gz
  slave vi.ja.1.gz: /usr/share/man/ja/man1/vim.1.gz
  slave vi.pl.1.gz: /usr/share/man/pl/man1/vim.1.gz
  slave vi.ru.1.gz: /usr/share/man/ru/man1/vim.1.gz
```