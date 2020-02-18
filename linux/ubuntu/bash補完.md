## bash補完有効化

* bash-completionがインストール済みで有ることを確認
* /etc/profile の、/etc/bash_completion読み込み部分のコメントアウト解除

```
# enable bash completion in interactive shells
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
```