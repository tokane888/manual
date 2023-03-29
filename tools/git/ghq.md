# ghq

## 導入手順

go install github.com/x-motemen/ghq@latest

## コマンド

- git clone
  - ghq get (uri)
- git repo一覧表示
  - ghq list

## pecoと連携

下記を.zshrcに記載でctrl+]でrepo移動可能に

```
function peco-src () {
  local selected_dir=$(ghq list -p | peco --query "$LBUFFER")
  if [ -n "$selected_dir" ]; then
    BUFFER="cd ${selected_dir}"
    zle accept-line
  fi
  zle clear-screen
}
zle -N peco-src
bindkey '^]' peco-src
```