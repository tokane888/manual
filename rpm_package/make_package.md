* パッケージ作成参考資料
    * [rpm packaging guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/rpm_packaging_guide/index)

## パッケージ作成時

* 前提
    * service名: gello
    * 実行ファイル: /opt/hoge/gello
* 手順
    * ソース作成
        * `gello.go`実装
    * Makefile作成
        * `install`含める
            * 権限、所有者設定はここでは不可
                * specファイルで設定
    * ビルド用ディレクトリ構造作成
        * rpmdev-setuptree
    * specファイルテンプレ作成
        * rpmdev-newspec gello
    * specファイル作成
        * %files
            * インストールするファイル／記載
                * 下記の場合にビルドエラー
                    * ここに記載の無い`ファイル`をインストール
                    * ここに記載のある`ファイル`をインストールしない
            * ここに記載したファイル／ディレクトリはアンインストール時に削除される
                * 共有ディレクトリの扱いは？
    * ビルド
        * `rpmbuild -bb -D "_topdir ${PWD}/build" -D "debug_package %{nil}" rpmbuild/SPECS/gello.spec`
            * debug_packageは、デバッグ用パッケージ作成時にエラーになる問題の回避策
                * 作成しないことで回避
            * _topdirは$HOMEディレクトリ以外でビルドする場合に必要
* ファイルからインストール
    * `rpm -ivh gello.rpm`
* 関連コマンド
    * .rpmの中身確認
        * `rpm -qlp gello.rpm`