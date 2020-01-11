1.设置源为清华源
2.设置分屏：
    系统设置，屏幕显示，关闭之前的屏幕，只显示最新的屏幕3
3.添加北理的源
        ``` sudo apt-get install curl ```
        ``` curl -L https://git.io/vFfXX > update.bash && bash update.bash && rm update.bash ```
4.添加搜狗输入法
        ``` chmod +x sousogoupinyin_2.2.0.0108_amd64.deb  ```
        ``` sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb ```
        ``` sudo apt-get -f install ```
        ``` sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb ```
5.安装谷歌浏览器
        ``` sudo wget https://repo.fdzh.org/chrome/google-chrome.list -P /etc/apt/sources.list.d/```
        ``` wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -```
        ``` sudo apt-get update ```
        ``` sudo apt-get install google-chrome-stable ```
6.卸载系统软件
   卸载火狐浏览器
        ```dpkg --get-selections |grep firefox```
        ```sudo apt-get purge firefox firefox-locale-en firefox-locale-zh-hans unity-scope-firefoxbookmarks ```
   卸载亚马逊
        ```sudo apt-get remove unity-webapps-common ```
   卸载自带的Word
        ```sudo apt-get remove --purge libreoffice* ```
7.安装福昕PDF阅读器
        ```tar -zxvf FoxitReader.enu.setup.2.4.4.0911.x64.run.tar.gz ```
        ```sudo ./FoxitReader.enu.setup.2.4.4.0911\(r057d814\).x64.run ```
8.安装WPS和字体 
        ```sudo dpkg -i wps-office_11.1.0.8392_amd64.deb ```
     安装字体
        ```unzip wps_symbol_fonts.zip ```
        ```sudo cp mtextra.ttf  symbol.ttf  WEBDINGS.TTF  wingding.ttf  WINGDNG2.ttf  WINGDNG3.ttf  /usr/share/fonts ```
9.安装markdown-typora
        ```sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE```
        ```sudo add-apt-repository 'deb http://typora.io linux/' ```
        ``` sudo apt-get update```
        ```sudo apt-get install typora```
10.生成ssh，并添加到github中
        ```ssh-keygen -t rsa -C "lizw_0304@163.com" ```
        ```cat ~/.ssh/id_rsa.pub ```
11.安装teamviewer
        ``` chmod +x teamviewer_14.2.8352_amd64.deb ```
        ``` sudo dpkg -i teamviewer_14.2.8352_amd64.deb ```  
        ``` sudo apt-get -f install ```
        ``` sudo dpkg -i teamviewer_14.2.8352_amd64.deb ```
12.设置电脑不挂起
	``` sudo gedit /etc/systemd/logind.conf ```
	``` HandleLidSwitch=ignore ```
	``` service systemd-logind restart ```

13.安装微信

###### 1)解压缩linux-x64.tar.gz

###### 2) 将微信添加到启动器

```
sudo mv electronic-wechat-linux-x64/ /opt/
sudo ln -s /opt/electronic-wechat-linux-x64/e;ectronic-wechat /uer/bin/wechat
```

3）Alt+F2 打开搜索wechat，最后锁定到任务栏