# postgresコマンド

* 検証環境
  * Ubuntu 20.04

## 型

* 文字列
  * TEXT型使えばOK
  * char(n)型: 文字数分までspaceがpaddingされる
  * varchar(n)型: 文字数可変の文字列型。文字数超過でエラー。基本textで良い

## sql例

* create table
  ```
  create table meibo (
    id serial,
    name text,
    zip char(8),
    address text,
    birth date,
    sex boolean
  );
  ```
  * 上記実行でシーケンスは自動で作成されている
    * \d
      ```
      postgres=# \d
                    List of relations
      Schema |     Name     |   Type   |  Owner
      --------+--------------+----------+----------
      public | meibo        | table    | postgres
      public | meibo_id_seq | sequence | postgres
      (2 rows)
      ```
  * tableのカラム詳細確認は下記(上記create table後多少変更を加えた後実行結果)
    * \d (table名)
      ```
      postgres=# \d meibo
                                    Table "public.meibo"
      Column  |     Type     | Collation | Nullable |              Default
      ---------+--------------+-----------+----------+-----------------------------------
      id      | integer      |           | not null | nextval('meibo_id_seq'::regclass)
      name    | text         |           |          |
      zip     | character(8) |           |          |
      address | text         |           |          |
      birth   | date         |           |          |
      sex     | boolean      |           |          |
      Indexes:
          "id_idx" UNIQUE, btree (id)

      ```
  * 
* insert
  ```
  insert into meibo(name, zip, address, birth, sex) values ('鈴木花子', '111-0000', '東京都千代田区', '1950/1/1', false);
  insert into meibo(name, zip, address, birth, sex) values ('田中一郎', '222-3333', '神奈川県横浜市', '1970/5/5', true);
  insert into meibo(name, birth, sex) values ('山田太郎', '1990/7/20', true);
  insert into meibo(name, zip, address, birth, sex) values ('佐藤道子', '333-4444', '千葉県千葉市', '1988/3/3', '0');
  insert into meibo(name, zip, address, birth, sex) values ('近藤次郎', '444-5555', '大阪府大阪市', '1960/12/24', '1');
  insert into meibo(name, zip, address, birth, sex) values ('石川順子', '555-6666', '北海道札幌市', '2000/5/3', 'no');
  ```
* select
  * select * from meibo where birth > '1970/1/1';
  * select avg(id) from meibo;
  * select min(id) from meibo;
  * select max(id) from meibo;
  * select sum(id) from meibo;
  * select pow(id, 2) from meibo;
  * multi byte文字の文字数カウント
    * select name, char_length(name) from meibo;
  * postgresのversion確認
    * select version();
  * id: 4-5番取り出し
    * select * from meibo limit 2 offset 3;
  * 性別ごとのデータ数集計
    * select sex, count(*) from meibo group by sex;
  * データ数3以上の性別だけ集計
    * select sex, count(*) from meibo group by sex having count(*) >= 3;

## 機能

* index
  * 注意点
    * 重複が多いカラムには使わない
      * indexで取り出しても実処理が軽くならず負荷だけ大きいため
  * 作成
    * unique index
      * 重複しないカラムに張るindex
      * create unique index id_idx on meibo(id);
  * 削除
    * drop index id_idx;
  * 一覧表示
    * \di
* 制約
  * not null制約付与
    * alter table meibo alter name set not null;
  * unique制約付与
    * alter table (table名) add unique (カラム名)
      * unique制約有りのカラムには重複値を挿入できないが、null値は挿入可能
  * 主キー制約
    * alter table meibo add primary key (id);
  * check制約
    * 例
      * alter table (table名) add check price >= 0;
  * default制約
    * create tableで指定する場合
      * create table table名 (列名 データ型 default デフォルト値);
  * 外部キー制約
    * create tableで指定する場合
      * create table table名 (列名 データ型 references table名 [列名])
* トランザクション
  * デフォルトのトランザクションレベル確認
    * show default_transaction_isolation;
  * 開始
    * 下記実行でトランザクション開始
      * begin;
      * レベルを指定する場合は下記形式
        * begin isolation level serializable;
    * 指定可能なレベルは下記  
      * serializable
        * 最も安全
      * repeatable read
        * ファントムリードの可能性あり
          * トランザクション中に他のトランザクションによってデータがinsert, updateされ、前後でデータ数が異なる
      * read commited
        * 上記に加え、反復不能読み取りの可能性あり
          * トランザクション内で一度読み込んだデータを再度読み込んだ際に、他のトランザクションが当該データを更新される
        * defaultはこれだった
      * read uncommited
        * 上記に加え、ダーティーリードの可能性あり
          * 書き込みが行われたが、コミットされていないデータを同時実行された他のトランザクションが読み込み
        * 使うことはないが、使用すると実際にはread commitedと同様の処理をするとのこと
          * postgresではダーティーリードが発生しない仕組みのため
  * commits
    * 下記実行でコミット
      * commit;
  * rollback
    * 下記実行でrollback
      * rollback;
  * savepoint
    * savepointを指定すると、rollbackしてもsavepointまでは確定される
    * savepointに戻るときの構文は下記
      * rollback to (savepoint名)
* 予約語確認
  * select * from pg_get_keywords();
* 試験的にコンテナ作成
  ```
  docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
  docker exec -it some-postgres psql postgres postgres
  ```

## トラブルシュート

* ラズパイでtimestamp型のdefault値をCURRENT_TIMESTAMPにすると、現在のtransaction開始時点の時刻が入る
  * 1950年とかになったりする
  * now()等も同じ
  * ラズパイ上のdockerコンテナはバグ？で時刻が正常にhostと同期しないことがあるため
* 実行中のsql確認
  * 後述のコマンドの出力が見にくいので、まず拡張表示をonに
    * \x
  * select * from pg_stat_activity;
  * activeなsqlに限定して必要な項目のみ表示する場合は下記
    * SELECT pid, query_start, query AS query FROM pg_stat_activity WHERE state='active' ORDER BY query_start;
* ロック状態確認
  ```
  SELECT l.pid,l.granted,d.datname,l.locktype,relation,relation::regclass,transactionid,l.mode
    FROM pg_locks l  LEFT JOIN pg_database d ON l.database = d.oid
    WHERE  l.pid != pg_backend_pid()
    ORDER BY l.pid;
  ```