---
layout: post
title: 设计模式之（五）【迭代器模式】
category: 设计模式
date: 2014-12-9
---
* content
{:toc}
##什么是迭代器模式
在不暴露对象内部结构或属性的情况下，提供一种能够顺序访问对象中各个元素的设计模式。

<!-- more -->

##应用场景
>
JAVA 中的 iterator。

##类图和实现
类图：
![迭代器模式类图](/res/img/blogimg/iterator.jpg)

代码：
Iterator.java：
{% highlight java %}
public interface Iterator {
   public boolean hasNext();
   public Object next();
}
{% endhighlight %}

Container.java：
{% highlight java %}
public interface Container {
   public Iterator getIterator();
}
{% endhighlight %}

NameRepository.java：
{% highlight java %}
public class NameRepository implements Container {
   public String names[] = {"Robert" , "John" ,"Julie" , "Lora"};

   @Override
   public Iterator getIterator() {
      return new NameIterator();
   }

   private class NameIterator implements Iterator {

      int index;

      @Override
      public boolean hasNext() {
         if(index < names.length){
            return true;
         }
         return false;
      }

      @Override
      public Object next() {
         if(this.hasNext()){
            return names[index++];
         }
         return null;
      }
   }
}
{% endhighlight %}

调用：
{% highlight java %}
  NameRepository namesRepository = new NameRepository();
      for(Iterator iter = namesRepository.getIterator(); iter.hasNext();){
         String name = (String)iter.next();
         System.out.println("Name : " + name);
      }
/**输出
 * Name : Robert
Name : John
Name : Julie
Name : Lora
*/
{% endhighlight %}


<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- hah -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-7313295507948994"
     data-ad-slot="2634724060"
     data-ad-format="auto"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>



