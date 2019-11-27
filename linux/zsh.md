##zsh 安装 ubuntu
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
安装插件 自定义插件安装目录 .oh-my-zsh/plugins
- git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
- git clone https://github.com/zsh-users/zsh-history-substring-search
- git clone https://github.com/zsh-users/zsh-autosuggestions
- git clone https://github.com/jhipster/jhipster-oh-my-zsh-plugin.git

mv jhipster-oh-my-zsh-plugin jhipster

```

vim ~/.zshrc

plugins=(
  git
  zsh-history-substring-search
  zsh-autosuggestions
  zsh-syntax-highlighting
  extract
  docker
  jhipster
)

setopt nonomatch #增加识别通配符 比如* 在最后一行加入

source ~/.zshrc

rm ~/.zcompdump* #解决vim tab报错
```

##zsh 安装 centos
```
yum install -y zsh
chsh -s /bin/zsh
yum install -y git

git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
source ~/.zshrc

#同上
```