---
layout: post
title: "Java基础常见问题 - 引用"
date: 2012-05-09 22:00
comments: true
published: false
categories: 
- java
- reference
- computer
---
最近看到[知乎][]上有些初学者在问一些基础问题。忽然想起自己若干年前做过的一套Java实验题，里面通过代码实验的方式阐述了一些Java的基础知识和一些常犯的错误。打算今天开始利用一些时间，把内容整理到博客上，希望能给他人带来帮助。

首先要说的就是Java的基础中的基础——引用。
<!-- more -->

先来看看这一段代码。
``` java TestPrint.java (0)
public class TestPrint {

	public static void main(String[] arg) {

		// What's the output? And why? 
		String a = "AAA";
		String b = "BBB";
		String c = a;

		TestPrint test = new TestPrint();
		test.changeString(a);
		System.out.println("A: " + a);
		System.out.println("B: " + b);
		System.out.println("C: " + c);
	}

	public void changeString(String s) {
		String temp = "changedString";
		s = temp;
	}
}
```
那么这段代码的输出是什么呢？

首先
这段代码执行后的输出如下：
``` sh output (0)
A: AAA
B: BBB
C: AAA
```


``` java TestPrint.java (1)
public class TestPrint {

	public static void main(String[] arg) {

		// What's the output? And why? 
		String a = "AAA";
		String b = "BBB";
		String c = a;

		TestPrint test = new TestPrint();
		a = test.changeString(a);
		System.out.println("A: " + a);
		System.out.println("B: " + b);
		System.out.println("C: " + c);
	}

	public String changeString(String s) {
		String temp = "changedString";
		s = temp;
		return s; 
	}
}
```

``` java TestPrint.java (3)
public class TestPrint {
	public static void main(String[] arg) {
		
		// What's the output? And why? 
		User a = new User("AAA");
		User b = new User("BBB");
		User c = a;
		
		TestPrint.changeUserName(a); 
		
		System.out.println("A: " + a);
		System.out.println("B: " + b);
		System.out.println("C: " + c);
	}
	public static void changeUserName(User user) {
		user.setName("changedName"); 
	}
}
```

其中(3)的代码中用到的User类的代码如下：

``` java User.java
public class User {

	private String name;
	
	public User() {
	}
	
	public User(String name) {
		this.name = name; 
	}

	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name; 
	}
	
	public String toString() {
		return "Name(" + this.name + ")";  
	}
}
```

问，上述第(0)、(1)、(3)的代码的输出分别是什么？

> 为什么没有(2)？原本是有(2)的，但我觉得(2)的内容有点二，所以就不拿来侮辱大家的智慧了。

这里，希望有条件的人可以自己思索一下，并复制代码执行一下。想一想运行结果为什么是那样的。

----
没有条件的人，可以看一下下面的结果：
``` sh output (1)
A: changedString
B: BBB
C: AAA
```
``` sh output (3)
A: Name(changedName)
B: Name(BBB)
C: Name(changedName)
```

{% include links %}
