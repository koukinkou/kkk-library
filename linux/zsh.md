##zsh 安装
```
apt install zsh
chsh -s $(which zsh)
```
重新登录生效

```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
source ~/.zshrc

vim ~/.zshrc
```
设置主题并生效 ZSH_THEME='ys'
安装插件


```
vim ~/.zshrc

plugins=(
  git
  autojump 
  zsh-autosuggestions
  zsh-syntax-highlighting
  extract
  docker
)
. /usr/share/autojump/autojump.sh

source ~/.zshrc
```

