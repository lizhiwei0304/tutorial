# <center>Qt学习笔记<cener/>
## 1.C++11 标准
时代在变化，C++ 标准也在前进。C++ 正式公布标准有 C++98、C++03、C++11。最新的 C++11 标准是2011年8月12日公布的，在公布之前该标准原名为 C++0x 。这是一次较大的修订和扩充，建议读者专门学一下。

Qt 从 4.8 版本就开始用 C++11 新特性了。编译器里面开始支持 C++11 的版本是 MSVC 2010、GCC 4.5、Clang 3.1，这之后版本的编译器都在逐步完善对 C++11 的支持，现在新版本编译器对新标准的支持都比较全面了。

Qt 官方在编译 Qt5 库的时候都是开启 C++11 特性的，如果我们要在自己项目代码启用新标准，需要在 .pro 文件里面添加一行：
```CONFIG += c++11```

如果是 Qt4 版本则是添加：
```gcc:CXXFLAGS += -std=c++0x```

MSVC 编译器默认开启 C++11 特性，GCC（g++命令）则需要自己添加选项 -std=c++0x ，上面 CXXFLAGS 就是为 GCC 编译器（g++命令）添加 -std=c++0x 选项。
## 2.新建一个qt项目
Qt Creator 可以创建多种项目，在最左侧的列表框中单击“Application”，中间的列表框中列出了可以创建的应用程序的模板，各类应用程序如下：
Qt Widgets Application，支持桌面平台的有图形用户界面（Graphic User Interface，GUI） 界面的应用程序。GUI 的设计完全基于 C++ 语言，采用 Qt 提供的一套 C++ 类库。
Qt Console Application，控制台应用程序，无 GUI 界面，一般用于学习 C/C++ 语言，只需要简单的输入输出操作时可创建此类项目。
Qt Quick Application，创建可部署的 Qt Quick 2 应用程序。Qt Quick 是 Qt 支持的一套 GUI 开发架构，其界面设计采用 QML 语言，程序架构采用 C++ 语言。利用 Qt Quick 可以设计非常炫的用户界面，一般用于移动设备或嵌入式设备上无边框的应用程序的设计。
Qt Quick Controls 2 Application，创建基于 Qt Quick Controls 2 组件的可部署的 Qt Quick 2 应用程序。Qt Quick Controls 2 组件只有 Qt 5.7 及以后版本才有。
Qt Canvas 3D Application，创建 Qt Canvas 3D QML 项目，也是基于 QML 语言的界面设计，支持 3D 画布。
## 3.项目管理文件
***
QT       += core gui

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

TARGET = samp2_1

TEMPLATE = app

SOURCES += \
        main.cpp \
        widget.cpp
        
HEADERS += \
        widget.h
        
FORMS += \
        widget.ui
***
“Qt += core gui”表示项目中加入 core gui 模块。core gui 是 Qt 用于 GUI 设计的类库模块，如果创建的是控制台（Console）应用程序，就不需要添加 core gui。

Qt 类库以模块的形式组织各种功能的类，根据项目涉及的功能需求，在项目中添加适当的类库模块支持。例如，如果项目中使用到了涉及数据库操作的类就需要用到 sql 模块，在 pro 文件中需要增加如下一行：
``` Qt +=sql ```

samp2_1.pro 中的第 2 行是：
``` greaterThan(Qt_MAJOR_VERSION, 4): Qt += widgets  ```

这是个条件执行语句，表示当 Qt 主版本大于 4 时，才加入 widgets 模块。

“TARGET = samp2_1”表示生成的目标可执行文件的名称，即编译后生成的可执行文件是 samp2_1.exe。

“TEMPLATE = app”表示项目使用的模板是 app，是一般的应用程序。

后面的 SOURCES、HEADERS、FORMS 记录了项目中包含的源程序文件、头文件和窗体文件（.ui 文件）的名称。这些文件列表是 Qt Creator 自动添加到项目管理文件里面的，用户不需要手动修改。当添加一个文件到项目，或从项目里删除一个文件时，项目管理文件里的条目会自动修改。

功能 | 快捷键   |	 解释

Switch Header/Source	F4	在同名的头文件和源程序文件之间切换
Follow Symbol Under Cursor	F2	跟踪光标下的符号，若是变量，可跟踪到变量声明的地方；若是函数体或函数声明，可在两者之间切换
Switch Between Function
Declaration and Definition	Shift+F2	在函数的声明（函数原型）和定义（函数实现）之间切换
Refactor\Rename Symbol Under Cursor	Ctrl+Shift+R	对光标处的符号更改名称，这将替换到所有用到这个符号的地方
Refactor\Add Definition in .cpp	 	为函数原型在 cpp 文件里生成函数体
Auto-indent Selection	Ctrl+I	为选择的文字自动进行缩进
Toggle Comment Selection	Ctrl+/	为选择的文字进行注释符号的切换，即可以注释所选代码，或取消注释
Context Help	F1	为光标所在的符号显示帮助文件的内容
Save All 	Ctrl+Shift+S	文件全部保存
Find/Replace	Ctrl+F	调出查找/替换对话框
Find Next	F3	查找下一个
Build	Ctrl+B	编译当前项目
Start Debugging	F5	开始调试
Step Over 	F10	调试状态下单步略过，即执行当前行程序语句
Step Into	F11	调试状态下跟踪进入，即如果当前行里有函数，就跟踪进入函数体
Toggle Breakpoint	F9 	设置或取消当前行的断点设置