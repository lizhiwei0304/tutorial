git的使用

1.首先建立一个文件夹

2.将文件或文件夹拷入

3.初始化一个git的仓库

```
git init
```

4.添加文件或文件夹到git仓库，分两步

```
1.使用命令  git add <file>  注意，可以反复多次使用，添加到多个文件
2.使用命令  git commit -m <message>，完成对修改内容的标注
```

5.使用命令查看版本的日志

```
git log    <--pretty=oneline>  可选，解决输出信息太多
```

6.回退到上一个版本

```
git reset --hard HEAD^
```

​         Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

​         `       --hard`参数有啥意义？这个后面再讲，现在你先放心使用。

​         如果后悔了，怎么办？办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

​         在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

​        终于舒了口气，从输出可知，`append GPL`的commit id是`1094adb`，现在，你又可以乘坐时光机回到未来了。

