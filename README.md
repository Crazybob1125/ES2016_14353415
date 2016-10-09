# Lab1 DOL开发环境配置


DOL-Distribution Operation Layout。 分布式操作系统（Distribution System），是指分布式处理的系统，而分布式处理是将不同地点，或具有不同功能的多台计算机听过通信网络连接起来，在控制系统的统一管理下处理同一个大规模的计算机任务。简单地说，就是讲一个很大型的任务拆散开来，分配到各个主机去计算，最后汇总。

本次试验主要是配置DOL的实验环境。

##1.安装一些必要的环境：

$	sudo apt-get update

$	sudo apt-get install ant

$ 	sudo apt-get install openjdk-7-jdk

$	sudo apt-get install unzip

输入之后这些组件都会自动下载，要注意电脑要联网。

##2.将实验文档里的文件解压到dol文件夹中。

$ unzip dol_ethz.zip -d dol

$ tar -zxvf systemc-2.3.1.tgz

####  *这里遇到了g++组件未安装的问题。于是先跳出来安装g++组件：

  *最好全程都在ROOT权限下操作:

$ sudo su //获取ROOT权限

$ sudo apt-get update
![](http://i.imgur.com/OyH3gKU.png)
<font size=1>*注：因本人配置环境时未截图，故借用舍友的截图。</font>

$ sudo apt-get install g++

$ g++ //验证一下是否安装成功


 成功提示：g++: fatal error: no input files
   compilation terminated.
![](http://i.imgur.com/bpFPn7P.png)

####*顺便把Java环境配置一下：
<font size=1>*注FROM师兄的实验文档，完全照做。唯一遇到的一点麻烦在于a步骤。此安装包对应64位系统，与我的系统不符，故去下了另一个可用的jdk安装包</font>
![](http://i.imgur.com/aGVrUnd.png)


##3.编译SYSTEMC
 a.将systemc-2.3.1解压到自定义安装路径下

 运行configure

$	../configure CXX=g++ --disable-async-updates

出现如下结果：

![](http://i.imgur.com/1mCewAW.png)

b.编译

$   sudo make install

$ pwd 把当前路径记录下来 

##4.编译DOL


进入刚刚DOL的文件夹找到build_zip.xml文件。

找到下面这段话，将其中YYY改成刚才pwd得到的内容。

<property name="systemc.inc" value="YYY/include"/

<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/

然后编译

$ ant-f build_zip.xml all

运行第一个例子：

$ ant -f runexample.xml -Dnumber=1

成功结果:

![](http://i.imgur.com/qmbz18Q.png)

##实验感想
通过这次试验了解了DOL是个什么东西，简单认识到DOL的优越性，在配置的过程中出现了挺多问题比如权限不够有些操作进行不了，使用sudo su获取权限即可。还有课件里给的JDK包是适用于64位系统的，只能上网再下了一个32位的包装上了。也熟悉了很多LINUX的语法，比如创建文件，解压，移动，重命名，修改文件等等。



 