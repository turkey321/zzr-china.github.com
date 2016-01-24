---
layout: post
title: 设计模式之（八）【组合模式】
category: 设计模式
date: 2015-1-19
---

* content
{:toc}

##什么是组合模式
组合模式是一种将对象按照父子关系组合成一种树形结构的设计模式。

##应用场景
>
同一个类的多个对象之间有明显的父子关系，而且有一种“一对多”的组织关系。

##类图和实现
类图：

![数据访问对象模式](/res/img/blogimg/2015/01/compositeDP.png)

代码：
Employee实体：
{% highlight java %}
public class Employee {
	/**
	 * 姓名
	 */
	private String name;
	/**
	 * 部门
	 */
	private String dept;
	/**
	 * 下属
	 */
	private List<Employee> subordinates;

	public Employee(String name, String dept) {
		this.name = name;
		this.dept = dept;
		subordinates = new ArrayList<Employee>();
	}

	public Employee add(Employee e) {
		subordinates.add(e);
		return this;
	}

	public Employee remove(Employee e) {
		subordinates.remove(e);
		return this;
	}

	public List<Employee> getSubordinates() {
		return subordinates;
	}

	@Override
	public String toString() {
		return "Employee [name=" + name + ", dept=" + dept + "]";
	}

}
{% endhighlight %}



测试一下：
{% highlight java %}
		public static void main(String[] args) {
			Employee CEO=new Employee("张总","BOSS");

			Employee	Lidirector=new Employee("李总监","销售部");
			Employee	WangDirector=new Employee("王总监","行政部");

			CEO.add(WangDirector).add(Lidirector);

			Employee liuManager =new Employee("刘经理","销售部");
			Employee zhaoManager =new Employee("赵经理","销售部");

			Lidirector.add(liuManager).add(zhaoManager);

			printEmployee(CEO);

		}

		private static void printEmployee(Employee employee){
			System.out.println(employee.toString());
			for(Employee e:employee.getSubordinates()){
				printEmployee(e);
			}
		}

			/**
				输出
			 * Employee [name=张总, dept=BOSS]
				Employee [name=王总监, dept=行政部]
				Employee [name=李总监, dept=销售部]
				Employee [name=刘经理, dept=销售部]
				Employee [name=赵经理, dept=销售部]
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