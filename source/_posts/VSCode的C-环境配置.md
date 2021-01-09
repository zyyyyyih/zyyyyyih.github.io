---
title: VSCode的C++环境配置
tags:
  - VSCode
  - C++
categories: 工具
abbrlink: 9905827f
date: 2020-10-07 18:18:45
---
# VSCode简介
[VSCode](https://code.visualstudio.com/)[是一个纯为本**编辑器**，不是ide（集成开发环境），所以需要给他配置**编译器**才可以发挥功能
![](https://i.niupic.com/images/2020/10/08/8OdY.png)
选择对应的Stable版本即可（本文仅针对Windows）

# 环境准备
先设置成熟悉的中文，在商店里搜索`chinese`，安装即可
同样的，搜索`C/C++`，安装
![](https://i.niupic.com/images/2020/10/08/8Oeu.png)

转到工作区（侧边栏圈起来的那个），然后点击打开文件夹，我新建一个了test文件夹在我的E盘（请根据你的习惯进行这一步操作）
![](https://i.niupic.com/images/2020/10/08/8Oev.png)
现在这个文件夹是空的，啥都没有。
我们先不要动这里

下面要有所选择
## 第一种：C/C++编译器TDM-GCC的安装与配置（本人推荐）
[TDM-GCC_64官网](https://jmeubank.github.io/tdm-gcc/)
![](https://i.niupic.com/images/2020/10/08/8OgY.png)
我直接搬运下载直连
[官网的下载直连](https://github.com/jmeubank/tdm-gcc/releases/download/v9.2.0-tdm64-1/tdm64-gcc-9.2.0.exe)

选择`Create`后面全部默认
![](https://i.niupic.com/images/2020/10/08/8OgV.png)
![](https://i.niupic.com/images/2020/10/08/8OgW.png)
![](https://i.niupic.com/images/2020/10/08/8OgX.png)
### 一般会直接配置好环境变量，可以自行检查，否则手动配置


# VSCode的配置文件
回到VSCode
创建一个你打算存放代码的文件夹，称作工作区文件夹；路径不能含有中文和引号，最好不要有空格，我用的是`E:\test`之前创建过了。（C和C++需要分别建立不同的文件夹，除非用虚拟工作区。不要选上一节存放编译器的文件夹，源代码和编译器要分开放。这个先别管。）

打开VSCode，选打开文件夹，点新建文件夹，名称为`.vscode`。不在资源管理里新建的原因是Windows的Explorer不允许创建的文件夹第一个字符是点（1903后才支持）。
手动创建如下文件，变成这样的效果：
![](https://i.niupic.com/images/2020/10/07/8Mv1.png)
我们先写一个`.cpp`的文件（新建一个平行于`.vscode`的文件夹），比如：
![](https://i.niupic.com/images/2020/10/08/8Ofw.png)
然后`crtl`+`s`保存，先放在那里不管

配置每个配置文件：（为了方便小白，直接抄我的就好了）
- 编辑`c_cpp_properties.json`
```json
{
    "configurations": [
        {
            "name": "Win32",
            "intelliSenseMode": "clang-x64",
            "includePath": [
                "${workspaceFolder}",
                "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include/c++",
                "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include/c++/x86_64-w64-mingw32",
                "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include/c++/backward",
                "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include",
                "C:/TDM-GCC-64/x86_64-w64-mingw32/include",
                "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include-fixed"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "__GNUC__=7",
                "__cdecl=__attribute__((__cdecl__))"
            ],
            "browse": {
                "path": [
                    "${workspaceFolder}",
                    "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include/c++",
                    "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include/c++/x86_64-w64-mingw32",
                    "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include/c++/backward",
                    "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include",
                    "C:/TDM-GCC-64/x86_64-w64-mingw32/include",
                    "C:/TDM-GCC-64/lib/gcc/x86_64-w64-mingw32/5.1.0/include-fixed"
                ]
            },

            "compilerPath": "C:\\TDM-GCC-64\\bin\\gcc.exe",
            "cStandard": "c11",
            "cppStandard": "c++17"
        }
    ],
    "version": 4
}
```
- 编辑`tasks.json`
```json
{
	"version": "2.0.0",
	"command": "g++",
	"args": [
	  "-g",
	  "${file}",
	  "-o",
	  "${file}.exe",
	  "-fexec-charset=gbk",
	  "-finput-charset=GBK"
	],
	"problemMatcher": {
	  "owner": "cpp",
	  "fileLocation": [
		"relative",
		"${workspaceRoot}"
	  ],
	  "pattern": {
		"regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
		"file": 1,
		"line": 2,
		"column": 3,
		"severity": 4,
		"message": 5
	  }
	}
  }
```

- 编辑`launch.json`
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "C++ Launch (GDB)",
            "type": "cppdbg",
            "request": "launch",
            "launchOptionType": "Local",
            "targetArchitecture": "x86",
            "program": "${file}.exe",
            "miDebuggerPath": "C:/TDM-GCC-64/bin/gdb.exe", //路径，根据自己TDM安装位置
            "args": [
                "blackkitty",
                "1221",
                "# #"
            ],
            "stopAtEntry": false,
            "cwd": "${workspaceRoot}",
            "externalConsole": true,
            "preLaunchTask": "g++"
        }
    ]
}
```

- 编辑`settings.json`
```json
{
    "C_Cpp.errorSquiggles": "Enabled",
    "files.associations": {
        "cstdio": "cpp",
        "iostream": "cpp",
        "array": "cpp",
        "atomic": "cpp",
        "*.tcc": "cpp",
        "bitset": "cpp",
        "cctype": "cpp",
        "cfenv": "cpp",
        "chrono": "cpp",
        "cinttypes": "cpp",
        "clocale": "cpp",
        "cmath": "cpp",
        "complex": "cpp",
        "condition_variable": "cpp",
        "csetjmp": "cpp",
        "csignal": "cpp",
        "cstdarg": "cpp",
        "cstddef": "cpp",
        "cstdint": "cpp",
        "cstdlib": "cpp",
        "cstring": "cpp",
        "ctime": "cpp",
        "cwchar": "cpp",
        "cwctype": "cpp",
        "deque": "cpp",
        "forward_list": "cpp",
        "list": "cpp",
        "unordered_map": "cpp",
        "unordered_set": "cpp",
        "vector": "cpp",
        "exception": "cpp",
        "fstream": "cpp",
        "functional": "cpp",
        "future": "cpp",
        "initializer_list": "cpp",
        "iomanip": "cpp",
        "iosfwd": "cpp",
        "istream": "cpp",
        "limits": "cpp",
        "memory": "cpp",
        "mutex": "cpp",
        "new": "cpp",
        "ostream": "cpp",
        "numeric": "cpp",
        "ratio": "cpp",
        "scoped_allocator": "cpp",
        "sstream": "cpp",
        "stdexcept": "cpp",
        "streambuf": "cpp",
        "system_error": "cpp",
        "thread": "cpp",
        "type_traits": "cpp",
        "tuple": "cpp",
        "typeindex": "cpp",
        "typeinfo": "cpp",
        "utility": "cpp",
        "valarray": "cpp"
    },
    "code-runner.runInTerminal": true,
    "code-runner.saveFileBeforeRun": true,
    "code-runner.clearPreviousOutput": true,
    "code-runner.ignoreSelection": true,
    "code-runner.preserveFocus": false
}
```

# 安装code runner插件
商店搜索安装即可
`ctrl`+`alt`+`n`可以编译并运行，在下方的终端进行数据的输入（集成黑框）
现在可以愉快的玩耍了
# DEBUG的使用（调试）
暂时咕咕咕

# 额外的扩展
暂时咕咕咕
![](https://i.niupic.com/images/2020/10/08/8Ohd.jpg)


## 第二种：C/C++编译器MINGW的安装与配置（网上大多数教程是这个）
### 1.安装
[MINGW在这里下载](https://osdn.net/projects/mingw/releases/)
然后如图
![](https://p3-tt.byteimg.com/origin/pgc-image/7544a762b639436285f1df3450235e3e?from=pc)

打开下载好的`.exe`文件
![](https://p3-tt.byteimg.com/origin/pgc-image/9361b1b671b647c2960b326fe2eec522?from=pc)
可以自行更改安装目录，但是这个东西不大，使用频率也高，我直接装默认C盘
![](https://i.niupic.com/images/2020/10/08/8Oey.png)
进度条走完后点击continue，转到如下界面
如图所示`Basic Setup`->`mingw-gcc-g++`->`Mark for Installation`，这一步是选择gcc/g++编译器选项，必须勾选这一项。
![](https://i.niupic.com/images/2020/10/08/8OeH.png)
然后再如图安装`mingw32-gdb`，这个是用于`debug`的**调试器**，装一下吧，可以用到
![](https://i.niupic.com/images/2020/10/08/8OeG.png)
然后点击菜单栏`Installation`，在下拉框中选择`Apply Changes`。然后`Apply`
![](https://i.niupic.com/images/2020/10/08/8OeF.png)
然后等几分钟
效果如下，在安装目录中找到如下文件（不要管我的皮肤）
![](https://i.niupic.com/images/2020/10/08/8Of5.png)
### 2.配置环境变量
为了让系统识别到你的编译器，你还需要配置环境变量。方法如下：

1. 右击`此电脑`，选择`属性`
2. 在弹出的窗口中选择`高级系统设置`
3. 在弹出的`系统属性`窗口中，选择`高级`、`环境变量`
4. 在弹出的`环境变量`窗口中，找到系统变量，找到`path`变量，点击`编辑按钮`，将g++/gcc/gdb所在目录的路径（例如，我的路径就是`C:\MinGW\bin`）添加到`path`变量中（win7用分号隔开，然后输入路径）
![](https://i.niupic.com/images/2020/10/08/8Of6.png)
### 3.测试
在命令行窗口中输入`gcc -v` `g++ -v` `gdb -v`命令，回车执行。如果你看到返回的gcc版本信息，就成功了。如图：（不一定和我一样，反正不出现红色的报错信息就是ok了）
![](https://i.niupic.com/images/2020/10/08/8Ofb.png)

~~什么你不会打开cmd？？ `win`+ `r` 然后输入`cmd`，然后回车~~
## 第二种的配置文件（我还没研究出来稳定的版本，咕咕咕）