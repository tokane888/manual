## man dh_make まとめ（ほぼ翻訳)

* ソースコードをdebian policyに従ってパッケージングするツール
* ソースコードを含むディレクトリで呼ぶ
  * ディレクトリ名は '<パッケージ名>-<version>'
    * <パッケージ名> : 小文字、ハイフン、数字を含められる
* debianディレクトリと、パッケージ名、versionでカスタマイズされたcontrolファイル類を生成

### パッケージクラス

* Single binary (s)
  * 最も基本的なクラス
    * 単一の.debバイナリ
* Arch-Independent (i)
  * アーキテクチャ非依存の単一パッケージ生成
* Library (l)
  * 最低2つのバイナリを生成
    * /usr/libにライブラリを含むだけのバイナリ
    * *-dev_*.deb: ドキュメントとC headerを含むバイナリ
* Python (p)
  * dh_make実行時に特にオプション指定しないと、s,i,l,pのどれか確認される
  * Pythonについてはmanに記載なし

### オプション指定

* --native指定がない場合
  * dh_makeはoriginal source archive (<package名>-<version>.orig.tar.gz)があることを確認する
    * アーカイブは.gz, bz2, lzma形式でも良い
    * 上記ファイルがない場合、-fで指定されたファイルがディレクトリにコピーされる
    * -f指定がなく、--createorigがある場合
      * 下記が作成された
        * ../<packagename>-<version>.orig.tar.gz
        * ../<packagename>-<version>.tar.gz
* -c, --copyright (license)
  * 生成するcopyrightファイルのライセンスタイプ
    * -c gpl3などとすると、gpl3のライセンス等を書き込んだcopyrightが生成される
  * ライセンスはgpl, gpl2, gpl3, lgpl, lgpl2 lgpl3, artistic, apache, bsd, mit, custom等が指定可能
  * 指定なしの場合、copyrightファイルのlicense type部分は空になる
* --docs
  * 独立したPACKAGE-docバイナリパッケージ生成
* -e, --email (address)
  * (address)部分を、debian/control記載の、Maintainerのメールアドレスフィールドに使用
* -n, --native
  * native debianパッケージ生成
    * .origアーカイブを生成しない
* -f, --file (file)
  * (file)をoriginal source archiveに使用
* -l, --library
  * package class = Library に設定。ユーザー入力を求められる質問省略
* -s, --sinble
  * package class = Single Binary に設定。ユーザー入力を求められる質問省略
* -i, --indep
  * package class = arch-independent Binary に設定。ユーザー入力を求められる質問省略
* -a, --addmissing
  * 既存のdebianソースディレクトリに、削除されているexample, controlファイルを再生成
* -t, --templates (directory)
  * debianディレクトリの生成に、カスタムテンプレート使用
* -o, --overlay (directory)
  * 既存のdebianディレクトリに、カスタムテンプレート適用
* -p, --packagename (name)
  * パッケージ名を強制的に(name)にする
* -d, --defaultless
  * default templateをdebianディレクトリに適用しない。通常--overlay, --templatesなどと同時に指定
* -y, --yes
  * 質問にすべて自動的にyesと返答

### 環境変数

* DEBEMAIL
  * control, changelogで使用するメールアドレス
* DEBFULLNAME
  * control, changelogで使用する氏名
* EMAIL
  * control, changelogで使用するメールアドレス
  * DEBEMAILが指定されていない場合に使用
* LOGNAME
  * email, full nameを他のディレクトリから探す際に、デフォルトで使用されるuser名

### ファイル

* /usr/share/debhelper/dh_make
  * 全テンプレートファイル格納
  * 埋め込み可能な部分は#でくくられている(#YEAR#, #EMAIL#等)
  * 配下のディレクトリ一覧
    * debian
      * ベースとなるテンプレファイル一覧
      * 20近いテンプレファイルが入ってる
    * debians
      * パッケージクラスSingle binary (s) の場合に、debianのテンプレを上書き
      * controlのテンプレだけ入ってる
    * debiani
      * パッケージクラスArch-Independent(i) の場合に、debianのテンプレを上書きして使うっぽい
      * controlのテンプレだけ入ってる
    * debianl
      * パッケージクラスLibrary(l) の場合に、debianのテンプレを上書き
      * control以外に5つ入ってる
    * debianm
      * control以外に2つ入ってる
      * controlで2つのパッケージの情報が記載されている
      * 複数パッケージを同時インストールするパッケージ向けの設定？
    * native
      * native debian package用のテンプレ
    * licenses
      * gpl3, apache等、Debian packageで使用される各共通ライセンスのテンプレ


### その他

* ユーザー名は下記の順で取得を試みる
  * 環境変数$DEBFULLNAME, $LOGNAME
  * NIS, YP, LDAP
