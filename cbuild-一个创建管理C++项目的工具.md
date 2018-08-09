# cbuild-一个创建和管理C++项目的工具

### 介绍：

这是个人开发的一个管理C++项目的工具，用shell脚本编写。

可能会不定期更新，也**欢迎大家一起完善**。

当前开发版本0.5。各版本功能如下：

- version 0.0	--	初始版本，具备创建、删除、编译、运行项目基本功能
- version 0.1	--	在0.0版本基础上使用模板文件，方便用户定制自我需求
- version 0.5    --      重构代码，优化参数选项

 

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
cbuild -a project #添加项目
cbuild -c project #创建项目
cbuild -r project #删除项目
cbuild -b project #编译项目
cbuild -e project #运行项目
cbuild -f		  #打开强制开关，可用于强制生成（覆盖）项目、强制编译\移除\运行项目
cbuild -h         #显示帮助

cbuild -R         #移除当前项目
cbuill -B         #编译当前项目
cbuild -E         #运行当前项目

cbuild uninstall  #卸载工具
```

大写选项不需要添加参数，默认使用当前项目；比如最近生成的项目或使用-a选项添加的项目。

强制开关打开后造成的项目覆盖破坏是难以逆转的，请合理使用。

**注意：**项目名称project应该只包含项目名称且合法，不应该包含路径符号和其他非法符号（'\', '/', '?', etc.）。这是不允许的。



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

