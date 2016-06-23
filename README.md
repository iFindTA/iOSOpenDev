# iOSOpenDev
steps for iOSOpenDev
#### 导读：
```
	本文不定期持续更新，是在前人研究基础上的总结性文章，主要介绍theos的替代品iOSOpenDev的安装、简单使用
```
#### iOSOpenDev的安装：

##### 准备资源：
```
1，MacPorts
2，DPKG
3，iOSOpenDev
```

##### step1－install MacPorts && DPKG
a，首先去[官网](https://www.macports.org/install.php)下载最新版本，作者的是OS X 10.11 El Capitan MacPorts 2.3.4版本(安装过程需要连接VPN,否则无法安装成功)，安装完MacPorts后打开终端，输入 sudo port -v selfupdate 更新MacPorts到最新版本，时间可能比较长。
b，更新完MacPorts后安装DPKG文件，在终端输入sudo port -f install dpkg

##### step2－install iOSOpenDev
a，首先去[官网](http://iosopendev.com/download/)下载最新版本，作者的是iOSOpenDev 1.6-2 Installer版本
b，直接安装iOSOpenDev.dmg文件，成功则可以忽略以下步骤，若出现以下错误，则继续：

![image](https://raw.githubusercontent.com/iFindTA/screenshots/master/iosopendev_0.png)

此时Command＋L可查看错误信息，作者的错误如下：
```
	PackageKit: Install Failed: Error Domain=PKInstallErrorDomain Code=112 "运行软件包“iOSOpenDev-1.6-2.pkg”的脚本时出错。" UserInfo={NSFilePath=./postinstall, NSURL=file://localhost/Users/ice/Downloads/iOSOpenDev-1.6-2.pkg#iodsetup.pkg, PKInstallPackageIdentifier=com.iosopendev.iosopendev162.iod-setup.pkg, NSLocalizedDescription=运行软件包“iOSOpenDev-1.6-2.pkg”的脚本时出错。} {
        NSFilePath = "./postinstall";
        NSLocalizedDescription = "\U8fd0\U884c\U8f6f\U4ef6\U5305\U201ciOSOpenDev-1.6-2.pkg\U201d\U7684\U811a\U672c\U65f6\U51fa\U9519\U3002";
        NSURL = "file://localhost/Users/ice/Downloads/iOSOpenDev-1.6-2.pkg#iodsetup.pkg";
        PKInstallPackageIdentifier = "com.iosopendev.iosopendev162.iod-setup.pkg";
    }
```

仔细观察发现是执行iod-setup脚本时出错，这个文件在那里呢？打开Finder，Command＋Shift＋g，输入路径：/opt 回车你会看到下边的三个文件夹：
```
	iosopendev
	iosopendevsetup
	iosopendevuninstall
```
c，脚本就在iOSOpenDevSetup/bin/文件夹下，打开编辑，可以使用sublime或者终端编辑器
d，找到function：downloadGithubTarball这个方法，发现有两处调用的地方，注释掉调用的地方（＃号为注释）,Ctl+S或者命令：wq
e，手动下载对应三个文件：
```
	https://github.com/kokoabim/iOSOpenDev 
    https://github.com/kokoabim/iOSOpenDev-Xcode-Templates 
    https://github.com/kokoabim/iOSOpenDev-Framework-Header-Files 
```
拷贝：
```
	sudo cp -r iosopendev-master/* /opt/iosopendev/
	(iosopendev里，sudo mkdir templates)
	sudo cp -r iosopendev-xcode-templates-master/* /opt/iosopendev/templates
	(iosopendev里，sudo mkdir frameworks)
	sudo cp -r iosopendev-framework-header-files-master/* /opt/iosopendev/frameworks
```
f，再次安装：
```
	sudo ./iod-setup base
	指定最新xcode sdk：
	sudo ./iod-setup sdk -sdk iphoneos
```
可能会出错：
```
	PrivateFramework directory not found: /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS9.3.sdk/System/Library/PrivateFrameworks
```
这是因为作者的xcode为7.3.1版本，apple已经将私有framework全部移走，有人给出解决办法是下载iOS9.2SDK版本指定为Xcode的编译SDK，不过如果你仅仅是为了hook替换一些方法，并没有用到私有方法，可以无需理会

g，安装成功的表现为：
1.
~/library/developer/xcode 里面会多出
Templates/iosopendev
2. 
sublime ~/.bash_profile
会看到：
export iOSOpenDevPath=/opt/iOSOpenDev
export iOSOpenDevDevice=
export PATH=/opt/iOSOpenDev/bin:$PATH
3.
启动xcode，新建工程，多出一个“iOSOpenDev”的模板。如图：

![image](https://raw.githubusercontent.com/iFindTA/screenshots/master/iosopendev_1.png)

##### 至此，你已经会在你的电脑上搭建越狱开发环境了，enjoy it now！

###### 参考：
```
	http://www.jianshu.com/p/29580725707a
    http://www.aichengxu.com/view/6004431
```