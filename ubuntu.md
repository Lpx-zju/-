# Ubuntu18.04

## 系统安装

**首先选择英文进行安装**, 否则之后会由于选择中文而导致分辨率出现问题, 从而有些选项不显示.

- 选择`Minimal Installation`
- `Other options`中不选择任何选项, 不要下载更新, 一个是安装的过程会很慢, 另一个也会出现未知的问题
- 安装的时候不要选择情况磁盘进行安装, 最好选择`Something else`

## 虚拟机(100G)

- `/`: 30G
- `swap`: 8G
- `/home`: 剩余部分

由于是在虚拟机中当中, 所以我就没有给启动设置分区(也就是efi分区), 因为不太需要. 虚拟机当中就是怎么简单怎么来, 毕竟只是作为练习用的. 那对应的区间分配好即可了.

在不设置`efi`分区的情况下, 虚拟机是通过`boot`的方式进行启动的. 不过现在的`Ubuntu`已经不需要分配`/boot`区了.

> 对于virtual box, 刚安装完是无法进行全屏显示的. 这个时候需要选择【设备】->【安装增强功能】, 将功能安装完成之后, 就可以正常全屏显示了.

### 设置共享文件夹

在虚拟机中可以设置和win系统的共享文件夹, 从而不用每次开机都进行挂载.

直接在`共享文件夹`上新建一个`share`文件夹, 选择`自动挂载`和`固定分配`, 从而将共享文件夹创建完成.

之后打开`Ubuntu`终端, 进入到`/mnt`下面, 输入如下指令进行挂载.

```bash
sudo mount -t vboxsf share /mnt/share
```

## [更换安装源](https://blog.csdn.net/zhangjiahao14/article/details/80554616)

我们需要修改的是`/etc/apt`下的`sources.list`文件, 这个文件是包管理工具apt所用的记录软件包仓库位置的配置文件.

首先我们需要将该文件进行备份, 以防万一, 后缀为`bak`的文件即是备份文件.

```bash
sudo cp /etc/apt/sources.list /etc/apt/source.list.bak
```

编辑该文件, 将其中有效的语句全部注释掉, 并添加上如下的语句.

```python
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
```

可以发现这种语句都是具有规律的, 其中是两个为一组, 具体的规律如下所示.

```
deb 源链接 系统代号 组件1 组件2 组件3
deb-src 源链接 系统代号 组件1 组件2 组件3
```

其中系统代号不同版本的`Ubuntu`是不同的, 我们可以使用==lsb_release -c==语句进行查看, `18.04`的系统代号为`bionic`, 从而以后针对不同的版本, 只要进行对应的修改即可.

之后的组件目前使用的是`main`, `restricted`, `universe`, `multiverse`.

修改好文件之后进行保存, 在进行**素质二连**即可.

## SSH连接

安装ssh服务

```bash
sudo apt-get install openssh-server
```

启动ssh服务, 并进行确认

```bash
sudo /etc/init.d/ssh start
ps -e | grep ssh
```

之后就可以通过22端口远程连接该电脑

**虚拟机**中需要将网络连接的方式改成`桥接模式`.

```bash
sudo netstat -tlpn #查看开启的端口, 不考虑防火墙
```

## 防火墙设置

`Ubuntu`默认的不启用的防火墙的, 所以不是很安全. 这里最好对防火墙进行设置.

```bash
sudo ufw enable #开启防火墙

sudo ufw reload #重启防火墙

sudo ufw status #查看防火墙状态, 以及开启的端口

sudo ufw allow 22 #开启22端口
```

开启防火墙之后, 之前可以访问的端口就无法访问了, 因此我们需要在防火墙的基础上将端口打开, 从而又可以进行连接了. **需要注意, 首先需要在内部将端口打开, 之后再设置防火墙上打开**, 如果内部就没有打开端口的话, 那是无法使用的.

## zsh and ohmyzsh

`Ubuntu`和`Mac`是不同的, 自带的shell不是`zsh`而是`bash`, 因此需要先安装`zsh`, 再设置`ohmyzsh`.

```bash
echo $SHELL #查看当前使用的shell
cat /etc/shells #查看系统拥有的shell
```

安装`zsh`

```bash
sudo apt-get install zsh
```

将`zsh`配置成默认的shell, 重启之后即生效

```bash
chsh -s /bin/zsh
```

安装`ohmyzsh`可以参考`mac`文档中的教程, 基本步骤是一样的.

## 安装miniconda

miniconda相对于anaconda更加精简, 只安装了conda包管理器已经python和对应支持的库. 比较适用于虚拟机中进行使用, 方便自己进行配置. 安装的方法比较简单. 安装之后需要更换掉镜像源, 从而加快下载的速度.

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

通过下面的命令来查看镜像源是否已经更改.

```bash
conda config --show channels
```

下面是关于conda的一些操作指令

```bash
conda install packagename #安装对应的包
conda install numpy=1.10 #安装对应的版本

conda upgard --all #升级全部库
conda upgard packagename #升级对应的包

conda remove packagename #移除一个包

conda list #查看所有安装的包

conda info -envs #查看现有的环境
conda create -n env_name python=3 #创建一个叫env_name的环境, 并设置python版本为3
source activate env_name #激活环境
source deactivate env_name #退出环境
conda env remove -n env_name #删除虚拟环境
```

在安装pytorch库的使用, 在更换了conda源之后, 其官方的安装语句中不要加上`-c pytorch`, **因为加上的话使用的还是官方的路径, 这样下载会很慢**.

## 安装搜狗输入法

[参考](https://www.cnblogs.com/tyty-Somnuspoppy/p/10007694.html)

