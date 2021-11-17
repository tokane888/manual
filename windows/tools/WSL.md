## Windows Subsystem Linux

### コマンド

* カレントディレクトリでエクスプローラを開く
  * explorer.exe .
* クリップボードへコピー
  * echo hoge | clip.exe
* Windowsのfile pathをlinux向けに変換
  * wslpath (Windows形式のpath)

### WSL2導入手順

* TODO: 下記の手順でWSL1が導入されたので、詳細確認
* Window10のバージョンを2004以上にupdate
* 参考) https://docs.microsoft.com/ja-jp/windows/wsl/install-win10
* Linux用Windows subsystem有効化
    * win+x => win+a
    * dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
* 仮想マシンプラットフォーム 有効化
    * win+x => win+a
    * dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    * Windows再起動
    * 指示に従ってkernel update
        * https://docs.microsoft.com/ja-jp/windows/wsl/wsl2-kernel
    * wsl --set-default-version 2
* Microsoft storeからUbuntu installして実行
* Docker起動時にバックエンドをWSL2に変更したいと言われるので良ければ変更
* WSL2をデフォルトでrootユーザーで起動するように設定
    * 下記でディストリビューション一覧表示
        * wsl -l
    * デフォルトユーザーをrootにしたいディストリビューションを指定して下記実行
        * Ubuntu config --default-user root
* WSL2上にpython3, pipを導入する場合
    * apt update -y
    * apt install -y python3 python3-pip

### 初期設定作業

* 高速化のためにリポジトリをjpに変更
    * sed -i -e 's/\(deb\|deb-src\) http:\/\/archive.ubuntu.com/\1 http:\/\/jp.archive.ubuntu.com/g' /etc/apt/sources.list
    * あとからの変更だと依存関係で問題が生じる場合がある
* ctrl+shift+(c/v)でコピペ可能にする
    * ウィンドウのヘッダ上で右クリック=>Properties押下
    * Optionsタブ選択
    * "QuickEdit Mode"有効化
    * "Use Ctrl+Shift+C/V as Copy/Paste"有効化
    * OK押下
    * 注意) 複数行コピー時は範囲選択後、"Enter"でコピー
        * ctrl+shift+cでコピーすると改行が無視される
* 日本語入力可能に
    * 通常のubuntuと同じ。locale設定で対応
        * ConEmu上のubuntuは問題ないが、Ubuntuを直接起動すると日本語が文字化け
            * TODO: 対策検討
* visual studio codeのデフォルトのshellに設定
    * ctrl+@ 押下
    * コンソール右上からプルダウン押下
    * "Select Default Shell"選択
    * "WSL Bash"選択
        * 一度選択すると、プルダウンに起動するコンソールの候補として表示されるようになる
* ssh用のid_rsa鍵とconfigを~/.ssh配下にコピーして権限設定
    * windowsのユーザーの$HOMEディレクトリは見に行かない
        * windowsのディレクトリでは権限設定(600)ができないので見に行ってもssh接続時にエラーになる
* WSLの外のdocker toolboxへアクセス可能に設定
    * 参考) https://medium.com/@joaoh82/setting-up-docker-toolbox-for-windows-home-10-and-wsl-to-work-perfectly-2fd34ed41d51
    * WSL上に空のtmp.sh作成
      * vim tmp.sh
    * 下記をtmp.shにコピペ
        ```
        # Update the apt package list.
        sudo apt-get update -y

        # Install Docker's package dependencies.
        sudo apt-get install -y \
            apt-transport-https \
            ca-certificates \
            curl \
            software-properties-common

        # Download and add Docker's official public PGP key.
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

        # Verify the fingerprint.
        sudo apt-key fingerprint 0EBFCD88

        # Add the `stable` channel's Docker upstream repository.
        #
        # If you want to live on the edge, you can change "stable" below to "test" or
        # "nightly". I highly recommend sticking with stable!
        sudo add-apt-repository \
          "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
          $(lsb_release -cs) \
          stable"

        # Update the apt package list (for the new apt repo).
        sudo apt-get update -y

        # Install the latest version of Docker CE.
        sudo apt-get install -y docker-ce

        # Allow your user to access the Docker CLI without needing root access.
        sudo usermod -aG docker $USER
        ```
    * chmod 777 tmp.sh
    * sudo ./tmp.sh
    * 一度コンソールを閉じて、再度開く
        * dockerを一般ユーザーで使用可能にするため
    * apt install -y docker-compose
    * 下記のパスがPATHに含まれていなければ追加
        * $HOME/.local/bin
    * docker toolboxがexportしているportを確認
        * powershellを開いて下記実行
            * docker-machine config
                ```
                PS C:\Users\tom> docker-machine config
                --tlsverify
                --tlscacert="C:\\Users\\tom\\.docker\\machine\\machines\\default\\ca.pem"
                --tlscert="C:\\Users\\tom\\.docker\\machine\\machines\\default\\cert.pem"
                --tlskey="C:\\Users\\tom\\.docker\\machine\\machines\\default\\key.pem"
                -H=tcp://192.168.99.101:2376
                ```
    * 上記のIP及びユーザー名を含む下記設定をWSL上の~/.bashrcに追加
        ```
        # WSL上からDocker Toolboxに接続可能に設定
        export DOCKER_HOST=tcp://192.168.99.101:2376
        export DOCKER_CERT_PATH=/mnt/c/Users/tom/.docker/machine/certs
        export DOCKER_TLS_VERIFY=1
        ```
    * source ~/.bashrc
        * これでdockerが最低限使用可能に
    * dockerのvolume設定可能に
        * WSLとDocker Toolboxはパス形式が異なる
        * WSL: /mnt/c
        * Docker Toolbox: /c
        * シンボリックリンク作成して差異を吸収
            * sudo ln -s /mnt/c /c
    * sudo vim /etc/wsl.conf
        * 下記コピペして保存
            ```
            [automount]
            options = "metadata"
            ```
            * windowsの各ファイルにメータデータを設定可能に
        * windows再起動
* WSL上のvimからyyで行コピー等可能に設定
    * .vimrcに下記記載

        ```
        " Windows Subsystem for Linux で、ヤンクでクリップボードにコピー
        if system('uname -a | grep Microsoft') != ''
        augroup myYank
            autocmd!
            autocmd TextYankPost * :call system('clip.exe', @")
        augroup END
        endif
        ```
* メモリリーク暫定対策
    * WSL2の既知のバグ。未改修
    * 下記のパスの設定ファイルを開く
        * %USERPROFILE%\.wslconfig
    * 下記のようにWSL2の最大メモリ使用量を指定
        ```
        [wsl2]
        memory=4GB
        swap=0
        ```
        * 指定しない場合、PC搭載メモリの80%がデフォルト値になる

### 設定リセット方法

* Win+i押下
* Apps押下
* Ubuntu押下
* "Advanced options"押下
* Reset押下
* (option)昔MSが実験的に出していたbash on ubuntu on windowsを消す場合
    * [stack over flowの手順](https://superuser.com/questions/1261110/is-it-possible-to-uninstall-bash-on-ubuntu-on-windows-since-the-latest-updates#answer-1389786)に従う

### 既知の問題

* windows terminal上でtmuxを使用すると日本語コピーに失敗
    * https://github.com/microsoft/terminal/issues/7819