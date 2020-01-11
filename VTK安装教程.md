## VTK安装教程

1.修改/usr/lib/x86_64-linux-gnu/qt-default/qtchooser/default.conf 文件，将里面的qt4改为qt5

这一步主要是设置电脑使用qt5，如果之前安装过qt5并且设置过的可以忽略

2.安装X11 

```
sudo apt-get install libx11-dev libxext-dev libxtst-dev libxrender-dev libxmu-dev libxmuu-dev
```

3.安装OpenGL

```
sudo apt-get install build-essential libgl1-mesa-dev libglu1-mesa-dev
```

4.安装**libglut-dev**

```
sudo apt-get install freeglut3-dev
```

5.编译源码（源码已经下载，版本不对，可以自行下载）

```
mkdir build && cd build 

cmake ..

make -jn

sudo make installl
```

