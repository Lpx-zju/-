# Mac配置清单

## VPN

这是使用的是自行购买的梯子, 具体的教程[官网](https://msmsm.xyz/clientarea.php?action=services)上已经给出了, 按部就班的执行就可以了.

## Homebrew

由于国内经常连接不上, 所以导致无法安装, 这里使用的是一位博主的自动脚本, 直接在终端复制即可。可以查看具体[文章](https://zhuanlan.zhihu.com/p/111014448).

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

安装过程中会让你选择镜像源, 根据需求进行选择即可.

## iTerm2

安装好homebrew之后, 可以使用其进行安装. 这个终端相比于原生的终端要好看一些.

```bash
brew cask install iterm2
```

## Oh My Zsh

由于mac自带了zsh和curl, 所以安装起来比较简单, 直接使用如下命令行安装.

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

不过我再安装的时候, 可能由于git没有配置好, 导致连接不到服务器. 所以我后来配置好之后, 直接克隆了项目再在本地运行`install.sh`文件, 从而成功安装.

其关键的插件可以查看它的配置文件, 一般在`~/.zshrc`里面, 并且更换主题也可以在这里面设置. 具体的操作和主题可以查看[官网](https://github.com/ohmyzsh/ohmyzsh). 我选择的主题是`ys`, 直接在配置文件中写入如下语句即可.

```bash
ZHE_THEME="ys"

source ~/.zshrc //这是配置完文件后让文件生效的语句
```

但为了能过正常显示所有主题, 还需要安装`Powerline fonts`字体, 这个可以按照[官网](https://github.com/powerline/fonts)的方式进行安装. 并在使用主题的时候, 在iterm2中设置对应的字体即可. 一般是将字体设置为`Meslo LG M for Powerline`即可. 

**PS: 安装了ohmyzsh之后, 之后涉及到环境变量的配置, 都需要直接配置到.zshrc文件当中, 否则在iterm2的终端中是不会起作用的**

## 设置终端代理

之前在进行`git clone`的时候由于速度过慢, 所以采用了GitHub的代理设置. 但一般的教程提供的都是GitHub的全局代理配置, 但是这样并不好, 如果git配置了全局代理会影响到`sourcetree`或者其他命令终端的git命令, 所以还是不建议进行使用.

我们可以设置终端的代理, 来克服网速过慢的情况. 并且这种代理是方便进行开关的.

终端的代理配置可以有两种方法, 一种是一次性的, 一种是永久性的.

### 一次性

```bash
export http_proxy="http://127.0.0.1:1087"
export https_proxy="http://127.0.0.1:1087"

//上述两步可以合成一步
export all_proxy="http://127.0.0.1:1087"

//也可以尝试使用scoks路径
export http_proxy="socks5://127.0.0.1:1080"
export https_proxy="socks5://127.0.0.1:1080"

//上述两步可以合成一步
export all_proxy="scoks5://127.0.0.1:1080"
```

### 永久性

通过在配置文件中的添加脚本, 从而达到永久性的代理配置. 如果使用的`ohmyzsh`, 则直接添加到`.zshrc`中并生效即可.

```bash
function proxy_on(){
	export http_proxy="http://127.0.0.1:1087"
	export https_proxy=$http_proxy
	echo -e "已开启代理"
	curl cip.cc
}

function proxy_off(){
	unset http_proxy
	unset https_proxy
	echo -e "已关闭代理"
	curl cip.cc
}
```

## GitHub代理配置(不建议使用)

由于国内环境, 所以克隆国外库的时候会奇慢无比, 这里提供一种方法来提升克隆速度.

1. 首先打开已经配置好的代理
2.  如果要设置全局代理, 可以按下发这样设置. 其中端口要根据自己的代理端口配置

```bash
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
```

3. 完成上述步骤, 就可以用git clone xxx进行下载, 但只能使用https协议.

```bash
git clone https://www.github.com/xxx/xxx.git	//https协议是有效的
git clone git@github.com:xxx/xxx.git	//ssh协议是无效的,需要另外设置
```

4. **不推荐使用全局代理**, 这样会导致国内的仓库变慢, 所以建议使用如下指令, 即只对github进行代理.

```bash
git config --global http.https://github.com.proxy https://127.0.0.1:1080
git config --global https.https://github.com.proxy https://127.0.0.1:1080
```

5. 如果希望使用ssh协议, 那么需要输入如下指令. (我用的vpn使用的就是这个)

```bash
git config --global http.https://github.com.proxy socks5://127.0.0.1:1086
git config --global https.https://github.com.proxy socks5://127.0.0.1:1086
```

## Pyenv

`Pyenv`是一款python版本管理工具, 用于不同版本的python的切换.

```bash
brew install pyenv //使用homebrew直接安装

pyenv -v //查看pyenv的版本信息

pyenv versions //查看已经安装的python版本信息, 默认显示的是system, 即系统版本

pyenv install --list //查看可安装的python版本

pyenv install 版本号 //安装指定的版本
```

安装的过程中, 可能会出现无法连接导致安装失败的情况. 这个时候需要开一下终端的代理, 从而就可以正常安装了.

安装完成还需要配置`pyenv`的环境变量, 否则无法进行版本的切换. 环境变量在`.zshrc`文件中进行设置.

```bash
export PYENV_ROOT=~/.pyenv
export PATH=$PYENV_ROOT/shims:$PATH
```

之后就可以进行版本的切换了, 具体的指令如下所示.

```bash
pyenv global 版本号 //设置全局的python版本
pyenv local 版本号 //设置当前目录的python版本
pyenv shell 版本号 //设置当前终端的python版本
```

如果需要换回到系统的版本, 则在版本号的位置直接填写`system`即可.

## 更换pip镜像源

这里更换的是豆瓣源, 更换其他源可以百度查一下, 不过豆瓣源比较稳定.

```bash
madir ~/.pip //建立.pip文件夹
cd ~/.pip/	//进入文件夹
vim pip.conf

[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host = pypi.douban.com
```

