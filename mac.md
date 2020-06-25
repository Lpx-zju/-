# Mac编程配置清单

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

但为了能过正常显示所有主题, 还需要安装`Powerline fonts`字体, 这个可以按照[官网](https://github.com/powerline/fonts)的方式进行安装. 并在使用主题的时候, 在iterm2中设置对应的字体即可. 一般是将字体设置为`Meslo LG M for Powerline`即可.  之后还可以为其配置[语法高亮插件](https://github.com/zsh-users/zsh-syntax-highlighting). 还有一些好用的[插件](https://www.jianshu.com/p/ba782b57ae96)

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

## 修正Github网页无法显示图片的问题

通常我们开启代理之后，就可以流畅访问`google`，`github`等网页。但是在`github`上，有的时候无法正常的显示图片。因此我们需要修改对应的文件来解决这个问题。

当我们发现某个图片无法正常显示，我们可以通过`右键点击`，变选择`检查`，从而我们可以在编码界面看到对应图片的地址，如`raw.githubcontent.com`。此时我们可以通过[网站](https://githubusercontent.com.ipaddress.com/avatars0.githubusercontent.com)查看其对应的`ip`, 并将这条记录添加到==/private/etc/hosts==文件中，下面是我添加的一个样例：

```bash
# Github
140.82.114.4 githbu.com
140.82.113.4 gist.github.com

199.232.68.133 raw.githubusercontent.com
199.232.68.133 camo.githubusercontent.com
199.232.68.133 cloud.githubusercontent.com
199.232.68.133 images.githubusercontent.com
199.232.68.133 avatars0.githubusercontent.com
199.232.68.133 avatars1.githubusercontent.com
199.232.68.133 avatars2.githubusercontent.com
199.232.68.133 avatars3.githubusercontent.com
```

所以，只要我们发现有图片无法加载了，我们就可以通过查找它的`ip`来添加记录，从而刷新网页即可。**但这个ip并不是一直不变的，当发生变化之后我们之前设置的记录就没有用了，此时需要我们更新ip值**。

## 安装miniconda

通过安装`miniconda`就可以帮助我们处理好所有的python环境，还可以进行包管理。同时相比于`anaconda`，这个更加轻量化。具体的信息可以查看`ubuntu`中的记录。

## 安装CLion

这个是为了写`C++`的程序，因为有学生认证，所以可以免费进行使用。

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



# 使用软件清单

- IINA：一款使用的本地播放器
- CheatSheet：快速查看快捷键的插件
- Spectacle：帮助进行分屏的插件
- office：通过学校的资源进行安装
- eZip：好用的压缩软件
- Typora：markdow编辑器
- Xmind：思维导图