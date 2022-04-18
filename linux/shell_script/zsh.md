# zsh

## wsl2へのzsh導入手順

* apt update -y
* apt install -y zsh
* 起動
  * zsh
* tmux起動時のデフォルトshellをzshに変更
  * ~/.tmux.conf末尾に下記記載
    set-option -g default-shell "${SHELL}"
    set -g default-command "${SHELL}"
  * powershellからwsl2をshutdown + 再度起動
    * wsl --shutdown
* oh-my-zshインストール
  * sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
* .bashrcから.zshrcに移行
  * cat ~/.bashrc >> ~/.zshrc
  * .zshrcからbash依存の行を削除
  * .zshrcのPS1関連設定をコメントアウトし、下記に置き換え
    ```
    ## プロンプトのオプション表示設定
    GIT_PS1_SHOWDIRTYSTATE=true
    GIT_PS1_SHOWUNTRACKEDFILES=true
    GIT_PS1_SHOWSTASHSTATE=true
    GIT_PS1_SHOWUPSTREAM=auto

    setopt PROMPT_SUBST ; PS1='%F{green}%n@%m%f: %F{cyan}%~%f %F{red}$(__git_ps1 "(%s)")%f
    %% '
    ```
  * .zshにbash_completion等の記載が残っていればコメントアウト
* 補完等強化
  * git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
    * aptでインストールした場合、$ZSH/plugins 配下にpluginが配置されず、正常に動作しない
  * git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
  * ~/.zshrcの"plugins=(***)"の行に項目追記
    * plugins=(git zsh-syntax-highlighting zsh-autosuggestions)