#  <center>CMake教程</center>
##语法
###1.PROJECT
`PROJECT(projectname  [CXX] [C] [Java])`

* 默认情况表示支持所有语言。
* 预定义了`PROJECT_BINARY_DIR，PROJECT_SOURCE_DIR`

###2.SET 指令
`SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])`

###3.MESSAGE 指令
`MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] "message to display"...)`

*  SEND_ERROR，产生错误，生成过程被跳过。
* STATUS，输出前缀为—的信息。
* FATAL_ERROR，立即终止所有 cmake 过程

###4. 变量

* 1.变量使用${}方式取值，但是在 IF 控制语句中是直接使用变量名
* 2.指令(参数 1 参数 2...)
* 3.参数使用括弧括起，参数之间使用空格或分号分开。
* 4.指令是大小写无关的，参数和变量是大小写相关的。推荐你全部使用大写指令
###5.语法的疑惑
* 1.SET(SRC_LIST main.c)也可以写成 SET(SRC_LIST “main.c”)
- 2.假设一个源文件的文件名是 fu nc.c(文件名中间包含了空格)。这时候就必须使用双引号
###6.清理工程
`make clean`

* 既可对现有构建的工程清理
* 但是cmake 不支持`make disclean`
###7.内部构建与外部构建
*   通过建立新的build文件进行`cmake ..`来实现构建称为外部构建，不会对源码有任何影响
* 内部构建已经就是直接`cmake ..`
###8.ADD_SUBDIRECTORY 指令
`ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])`

*  这个指令用于向当前工程添加存放源文件的子目录
* 可以指定中间二进制和目标二进制存放的位置
- EXCLUDE_FROM_ALL 参数的含义是将这个目录从编译过程中排除
###9.SUBDIRS 指令

- 这个指令已经不推荐使用
- 子目录体系仍然会被保存
###10.换个地方保存目标二进制

- `SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)`
- `SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)`
- 可执行二进制的输出路径为 build/bin 和库的输出路径为 build/lib
###11.make install

- `make install`用于安装make好的文件
- `configure`配置文件时，用`./configure -prefix=/usr`或者`./configure - -prefix=/usr`来确定安装的位置。
- 而CMake中，我们采用`CMAKE_INSTALL_PREFIX`来指定文件的安装位置
- 
          INSTALL(TARGETS targets...
                 [[ARCHIVE|LIBRARY|RUNTIME]
                 [DESTINATION <dir>]
                 [PERMISSIONS permissions...]
	             [CONFIGURATIONS
	             [Debug|Release|...]]
                 [COMPONENT <component>]
                 [OPTIONAL]
                 ] [...])
- TARGETS -- 目标文件：  可执行二进制、动态库、静态库
- ARCHIVE 特指静态库，LIBRARY 特指动态库，RUNTIME特指可执行目标二进制
- DESTINATION 定义了安装的路径
- 
          INSTALL(TARGETS myrun mylib mystaticlib
         		  RUNTIME DESTINATION bin
          		  LIBRARY DESTINATION lib
         	      ARCHIVE DESTINATION libstatic
           )
- 可执行二进制 myrun 安装到${CMAKE_INSTALL_PREFIX}/bin 目录
- 动态库 libmylib 安装到${CMAKE_INSTALL_PREFIX}/lib 目录
- 静态库 libmystaticlib 安装到${CMAKE_INSTALL_PREFIX}/libstatic 目录
- 你不需要关心 TARGETS 具体生成的路径，只需要写上 TARGETS 名称就可以
了
### 12.普通文件的安装
       INSTALL(FILES files... DESTINATION <dir>
              [PERMISSIONS permissions...]
              [CONFIGURATIONS [Debug|Release|...]]
              [COMPONENT <component>]
              [RENAME <name>] [OPTIONAL])
 - 可用于安装一般文件，并可以指定访问权限，文件名是此指令所在路径下的相对路径
 - OWNER_WRITE, OWNER_READ, GROUP_READ,和 WORLD_READ，即 644 权限
###13.非目标文件的安装
		INSTALL(PROGRAMS files... DESTINATION <dir>
                [PERMISSIONS permissions...]
                [CONFIGURATIONS [Debug|Release|...]]
                [COMPONENT <component>]
                [RENAME <name>] [OPTIONAL])
