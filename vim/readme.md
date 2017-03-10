# Install use_vim_as_ide
[use_vim_as_ide](https://github.com/yangyangwithgnu/use_vim_as_ide) instatll the vim as ide

This document is install this under ubuntu16.04 in vmware 11.0

## install vim with gtk2.0
when install the vim step by step. The first question is install gtk2.0

### install vim from source code

Q1: curse not install
```
checking for tgetent in -lcurses... no
no terminal library found
checking for tgetent()... configure: error: NOT FOUND!
      You need to install a terminal library; for example ncurses.
      Or specify the name of the library with --with-tlib.
```
So I install as below
```shell
sudo apt-get install libncurses5-dev
sudo apt-get install ruby-dev
sudo apt-get install libperl-dev
```
when I install lua, the below command not ok,
so use *source code* install lua [lua5.3.4.tar.gz](http://www.lua.org/ftp/lua-5.3.4.tar.gz)
```
#sudo apt-get install liblua5.3-dev 
#sudo apt-get install lua5.3 
```
Now I will install the gui configuration(--enable-gui=gtk2),
but I found use apt-get is not same as autor's description. 
so I do as below 
```
sudo apt-get install libx11-dev
#no gtk-dev
#need upate the gnome-core
apt-get install gnome-core
apt-get install pkg-config
apt-get install devhelp
apt-get install libglib2.0-doc libgtk2.0-doc
apt-get install libgtk2.0*
sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev libgtk2.0-dev libatk1.0-dev libbonoboui2-dev libcairo2-dev libx11-dev libxpm-dev libxt-dev
sudo apt-get install libgtk2.0-dev
or sudo apt-get install libgtk-3-dev
./configure --with-features=huge --enable-pythoninterp --enable-rubyinterp --enable-luainterp --with-lua-prefix=/usr/local --enable-perlinterp --with-python-config-dir=/usr/lib/python2.7/config/ --enable-gui=gtk2 --enable-cscope --prefix=/usr
```
Then I found the message out is ok.
```
checking --enable-gui argument... GTK+ 2.x GUI support
checking --disable-gtktest argument... gtk test enabled
checking for pkg-config... (cached) /usr/bin/pkg-config
checking for GTK - version >= 2.2.0... yes; found version 2.24.30
checking version of Gdk-Pixbuf... OK.
```
But unfortuntly my adjust screen in vmware is borken.


>在Ubuntu下面安装飞鸽，iptux，编译源码的时候需要使用gtk+2.0，
貌似大多数的图形界面软件都依赖于gtk+2.0，所以很有安装的必要
好像安装的步骤挺复杂的，
网上找到的文章，贴在这里留着以后备用。
apt-get install build-essential #这将安装gcc/g++/gdb/make 等基本编程工具
apt-get install gnome-core-devel #这将安装 libgtk2.0-dev libglib2.0-dev 等开发相关的库文件
apt-get install pkg-config #用于在编译GTK程序时自动找出头文件及库文件位置
apt-get install devhelp #这将安装 devhelp GTK文档查看程序
apt-get install libglib2.0-doc libgtk2.0-doc #这将安装 gtk/glib 的API参考手册及其它帮助文档
apt-get install glade libglade2-dev #这将安装基于GTK的界面GTK是开发Gnome窗口的c/c++语言图形库。
apt-get install libgtk2.0*, gtk+2.0所需的所有文件统通下载安装完毕。
应用程序编译命令：gcc test.c `pkg-config --cflags --libs gtk+-2.0`，编译通过，运行正常。
pkg-config是一个用来管理包的程序，在控制台输入 pkg-config --cflags --libs gtk+-2.0，可以发现输出的文本包括了gcc编译gtk+2.0所需要的所有选项（头文件目录和库文件）。
这里有一点需要注意， gcc test.c `pkg-config --cflags --libs gtk+-2.0`, pkg-config --cflags --libs gtk+-2.0两侧的引号并不是真正的引号，而是键盘数字件那一行，最左边的那个字符。如果错用了单引号，gcc无法使用 pkg-config --cflags --libs gtk+-2.0产生的文本作为编译选项。构造程序。 

I also use the fcitx install the sougou pingying.
```
sudo apt-get install fcitx
```

Q2: ubuntu glibc dev
Possible duplicate of How do I get the libc development libraries?
Run the command from the terminal sudo apt-get install build-essential
The build package should also install the libc6-devel package for you as a dependency.

### install clang and llvm
***install the YCM***

When I install the YCM. I follow the autor's step.
But I found I can install it fast. by YCM's script.
```
sudo apt-get install clang
sudo apt-get install libclang-dev
sudo apt-get install llvm
cd ~/.vim/bundle/YouCompleteMe
./install.py --clang-completer
```
so I think you should read the https://github.com/Valloric/YouCompleteMe 's readme



***install llvm***

you just follow the below links's step
[install llvm](http://clang.llvm.org/get_started.html)

what I do:
```
svn co http://llvm.org/svn/llvm-project/llvm/trunk llvm
cd llvm/tools
svn co http://llvm.org/svn/llvm-project/cfe/trunk clang
cd ../..
cd llvm/tools/clang/tools
svn co http://llvm.org/svn/llvm-project/clang-tools-extra/trunk extra
cd ../../../..
cd llvm/projects
svn co http://llvm.org/svn/llvm-project/compiler-rt/trunk compiler-rt
cd ../..
cd llvm/projects
svn co http://llvm.org/svn/llvm-project/libcxx/trunk libcxx
svn co http://llvm.org/svn/llvm-project/libcxxabi/trunk libcxxabi
cd ../..
mkdir build (in-tree build is not supported)
cd build
cmake -G "Unix Makefiles" ../llvm
make & make install ( will install all) 
```
because I don't known the libcxx and libcxxabi could make as same way.
my libcxx install as below:
```
cmake -G "Unix Makefiles" ../llvm
make cxx
make install-cxx install-cxxabi
```
you could find the document in libcxx/doc/BuildingLibcxx.rst

then you should update the lib links to /usr/lib as below
```
sudo cp /usr/local/lib/libc++.so* /usr/lib/
sudo cp /usr/local/lib/libc++abi.so* /usr/lib/
```
