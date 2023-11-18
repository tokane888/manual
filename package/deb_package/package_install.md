## debパッケージインストール関連コマンド

* Recommendsパッケージをインストールせずに当該パッケージをインストール
    * apt install --no-install-recommends -y (パッケージ名)
        * recommendパッケージは下記コマンド実行で`Recommends:`欄に表示される
            * apt show (パッケージ名)
                * 各項目記載のパッケージのデフォルトの扱いは以下
                    * `Depends:` => インストールされる
                    * `Recommends:` => インストールされる
                    * `Suggests:` => インストールされない
    * Dockerfileで、容量節約のために使われることが多い
        * パッケージサイズが1割、2割減る程度なのでなくても良い
* 使われなくなった依存パッケージ一括削除
    * apt autoremove