- 跟上面的 FILES 指令使用方法一样，唯一的不同是安装后权限为:
OWNER_EXECUTE, GROUP_EXECUTE, 和 WORLD_EXECUTE，即 755 权限
###14.目录的安装
        INSTALL(DIRECTORY dirs... DESTINATION <dir>
                [FILE_PERMISSIONS permissions...]
                [DIRECTORY_PERMISSIONS permissions...]
                [USE_SOURCE_PERMISSIONS]
                [CONFIGURATIONS [Debug|Release|...]]
                [COMPONENT <component>]
                [[PATTERN <pattern> | REGEX <regex>]
                [EXCLUDE] [PERMISSIONS permissions...]] [...])
` DIRECTORY `后面连接的是所在 Source 目录的相对路径，但务必注意abc 和 abc/有很大的区别。

-  如果目录名不以/结尾，那么这个目录将被安装为目标路径下的 abc
- 如果目录名以/结尾，代表将这个目录中的内容安装到目标路径，但不包括这个目录本身。
- PATTERN 用于使用正则表达式进行过滤，PERMISSIONS 用于指定 PATTERN 过滤后的文件权限。
- 例子

            INSTALL(DIRECTORY icons scripts/ DESTINATION share/myproj
                    PATTERN "CVS" EXCLUDE
                    PATTERN "scripts/*"
                    PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ
                    GROUP_EXECUTE GROUP_READ)
 这条指令的执行结果是：
