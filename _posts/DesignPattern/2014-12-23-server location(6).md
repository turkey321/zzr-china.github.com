---
layout: post
title: 设计模式之（六）【服务定位器模式】
category: 设计模式
date: 2014-12-23
---
* content
{:toc}
##什么是服务定位器模式
顾名思义就是通过定位器获取到指定的服务类，这样就达到了解耦服务使用者和服务的作用了。

<!-- more -->

##应用场景
>
一个人出生后，会登记他的身份信息（出生地、出生日期、性别等），当这个人到了该上学的年龄，教育部门可能就需要通知其参加义务
教育，当这个人到了一定的年龄需要服兵役了，国家则会通知他来服兵役……


##类图和实现
类图：
![服务定位器模式](/res/img/blogimg/locator.png)

代码：
服务接口Service：
{% highlight java %}
public interface Service {
	 public void execute();
}
{% endhighlight %}

服务缓存类：
{% highlight java %}

public class ServiceCache {

	private Map<String,Service> cache=new HashMap<String,Service>();

	private static ServiceCache serviceCache;

	private ServiceCache(){};

	public void addService(String name,Service service){
		cache.put(name, service);
	}
	public void removeService(String name){
		if(cache.containsKey(name)){
			cache.remove(name);
		}
	}
	public Service getService(String name){
		return cache.get(name);
	}

	public static ServiceCache getInstance(){
		if(serviceCache==null){
			serviceCache=new ServiceCache();
		}
		return serviceCache;
	}

}
{% endhighlight %}

服务1Service1：
{% highlight java %}
public class Service1 implements Service {

	public Service1(){
		ServiceCache.getInstance().addService(this.getClass().getName(), this);
	}

	@Override
	public void execute() {
		System.out.println("This is Service1");

	}
}
{% endhighlight %}

服务2Service2：
{% highlight java %}
public class Service2 implements Service {

	public Service2(){
		ServiceCache.getInstance().addService(this.getClass().getName(), this);
	}
	@Override
	public void execute() {
		System.out.println("This is Service2");

	}

}
{% endhighlight %}

服务定位器ServiceLocator：
{% highlight java %}
public class ServiceLocator {

	public Service getService(String name){
		return  ServiceCache.getInstance().getService(name);
	}
}
{% endhighlight %}

测试一下：
{% highlight java %}
		Service1 s1=new Service1();
		Service2 s2=new Service2();
		ServiceLocator locator=new ServiceLocator();
	    Service service=	locator.getService("com.design.locator.Service2");
	    service.execute();
        //输出This is Service2
{% endhighlight %}

