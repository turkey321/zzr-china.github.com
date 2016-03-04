---
layout: post
title:  "使用express+jade+mongodb创建网站"
date:   2016-3-3 22:14:54
categories: node mongodb
excerpt: node nodejs  blog 张子锐 mongodb express jade
---

* content
{:toc}


## Nodejs建站总结（一）
这个网站使用nodejs完成，前端用jade模板构建，用到了bootstrap框架，其中删除功能使用jQuery完成
后端用express+mongo完成。是很简洁小巧的模式。[源代码](https://github.com/ZZR-china/Xiew)在我的github上。

###使用的工具
npm、bower、sublime、git和mongoVUE

###涉及到的npm模块 
express:基于Node.js 平台，快速、开放、极简的 web 开发框架<br>
jade:[jade](https://segmentfault.com/a/1190000000357534)是一个高性能的模板引擎，它深受 Haml 影响，它是用 JavaScript 实现的，并且可以供 Node 使用<br>
path:标准化路径<br>
body-parser: express的中间件,现在已从express中分离出来，调用时可以使用
<pre>
app.use(bodyParser.urlencoded({ extended: true }))
</pre><br>
mongoose:封装了一些mongoDB底层，便于使用数据库功能<br>
underscore:underscore下的extend方法可以用另一个对象的新的字段替换老的对象对应的字段<br>
moment:一款用于js中的时间插件<br>
	
### 学习到的知识点
知道了怎么配置路由，了解了jade这个逼死强迫症的模板（最讨厌强制缩进语法，所以一直不学Python╮(╯▽╰)╭）。学会了如何在nodejs中使用mongoose调用mongo数据库。一些基本的配置问题也都了解了。学习后才发现以前自己懂得只是前端的皮毛，还要多加努力O(∩_∩)O
