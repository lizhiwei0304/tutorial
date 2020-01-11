### opencv安装教程

1.从下面网站地址下载相应版本的opencv

​           http://jaist.dl.sourceforge.net/project/opencvlibrary/opencv-unix/

2.解压缩下载的包

3.进行编译安装

​          

```
           cd opencv-2.4.13

​		   mkdir release 

​          sudo apt-get install build-essential cmake libgtk2.0-dev pkg-config python-dev python-numpy libavcodec-dev libavformat-dev libswscale-dev

​          cd release 

​          cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..

​          make -jN         (N表示你电脑的核数，不设置表示默认)

​          sudo make install 
```

