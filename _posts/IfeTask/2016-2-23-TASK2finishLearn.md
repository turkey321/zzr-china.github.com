---
layout: post
title: Task2学习总结
date: 2016-2-28
categories: JavaScript ife
excerpt: Baidu JavaScript CSS  张子锐   
---

* content
{:toc}


## task2学习总结
今天把[task0002](https://github.com/ZZR-china/ife/blob/master/task%2Ftask0002%2FREADME.md)
的几个项目看完了，学到了很多，也意识到了自己的很多不足。

###实现功能模块的逻辑性很重要
<br>
<pre>
接下来是一篇来宾博文，作者是[Philip Walton](http://philipwalton.com/)[@philwalton](http://twitter.com/philwalton)。
他将为我们解释为什么stopPragation()并不是一个明智的方法，并且你最好完全的避免使用这一方法。
</pre>如果你是一位前端工程师，在某些时候你没准要在页面实现这样的功能：在用户点击页面的其余位置后，弹出框或对话框便消失。
如果你通过网上搜索去查明如何实现这一功能，你有很大可能性找到这一链接[Stack Overflow question](http://stackoverflow.com/questions/152975/how-to-detect-a-click-outside-an-element)。<br>
这里是获得最高赞答案的代码：
<br>
**jQuery**
<pre>
$('html').click(function() {
  // Hide the menus if visible.
});

$('#menucontainer').click(function(event){
  event.stopPropagation();
}); 
</pre>

防止大家不是很明白这行代码的作用，这里是一个简单的纲要：
如果一个点击事件冒泡向<html>元素，隐藏菜单。如果那个单击事件是从#menucontainer内部发起的,停止冒泡这样他就不会在冒泡向<html>,因此只有外界的单击事件才会触发这一事件。
上一段代码是简单优雅的并且很聪明。但是，不幸的是，他也是一个非常槽糕的建议。
这一方案之粗糙就像是你为了修理坏掉的淋浴头而把浴室的水源切断了。
他的确行之有效，但是它完全阻断了这一页面其他代码调用这一事件的可能性。
尽管如此，他还是Stack Overflow下这一问题得赞最多的答案，说明人们承认这是一个合理的建议。(Philip君内心吐槽：这么low的代码你们还赞！)

###会出现什么问题？
<br>
你也许会这样想：谁还会写这样的代码？我使用了一个很好的库像Bootstrap，所以我不用担心这个是吗？
不幸的是，并不。停止事件冒泡并不只是被Stack Overflow推荐的坏答案；他也出现在一些最流行的库中。
为了证明这一点，让我给你展示在Ruby on Rails app中使用Bootstrap是多么容易发生BUG。包含JavaScript库的Rails ships 被称为jquery-ujs，它允许开发者去说明式地添加移除通过data-remote属性的AJAX声明连接。
(这段的翻译有问题，毕竟我不是很懂Ruby，但是并不影响文章理解O(∩_∩)O )
在下面的例子中，如果你打开dropdown控件并且点击frame窗口的其他任意位置，dropdown控件都会关闭，但是当你打开dropdown后再去点击"remote Link",它就不会关闭了。

例子在CODEPEN网站里，这里是[链接](http://codepen.io/philipwalton/pen/KzHjc).

这一bug的发生是因为Bootstrap只关闭那些监听了document属性里click事件的dropdown菜单。
但是由于ujs在它的data-remote链接操作里停止了事件冒泡，这些点击事件就再也不会到达document属性了，
因此Bootstrap的代码就不会再运行了。最差劲的地方就是在Bootstrap（或者任何框架里）不能提供任何帮助去阻止这一BUG。如果你在处理DOM，你只能任凭其他不严谨的代码在页面上运行。

###事件的问题

就像许多在JavaScript中的事情一样，DOM事件是全局性的。并且就像大多数人知道的那样，全局变量会带来凌乱、耦合的代码。
修饰一个单一的、短暂的事件乍一看上去也许会是无害的，但是，它带来了风险。<br>
当你改变一个人们期待和其他代码依赖的行为属性，你就将获得bug(%>_<%)，这只是一个时间问题。<br>
并且在我的经验里，这一种的BUG是最难去捕获到的。

###为什么人们使用停止事件冒泡这一方式？
我们知道在网上有推广着使用stopPropagation无用性的不好的建议，但是这不是唯一的理由人们做这个。开发者时常无意识地停止事件冒泡。

###返回false值

有无数的混乱围绕着你当你从一个事件句柄中返回false时。考虑下列三种情形：
<br>
**HTML**

<pre>
<!-- An inline event handler. -->
&lt;a href="http://google.com" onclick="return false"&gtGoogle&lt;/a&gt;
</pre>
<br>
**jQuery**
<pre>
// A jQuery event handler.
$('a').on('click', function() {
  return false;
});
</pre><br>
**JavaScript**
<pre>
// A native event handler.
var link = document.querySelector('a');
link.addEventListener('click', function() {
  return false;
 });
</pre>

这三个例子看来都想做同样的事情(只是返回false)，但是在现实中返回的结果却是不一样的。这里是实际上会发生在上面
的情况中情形：<br>
   1.从一个内联事件句柄返回false阻止了浏览器操纵连接地址，但是不会阻止从DOM冒泡而来的事件。<br>
   2.从一个jQuery事件句柄返回false阻止了浏览器操纵连接地址，也阻止了会停止从DOM冒泡而来的事件。<br>
   3.从一个正常DOM事件句柄返回的false完全不会有任何作用<br>
当你期望一些事件的发生但是并没有时，这是令人困惑的，但是你通常能立刻捕获它。一个更大的问题是当你期望一些事发生它也如你所愿，但是它带来了意料之外不显眼的副作用。这就是那些可怕BUG的来由。在jQuery案例中，我们已经清楚跟另外两个事件句柄相比返回false一点也不会带来什么不同的行为，但是它有。<br>在后台，jQuery实际上调用了下面两个语句：<br>
**jQuery**
<pre>
event.preventDefault();
event.stopPropagation();
</pre>

因为围绕着返回false的困惑和它停止事件冒泡在jQuery事件句柄的这一事实,我建议最好决不使用它。
明确你的意图和直接调用这些事件方法是更好的选择。<br>
**注意**：如果你将jQuery和CoffeeScript一起使用(自动返回函数的最后一个表达式),确保你没有使用任何评估为false的布尔值结束你的事件句柄不然你会有同样的问题。

###性能

时常你会读一些忠告(通常写于很久之前),建议为了性能而停止冒泡。<br>
在IE6还盛行的日子（前端第一大头疼╭(╯^╰)╮）甚至更老式浏览器还流行的时候,一个复杂DOM真的可以
减缓你的网站性能。由于事件经过整个DOM,节点越多,得到任何事件的时间越慢。
[quirksmode.org](quirksmode.org)的作者Peter Paul Koch 建议了下面的做法在一篇老文章的主题上：
<pre>
如果你的文档结构是非常复杂的(很多嵌套表等),你可能会为了节省系统资源而关闭冒泡。浏览器必须经过事件目标的每一个父元素,看它是否有一个事件句柄。即使没有发现,搜索仍需占用时间。
</pre>

在现在浏览器中，任何你通过阻止事件冒泡得到的性能提升都有可能会被你的用户忽视。<br>这是一个微优化而不是你的性能瓶颈。
我的建议是不必担心通过整个DOM的事件冒泡。毕竟,这是规范的一部分,而且浏览器已经很擅长这样做。

###怎样去替代呢？

作为一项基本规则,停止事件冒泡不能作为一个方案去解决一个问题。如果你的网页上多个事件句柄,并且有时相互干扰,然后你发现阻止冒泡可以解决一切问题,这是一件坏事。它可能会解决你当下的问题,但它可能创建另一个你不知道的。
阻止冒泡应该被认为像取消一个事件,它也只能使用于那种目的。
也许你想阻止表单提交或不允许关注页面的一个区域。在这些情况下你阻止冒泡,因为你不想让一个事件发生,不是因为你有一个不想要的DOM事件句柄被向上传递了。
在“如何在元素之外的检测一个点击事件?”的例子里,调用stopPropagation的目的并不是要完全除去单击事件,这是为了避免在页面上运行一些别的代码。
这是一个坏主意除了因为它改变了全局行为,还因为它将菜单隐藏逻辑放在在两个不同的和无关的地方,使它比需要的更脆弱。
一个更好的解决方案是有一个单独的事件句柄其逻辑是完全封装,而它唯一的作用是通过给出的事件判断是否隐藏菜单。
事实证明,更好的选择最终需要更少的代码:<br>
**jQuery**
<pre>
$(document).on('click', function(event) {
  if (!$(event.target).closest('#menucontainer').length) {
    // Hide the menus.
  }
});     
</pre>

上述句柄监听document上的单击事件并检查事件的目标是否为#menucontainer或# menucontainer的父节点。如果没有,你知道点击起源于# menucontainer之外,因此你可以隐藏菜单如果他们是可见的。

###默认阻止？

大约一年前,我开始写一个事件句柄库来帮助处理这个问题。而不是阻止事件冒泡,你也许会把一个事件简单的标记为“被处理的”。这将允许事件监听器注册更远的DOM事件去检查事件,根据是否被“处理”,确定进一步的行动是否为必要的。我的想法是,你可以“停止事件冒泡”然而实际上没有停止它。<br>
最终结果是,我从来没有需要这个库。在100%的情况下当我发现自己想要检查一个事件是否被“处理”,我注意到一个叫做preventDefault的早先的侦听器。和DOM API已经提供了一种方法来检查:defaultPrevented属性。<br>
帮助澄清这一点,我提供了一个例子。
<br>
想象你将一个事件监听器添加到文档,将使用谷歌分析跟踪用户点击链接到外部域。它可能看起来像这样:<br>
**jQuery**
<pre>
$(document).on('click', 'a', function(event) {
  if (this.hostname != 'css-tricks.com') {
    ga('send', 'event', 'Outbound Link', this.href);
  }
});    
</pre>

这段代码的问题是,并非所有的链接点击带你去其他页面。有时JavaScript会拦截点击,调用preventDefault和做其他的事情。上述数据远程链接是一个典型的例子。另一个例子是一个Twitter分享按钮,打开一个弹出框而不是跳去twitter.com。
为了避免跟踪这些点击,你可能想要停止事件冒泡,但是检查defaultPrevented事件是一个更好的方式。<br>
**jQuery**
<pre>
$(document).on('click', 'a', function(event) {

  // Ignore this event if preventDefault has been called.
  if (event.defaultPrevented) return;

  if (this.hostname != 'css-tricks.com') {
    ga('send', 'event', 'Outbound Link', this.href);
  }
});    
</pre>

由于在点击句柄中调用preventDefault总是阻止浏览器导航到一个链接的地址,你可以100%相信如果defaultPrevented是真的,用户没有去任何地方。换句话说,这种技巧既比stopPropagation更可靠,而且不会有任何的副作用。

###结论

希望本文能够帮助你从一个新角度思考DOM事件。他们不是可以修改而不考虑后果的孤立的块。他们是全局的,相互关联的对象,常常影响到远比你能意识到的更多的代码。<br>
为了避免错误,最好让事件孤立,让他们就像浏览器预期的一样冒泡。<br>
如果你不确定要做什么,问问自己以下问题:有没有可能是现在或将来其他的一些代码,可能想让这事件发生?答案通常是肯定的。无论是对琐事的Bootstrap模式或关键事件跟踪分析,获得事件对象是非常重要的。有疑问时,不要阻止冒泡。