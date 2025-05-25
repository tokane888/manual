# Devin's Workspace Setup

1. Git pull
   1. そのままでok
2. Configure Secrets
   1. 一旦無視
3. Install Dependencies
   1. cd ~/repos/ip-block-batch && go mod tidy
      1. cd先のパスは要調整
      2. 事前に下記でgoのinstallが必要

        ```shell
        sudo apt update
        sudo apt install -y wget tar
        sudo rm -rf /usr/local/go
        wget https://go.dev/dl/go1.23.8.linux-amd64.tar.gz
        sudo tar -C /usr/local -xzf go1.23.8.linux-amd64.tar.gz
        ```
4. 3番と同じ
5. Setup lint
   1. go fmt ./... && go vet ./...
6. Setup Tests
   1. go test -v ./...
