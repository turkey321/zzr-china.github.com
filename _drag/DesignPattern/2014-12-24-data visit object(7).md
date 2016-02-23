---
layout: post
title: 设计模式之（七）【数据访问对象模式】
category: 设计模式
date: 2014-12-24
---

* content
{:toc}

##什么是数据访问对象模式
数据访问对象模式（Data Access Object Pattern）或者叫做DAO模式：将数据数据的访问和操作从其他的业务逻辑中剥离出来。

<!-- more -->

##应用场景
>
这在Java企业级开发中非常常见，就不举例子了。

##类图和实现
类图：

![数据访问对象模式](/res/img/blogimg/DAO.png)

代码：
User实体：
{% highlight java %}
public class User {

	private String name;
	private int age;

	public User(String name,int age){
		this.name=name;
		this.age=age;
	}

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}

	@Override
	public String toString() {
		return "User [name=" + name + ", age=" + age + "]";
	}
}
{% endhighlight %}

Dao接口：
{% highlight java %}
public interface UserDao {
	/**
	 * 获取所有User
	 * @return
	 */
	public List<User> getAllUser();

	/**
	 * 添加一个User
	 * @return
	 */
	public boolean addUser(User user);


}
{% endhighlight %}

Dao的实现：
{% highlight java %}
public class UserDaoImpl implements UserDao {

	//此处相当于数据库了
	List<User> userList=new ArrayList<User>();

	public UserDaoImpl(){
		User user1=new User("u1",12);
		User user2=new User("u2",13);
		userList.add(user1);
		userList.add(user2);
	}

	@Override
	public List<User> getAllUser() {
		return userList;
	}

	@Override
	public boolean addUser(User user) {
		return userList.add(user);
	}

}
{% endhighlight %}

测试一下：
{% highlight java %}
		UserDao userDao=new UserDaoImpl();

		User user=new User("u3",15);
		userDao.addUser(user);

		List<User> userList =userDao.getAllUser();

		for(User u:userList){
			System.out.println(u.toString());
		}
		/**
		 * 输出：
		 * 	User [name=u1, age=12]
		 *  User [name=u2, age=13]
		 *	User [name=u3, age=15]
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