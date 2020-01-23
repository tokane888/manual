## docker

* 一般userでdockerコマンド実行
  * docker groupにubuntuユーザーを追加
    * gpasswd --add (ユーザー名) docker
      * ex) gpasswd --add ubuntu docker
* image検索
  * docker search (image名)
  ```
  # docker search alpine
  NAME                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
  alpine                                 A minimal Docker image based on Alpine Linux…   6037                [OK]
  mhart/alpine-node                      Minimal Node.js built on Alpine Linux           453
  anapsix/alpine-java                    Oracle Java 8 (and 7) with GLIBC 2.28 over A…   439                                     [OK]
  ```
  * tag一覧取得は標準のコマンドでは無理
    * おとなしくDocker Hub見ておく

### DockerHub

#### imageをpush

* docker login
* tagの付け替え
  * docker tag (前image名):(前tag名) (user名)/(変更後image名):(変更後tag)
* imageのpush
  * docker push (user名)/(image名):(tag)
