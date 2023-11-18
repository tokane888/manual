## dh_makeで生成されたテンプレ確認

* changelog
  * 変更履歴
  * 基本的に変更不要
    * --emailオプションなしの場合、email部分に変な改行が入ることがある
  * 他パッケージのchangelogは下記で確認可能
    * apt changelog (パッケージ名)
* compat
  * debhelperとの互換性レベル
  * 新規パッケージでは10と書いておけば良いとのこと
    * [公式](https://www.debian.org/doc/manuals/maint-guide/dother.ja.html#compat)
  * dh_makeで自動生成された直後は9になっているので、問題無ければ10にする
* control
  * パッケージ名、アーキテクチャ等
  * 他パッケージのcontrolは下記で確認可能
    * apt show (パッケージ名)
* copyright
  * 当該パッケージのライセンス記載
  * OSSのライセンスも記載
  * フォーマットはプログラムが処理しやすいフォーマットに沿ったものが生成される
    * [Machine-readable debian/copyright file](https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/)
* init.d
  * systemdを使っていないシステム向けのserviceファイル的なもの
* manpage.*
  * manpageのテンプレ
* menu
