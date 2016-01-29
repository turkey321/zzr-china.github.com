---
layout: post
title:  "颜色变化的九宫格"
date:   2016-1-29 
categories: 前端学习
excerpt: CSS JavaScript color
---

* content
{:toc}


## IT修真学院

前几天在网上闲逛时看到有个IT修真学院的，一看口气这么大我当然要去看看，之后就决定先跟着他们的课程试试，他们的JS课程是顺着他们的CSS课程接着来的，于是只好在跟着css课程先做。

---

## 用CSS实现一个九宫格

这个很简单，一些简单的css代码而已，但是一开始竟然忘了div是没有color属性的，(⊙﹏⊙)，设置背景色用background，一个div包含9个小div，css代码如下
<pre>
div#container{
width:400px;
height:400px;
padding:1;
margin: 1;

}
div.sudoku{
         margin: 10;
         padding: 0;
         width:100px;
		 height:100px;
		 background-color:orange; 
         float: left;
        
		 }
</pre>
---

## 编写js代码控制颜色变化

这里用了一个 color数组包含一些color参数，然后编写一个function change()实现div的颜色style变化，最后使用了setinterval（change,1000）实现函数的自调用，代码如下：

<pre>
  var color=new Array(
    "red","blue","#cc0000","white","black","gray",
	"#FAEBD7","F0F8FF","#4B0082","#BC8F8F","#FFE4B5","#3CB371","#7B68EE"
  );
  var divChange=document.getElementById("1");
  var divChange2=document.getElementById("2");
  var divChange3=document.getElementById("3");
  var divChange4=document.getElementById("4");
  var divChange5=document.getElementById("5");
  var divChange6=document.getElementById("6");
  var divChange7=document.getElementById("7");
  var divChange8=document.getElementById("8");
  var divChange9=document.getElementById("9");
 
   var i=0; 
  function change(){
    divChange.setAttribute("style","background:"+color[i]);
    divChange2.setAttribute("style","background:"+color[i+1]);
    divChange3.setAttribute("style","background:"+color[i]);
    divChange4.setAttribute("style","background:"+color[i]);
    divChange5.setAttribute("style","background:"+color[i]);
    divChange6.setAttribute("style","background:"+color[i]);
    divChange7.setAttribute("style","background:"+color[i]);
    divChange8.setAttribute("style","background:"+color[i]);
    divChange9.setAttribute("style","background:"+color[i]);
	i++;
	
  }
  var set=setInterval(change,1000);


  </pre>

---













##总结
  修真学院的教程的确不错但是不适合我，我还是在网上找些js项目跟着做，然后学习jQuery,Angular.js框架。



