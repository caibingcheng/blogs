# cbuild-一个创建和管理C++项目的工具

### 介绍：

这是个人开发的一个管理C++项目的工具，用shell脚本编写。

可能会不定期更新，也**欢迎大家一起完善**。

当前开发版本0.1。个版本功能如下：

0.0	--	初始版本，具备创建、删除、编译、运行项目基本功能

0.1	--	在0.0版本基础上使用模板文件，方便用户定制自我需求



### 使用方法：

github地址：https://github.com/caibingcheng/cbuild

gitee地址：https://gitee.com/jerry323/cbuild

github和gitee中该项目是同步的。

#### 安装：

clone项目到本地，进入项目根目录。

```shell
git clone git@github.com:caibingcheng/cbuild.git ~/cbuild
cd ~/cbuild
```

运行install.sh脚本。

```shell
sh ./install.sh	#普通安装
sh ./install.sh -f #强制安装，用于重新安装或者添加新功能
```

安装后，工具包会安装在~/.cbuild下，进入目录，其中template模块包含的是生成项目时一些文件的默认内容，如有需要可以自行修改其中的内容。

**注意：**在~/.cbuild/template/CMakeLists.txt文件中包含{\_\_CBUILD_PROJECT\_\_}字段，这是工具自定义的，不可修改。

#### 使用：

可用命令：

```shell
cbuild -c [project] #创建项目
cbuild -r [project] #删除项目
cbuild -b [project] #编译项目
cbuild -e [project] #运行项目
cbuild -h           #显示帮助
```

在第一次管理项目时，不能省略项目名称[project]参数。

此后可以省略项目名称[project]参数，该工具内部会自动补全为最近一次使用的项目。

**注意：**项目名称[project]应该只包含项目名称且合法，不应该包含路径符号和其他非法符号（'\', '/', '?', etc.）。这是不允许的。



### 项目树：

```
project

  |--bin

       |--#binara file(executable file)

  |--build

       |--#MakeFile

  |--src

       |--main.cpp    

  |--include

       |--#include file

  |--lib

       |--#libs

  |--CMakeLists.txt

```

