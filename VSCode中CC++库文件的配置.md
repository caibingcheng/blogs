> VSCode中C/C++库文件的配置



之前一直在是用sublime做主要编辑器，现在主要使用VSCode，毕竟大厂制作，从目前的使用情况来看，我更喜欢使用VSCode编辑器。

有时候会用VScode来组件C/C++工程，并且用到了一些外部依赖的库文件，比如OpenCV。此时希望VSCode的代码提示功能能够提示OpenCV中的函数，这时候就需要配置工作空间中的C/C++编译环境。

如果你使用过sublime或者VSCode，就知道Ctrl+Shift+P可以调出控制窗口，那么先按下Ctrl+Shift+P：

![](/home/jerry/Pictures/Screenshot from 2018-06-12 19-54-33.png)

再输入edit或者configuration，选择"C/Cpp:Edit Configurations"：

![](/home/jerry/Pictures/Screenshot from 2018-06-12 19-55-31.png)

之后会在你的工作空间生成./.vscode/c_cpp_properties.json文件，我们需要做的就是配置这个文件的参数；

在“includePath”的属性中添加你的库文件的地址就行了，类似：

```json
                "/home/**/Tools/opencv/cpu-install/include",
                "/home/**/Tools/opencv/gpu-install/include"
```

看截图：

![](/home/jerry/Pictures/Screenshot from 2018-06-12 19-58-43.png)

配置好后，最好重启一下客户端，VSCode就也会给你自动提示opencv库中的函数或类等等的了。