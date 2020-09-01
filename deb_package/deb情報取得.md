## debパッケージの情報取得

### ファイル一覧取得

* リポジトリから
    * `apt-file list (パッケージ名)`
        * apt-fileパッケージが必要
        * 10秒以上かかることが多い
* インストール済みパッケージから
    * `dpkg -L (パッケージ名)`
* .debファイルから
    * `dpkg -c *.deb`

### メタデータ取得

* リポジトリから
    * パッケージのバージョン一覧取得
        * `apt policy (パッケージ名)`
    * パッケージの依存関係取得
        * `apt depends (パッケージ名)`
    * パッケージの詳細メタデータ取得（依存関係など含む）
        * `apt show (パッケージ名)`
            * Recommends, Suggestsパッケージ等確認可能
                * Recommends: 合わせてインストールされるパッケージ
                * Suggests: インストールされない。おすすめパッケージ
* インストール済みパッケージから
    * 詳細メタデータ取得（依存関係など含む）
        * `dpkg -s (パッケージ名)`
* .debファイルから
    * dpkg-deb -I *.deb

### .deb, ソースファイル取得、分析

* .debファイルをカレントディレクトリにダウンロード
    * `apt download (パッケージ名)`
* .debファイル及び依存パッケージをカレントディレクトリにダウンロード
    * apt install apt-rdepends
    * apt download $(apt-rdepends [package名] | grep -v "^ ")
        * 上記で、下記のエラーが出る場合がある
            * E: Can't select candidate version from package debconf-2.0 as it has no candidate
                * 下記で対応
                    * apt-get download $(apt-rdepends [package名] | grep -v "^ " | sed 's/debconf-2.0/debconf/g')
                * エラーが出るパッケージ名は上記と別の場合もある
                    * 下記でエラーが出たパッケージをinstall対象から除外して対応
                        * apt-get download $(apt-rdepends [package名] | grep -v "^ " | sed '/[除外対象]/d')
* preinst, postinst, prerm, postrm取得
    * .debファイルから
        * `dpkg -e *.deb`
            * カレントにDEBIANディレクトリが生成され、その下に各ファイルが出力される
* ソースパッケージ取得
    * `apt source (パッケージ名)`
* インストールシミュレーションを実行し、依存関係、バージョン等確認
    * `apt install -s (パッケージ名)`

### その他

* 特定ファイルを含むパッケージ名取得
    * `dpkg -S (ファイルパス)`
        * ex) `dpkg -S /usr/bin/gpg`
* aptのログ
    * /var/log/apt/history.log
* .debファイル展開
    * カレントディレクトリに展開
        * ar vx *.deb
            * 注) カレントディレクトリに大量のファイルが作成される
* パッケージインストールテスト
    * apt install --dry-run (パッケージ名)
    * apt install -s (パッケージ名)
        * どちらでも同じ
        * 依存関係でエラーが出ないことのチェックなどに使用