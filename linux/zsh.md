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
安装插件 自定义插件安装目录 .oh-my-zsh/custom/plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
git clone https://github.com/zsh-users/zsh-history-substring-search
git clone https://github.com/zsh-users/zsh-autosuggestions

```
apt-get install autojump

vim ~/.zshrc

plugins=(
  git
  zsh-history-substring-search
  zsh-autosuggestions
  zsh-syntax-highlighting
  extract
  docker
)
. /usr/share/autojump/autojump.sh

source ~/.zshrc

rm ~/.zcompdump* #解决vim tab报错
```