将 icons 目录安装到 <prefix>/share/myproj，将 scripts/中的内容安装到<prefix>/share/myproj
不包含目录名为 CVS 的目录，对于 scripts/*文件指定权限为 OWNER_EXECUTE
OWNER_WRITE OWNER_READ GROUP_EXECUTE GROUP_READ
###15.ADD_LIBRARY指令

         ADD_LIBRARY(libname [SHARED|STATIC|MODULE]
                             [EXCLUDE_FROM_ALL]
                             source1 source2 ... sourceN)
         
- SHARED，动态库
- STATIC，静态库

- MODULE，在使用 dyld 的系统有效，如果不支持 dyld，则被当作 SHARED 对待
- EXCLUDE_FROM_ALL 参数的意思是这个库不会被默认构建，除非有其他的组件依赖或者手工构建。
- 这个指令只能生成名字相同的一个静态或动态库，而不能同时生成
###16.SET_TARGET_PROPERTIES GET_TARGET_PROPERTY指令
      SET_TARGET_PROPERTIES(target1 target2 ...
                            PROPERTIES prop1 value1
                            prop2 value2 ...)
  
 - 这条指令可以用来设置输出的名称，对于动态库，还可以用来指定动态库版本和 API 版本

- 与他对应的指令是：

        GET_TARGET_PROPERTY(VAR target property)
    
- 具体用法如下例，我们向 lib/CMakeListst.txt 中添加：

         GET_TARGET_PROPERTY(OUTPUT_VALUE hello_static OUTPUT_NAME)
     
        MESSAGE(STATUS “This is the hello_static OUTPUT_NAME:”${OUTPUT_VALUE})
     

如果没有这个属性定义，则返回 NOTFOUND

-  动态库版本号

         SET_TARGET_PROPERTIES(hello PROPERTIES VERSION 1.2 SOVERSION 1)

- VERSION 指代动态库版本，SOVERSION 指代 API 版本。
###17.安装共享库

        INSTALL(TARGETS hello hello_static
                LIBRARY DESTINATION lib
                ARCHIVE DESTINATION lib)
                
        INSTALL(FILES hello.h DESTINATION include/hello)

- 静态库要使用 ARCHIVE 关键字

###18.使用外部共享库和头文件
    INCLUDE_DIRECTORIES([AFTER|BEFORE] [SYSTEM] dir1 dir2 ...)
- 这条指令可以用来向工程添加多个特定的头文件搜索路径
- CMAKE_INCLUDE_DIRECTORIES_BEFORE，通过 SET 这个 cmake 变量为 on，可以将添加的头文件搜索路径放在已有路径的前面
- 通过 AFTER 或者 BEFORE 参数，也可以控制是追加还是置前

LINK_DIRECTORIES 和 TARGET_LINK_LIBRARIES指令

     LINK_DIRECTORIES (directory1 directory2 ...)
- 这个指令非常简单，添加非标准的共享库搜索路径

        TARGET_LINK_LIBRARIES(target library1
                              <debug | optimized> library2
                              ...)
                              
 -  这个指令可以用来为 target 添加需要链接的共享库
     
        TARGET_LINK_LIBRARIES (main hello)  或者   TARGET_LINK_LIBRARIES(main libhello.so)
                   
- 如何链接到静态库

        TARGET_LINK_LIBRARIES(main libhello.a)
 
### 19.CMAKE_INCLUDE_PATH 和 CMAKE_LIBRARY_PATH
- 务必注意，这两个是环境变量而不是 cmake 变量
- 使用方法是要在 bash 中用 export 或者在 csh 中使用 set 命令设置或者CMAKE_INCLUDE_PATH=/home/include cmake ..等方式。
- 通过 INCLUDE_DIRECTORIES 指令加入非标准的头文件搜索路径
- 通过 LINK_DIRECTORIES 指令加入非标准的库文件搜索路径
- 通过 TARGET_LINK_LIBRARIES 为库或可执行二进制加入库链接

###20.Cmake变量
- cmake变量的取值：${}表示取值，但是在IF（）中直接用变量名
- 显示定义：set（HELLO_SRC main.c）
- 这样就定义了HELLO_SRC这个变量
- 隐式定义： 例如PROJECT（）就会定义<projectname>_BINARY_DIR 和<projectname>_SOURCE_DIR 
- 常用变量
     
         CMAKE_BINARY_DIR
         
         PROJECT_BINARY_DIR
       
         <projectname>_BINARY_DIR
  - 这三个变量指代的内容是一致的
  - 如果是 in source 编译，指得就是工程顶层目录
  - 如果是 out-of-source 编译，指的是工程编译发生的目录
  - PROJECT_BINARY_DIR 跟其他指令稍有区别，现在，你可以理解为他们是一致的
  
- 
     CMAKE_SOURCE_DIR
      
         PROJECT_SOURCE_DIR
    
         <projectname>_SOURCE_DIR
     - 这三个变量指代的内容是一致的，不论采用何种编译方式，都是工程顶层目录
     - 也就是在 in source 编译时，他跟 CMAKE_BINARY_DIR 等变量一致。
     - PROJECT_SOURCE_DIR 跟其他指令稍有区别，现在，你可以理解为他们是一致的
  
- 
     CMAKE_CURRENT_SOURCE_DIR
     
    - 指的是当前处理的 CMakeLists.txt 所在的路径，比如上面我们提到的 src 子目录
    
- CMAKE_CURRRENT_BINARY_DIR
     
     - 如果是 in-source 编译，它跟 CMAKE_CURRENT_SOURCE_DIR 一致
     - 如果是 out-ofsource 编译，他指的是 target 编译目录。
     - 使用我们上面提到的 ADD_SUBDIRECTORY(src bin)可以更改这个变量的值
     - 使用 SET(EXECUTABLE_OUTPUT_PATH <新路径>)并不会对这个变量造成影响，它仅仅修改了最终目标文件存放的路径
     
- 
      CMAKE_CURRENT_LIST_FILE
      - 输出调用这个变量的 CMakeLists.txt 的完整路径
      
 - 
      CMAKE_CURRENT_LIST_LINE
       - 输出这个变量所在的行

-  
      CMAKE_MODULE_PATH
      - 这个变量用来定义自己的 cmake 模块所在的路径
      - 你需要通过 SET 指令，将自己的 cmake 模块路径设
置一下，比如
      -  
        
      		SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
            
- 
      EXECUTABLE_OUTPUT_PATH 和 LIBRARY_OUTPUT_PATH
      - 分别用来重新定义最终结果的存放目录，前面我们已经提到了这两个变量
   
- 
      PROJECT_NAME
      - 返回通过 PROJECT 指令定义的项目名称。

### 20.cmake调用系统变量
使用 `$ENV{NAME} `指令就可以调用系统的环境变量了。比如
        
        MESSAGE(STATUS “HOME dir: $ENV{HOME}”)
 设置系统变量的值
 
       SET(ENV{变量名} 值)
1.CMAKE_INCLUDE_CURRENT_DIR

自动添加 CMAKE_CURRENT_BINARY_DIR 和CMAKE_CURRENT_SOURCE_DIR 到当前处理的 CMakeLists.txt。

2.CMAKE_INCLUDE_DIRECTORIES_PROJECT_BEFORE

将工程提供的头文件目录始终至于系统头文件目录的前面，当你定义的头文件确实跟系统发生冲突时可以提供一些帮助。

3.CMAKE_INCLUDE_PATH 和 CMAKE_LIBRARY_PATH 

我们在上一节已经提及。
    
    
###21.系统信息
1.CMAKE_MAJOR_VERSION，CMAKE 主版本号，比如 2.4.6 中的 2
2.CMAKE_MINOR_VERSION，CMAKE 次版本号，比如 2.4.6 中的 4
3.CMAKE_PATCH_VERSION，CMAKE 补丁等级，比如 2.4.6 中的 6
4.CMAKE_SYSTEM，系统名称，比如 Linux-2.6.22
5.CMAKE_SYSTEM_NAME，不包含版本的系统名，比如 Linux
6.CMAKE_SYSTEM_VERSION，系统版本，比如 2.6.22
7.CMAKE_SYSTEM_PROCESSOR，处理器名称，比如 i686.
8.UNIX，在所有的类 UNIX 平台为 TRUE，包括 OS X 和 cygwin
9.WIN32，在所有的 win32 平台为 TRUE，包括 cygwin

###22.主要的开关选项
1.CMAKE_ALLOW_LOOSE_LOOP_CONSTRUCTS，用来控制 IF ELSE 语句的书写方式，在下一节语法部分会讲到。
2.BUILD_SHARED_LIBS
这个开关用来控制默认的库编译方式，如果不进行设置，使用 ADD_LIBRARY 并没有指定库类型的情况下，默认编译生成的库都是静态库。如果 SET(BUILD_SHARED_LIBS ON)后，默认生成的为动态库。
３.CMAKE_C_FLAGS
设置 C 编译选项，也可以通过指令 ADD_DEFINITIONS()添加。
4.CMAKE_CXX_FLAGS
设置 C++编译选项，也可以通过指令 ADD_DEFINITIONS()添加。

###23.cmake常用指令
1.ADD_DEFINITIONS

-  向 C/C++编译器添加-D 定义
- ` ADD_DEFINITIONS(-DENABLE_DEBUG -DABC)`，参数之间用空格分割
-  如果你的代码中定义了#ifdef ENABLE_DEBUG #endif，这个代码块就会生效
- 如果要添加其他的编译器开关，可以通过 CMAKE_C_FLAGS 变量CMAKE_CXX_FLAGS 变量设置

2.ADD_DEPENDENCIES

- 定义 target 依赖的其他 target，确保在编译本 target 之前，其他的 target 已经被构建。

- 
 
        ADD_DEPENDENCIES(target-name depend-target1
                          depend-target2 ...)
       
3.ADD_EXECUTABLE、ADD_LIBRARY、ADD_SUBDIRECTORY

 前面已经介绍过了，这里不再罗唆。

4.ADD_TEST 与 ENABLE_TESTING 指令。

`ENABLE_TESTING `指令用来控制 Makefile 是否构建 test 目标，涉及工程所有目录。语法很简单，没有任何参数，ENABLE_TESTING()，一般情况这个指令放在工程的主CMakeLists.txt 中

      ADD_TEST(testname Exename arg1 arg2 ...)
      
`testname `是自定义的 test 名称，`Exename `可以是构建的目标文件也可以是外部脚本等等。后面连接传递给可执行文件的参数。如果没有在同一个 CMakeLists.txt 中打开ENABLE_TESTING()指令，任何 ADD_TEST 都是无效的。

5.AUX_SOURCE_DIRECTORY  

       AUX_SOURCE_DIRECTORY(dir VARIABLE)
       
作用是发现一个目录下所有的源代码文件并将列表存储在一个变量中，这个指令临时被用来自动构建源文件列表。因为目前 cmake 还不能自动发现新添加的源文件。

6.CMAKE_MINIMUM_REQUIRED

       CMAKE_MINIMUM_REQUIRED(VERSION versionNumber [FATAL_ERROR])
       
7.EXEC_PROGRAM

在 CMakeLists.txt 处理过程中执行命令，并不会在生成的 Makefile 中执行

        EXEC_PROGRAM(Executable [directory in which to run]
                     [ARGS <arguments to executable>]
                     [OUTPUT_VARIABLE <var>]
                     [RETURN_VALUE <var>])
                     
用于在指定的目录运行某个程序，通过 ARGS 添加参数，如果要获取输出和返回值，可通过OUTPUT_VARIABLE 和 RETURN_VALUE 分别定义两个变量.

8.FILE 指令

   	FILE(WRITE filename "message to write"... )
	FILE(APPEND filename "message to write"... )
	FILE(READ filename variable)
	FILE(GLOB variable [RELATIVE path] [globbing
		 expressions]...)
	FILE(GLOB_RECURSE variable [RELATIVE path]
 		  [globbing expressions]...)
	FILE(REMOVE [directory]...)
	FILE(REMOVE_RECURSE [directory]...)
	FILE(MAKE_DIRECTORY [directory]...)
	FILE(RELATIVE_PATH variable directory file)
	FILE(TO_CMAKE_PATH path result)
	FILE(TO_NATIVE_PATH path result)
	
这里的语法都比较简单，不在展开介绍了

9.INCLUDE 指令

		INCLUDE(file1 [OPTIONAL])
		INCLUDE(module [OPTIONAL])
		
OPTIONAL 参数的作用是文件不存在也不会产生错误。

10.FIND_指令

	FIND_FILE(<VAR> name1 path1 path2 ...)
VAR 变量代表找到的文件全路径，包含文件名

 	FIND_LIBRARY(<VAR> name1 path1 path2 ...)
VAR 变量表示找到的库全路径，包含库文件名 

	FIND_PATH(<VAR> name1 path1 path2 ...)
VAR 变量代表包含这个文件的路径。

	FIND_PROGRAM(<VAR> name1 path1 path2 ...)
VAR 变量代表包含这个程序的全路径

	FIND_PACKAGE(<name> [major.minor] [QUIET] [NO_MODULE]
					[[REQUIRED|COMPONENTS] [componets...]])
用来调用预定义在 CMAKE_MODULE_PATH 下的 Find<name>.cmake 模块，你也可以自己定义 Find<name>模块，通过 SET(CMAKE_MODULE_PATH dir)将其放入工程的某个目录中供工程使用，我们在后面的章节会详细介绍 FIND_PACKAGE 的使用方法和 Find 模块的编写。

11.IF 指令

	IF(expression)
		COMMAND1(ARGS ...)
		COMMAND2(ARGS ...)
		...
	ELSE(expression)
		COMMAND1(ARGS ...)
		COMMAND2(ARGS ...)
		...
	ENDIF(expression)
	
另外一个指令是 ELSEIF，总体把握一个原则，凡是出现 IF 的地方一定要有对应的
ENDIF.出现 ELSEIF 的地方，ENDIF 是可选的。

	SET(CMAKE_ALLOW_LOOSE_LOOP_CONSTRUCTS ON)
	IF(WIN32)
	
	ELSE()
	
	ENDIF()
12.WHILE

WHILE 指令的语法是：
		
		WHILE(condition)
			COMMAND1(ARGS ...)
			COMMAND2(ARGS ...)
			...
			ENDWHILE(condition)
其真假判断条件可以参考 IF 指令。
13.FOREACH

1.  

		
		FOREACH(loop_var arg1 arg2 ...)
		COMMAND1(ARGS ...)
		COMMAND2(ARGS ...)
		...
		ENDFOREACH(loop_var)

像我们前面使用的 AUX_SOURCE_DIRECTORY 的例子

2.  	
	
		FOREACH(loop_var RANGE total)
		ENDFOREACH(loop_var)

3. 		
		FOREACH(loop_var RANGE start stop [step])
		ENDFOREACH(loop_var)
		
###24.FIND`<package>`.cmake
1.对于系统预定义的 Find<name>.cmake 模块，使用方法一般如上例所示：
每一个模块都会定义以下几个变量
•`<name>`_FOUND
•` <name>`_INCLUDE_DIR or <name>_INCLUDES
•` <name>`_LIBRARY or <name>_LIBRARIES