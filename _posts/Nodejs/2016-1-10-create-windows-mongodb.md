---
layout: post
title:  "MongoDB安装成为windows服务"
date:   2016-1-10 22:14:54
categories: node mongodb
excerpt: node nodejs  blog 张子锐 mongodb
---

* content
{:toc}


## MongoDB安装成为Windows服务
在官网上下载好mongodb安装包并解压后，在解压目录创建data文件夹，并在其中创建logs和db文件夹,
并在logs文件夹中创建mongo.log文件用来存放mongo数据库的日志
	
### 搭建过程

使用以下命令将MongoDB安装成为Windows服务。笔者的MongoDB目录为D:\Program Files\mongodb
切换到D:\Program Files\mongodb\bin>输入如下命令
<pre>
mongod --logpath "D:\Program Files\mongodb\data\logs\mongo.log" --logappend --dbpath "D:\Program Files\mongodb\data\db" --directoryperdb --serviceName "MongoDB" --serviceDisplayName "MongoDB" --install
</pre>
其中 
--logpath "D:\Program Files\mongodb\data\logs\mongo.log" 指定日志文件路径<br>
--logappend --dbpath "D:\Program Files\mongodb\data\db"指定数据库工作路径<br>
--serviceName "MongoDB" --serviceDisplayName "MongoDB"添加服务<br>
--install安装<br>
输入以上命令，命令行中会显示如下数据，及成功<br>
Creating service MongoDB.
Service creation successful.
Service can be started from the command line via 'net start "MongoDB"'.

###启动服务。
net start mongoDB


该命令行指定了日志文件：D:\Program Files\mongodb\data\logs.tx，日志是以追加的方式输出的；

数据文件目录：D:\Program Files\mongodb\data，并且参数--directoryperdb说明每个DB都会新建一个目录；

Windows服务的名称：MongoDB；

最后是安装参数：--install，与之相对的是--remove

启动MongoDB：net start MongoDB