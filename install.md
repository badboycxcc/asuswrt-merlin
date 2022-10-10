## Ubuntu16.04 编译梅林固件

#### 首先搭建编译环境，使用ubuntu16.04  64位系统。先更新一下系统软件
```
sudo apt-get update
sudo apt-get upgrade
```

#### 安装编译梅林所需要的依赖软件包
```
sudo apt-get install autoconf automake bash bison bzip2 diffutils file flex m4 \
g++ gawk groff-base libncurses-dev libtool libslang2 make patch perl pkg-config \
shtool subversion tar texinfo zlib1g zlib1g-dev git-core gettext libexpat1-dev \
libssl-dev cvs gperf unzip python libxml-parser-perl gcc-multilib gconf-editor \
libxml2-dev g++-5 g++-multilib gitk libncurses5 mtd-utils libncurses5-dev \
libvorbis-dev g++-5-multilib git autopoint autogen sed lib32z1-dev lib32stdc++6 \
build-essential intltool libelf1:i386 libglib2.0-dev xutils-dev libltdl7-dev libproxy-dev
```

#### 从github获取梅林源代码，以下所有相关操作都在当前用户根目录进行， 输入 cd 然后回车可直接进入当前用户根目录

```
git clone https://github.com/RMerl/asuswrt-merlin.git
```

需要下载2GB左右的数据，需要等的时间有点久

#### 下载完以后将文件链接到指定目录
```
sudo ln -s ~/asuswrt-merlin/tools/brcm /opt/brcm
sudo ln -s ~/asuswrt-merlin/release/src-rt-6.x.4708/toolchains/hndtools-arm-linux-2.6.36-uclibc-4.5.3 /opt/brcm-arm
sudo mkdir -p /media/ASUSWRT/
sudo ln -s ~/asuswrt-merlin /media/ASUSWRT/asuswrt-merlin
```
声明环境变量，直接执行，也可添加到.bashrc文件中(添加后需要执行source ~/.bashrc)

```
export PATH=$PATH:/opt/brcm/hndtools-mipsel-linux/bin:/opt/brcm/hndtools-mipsel-uclibc/bin:/opt/brcm-arm/bin
```

先解决一下编译的时候会缺失文件的问题
```
cp /usr/include/proxy.h ~/asuswrt-merlin/release/src/router/neon/
cd ~/asuswrt-merlin/release/src/router/libdaemon
aclocal
cd ~/asuswrt-merlin/release/src/router/libxml2
sed -i s/AM_C_PROTOTYPES/#AM_C_PROTOTYPES/g ~/asuswrt-merlin/release/src/router/libxml2/configure.in
aclocal
```

以AC-68U为例，进入相关目录编译
```
cd ~/asuswrt-merlin/release/src-rt-6.x.4708
make rt-ac68u
```
如果编译出问题需要重头再来，建议解决问题以后先执行 
```
make clean
```
