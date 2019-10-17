---
layout: post
title: "Java学习日记———基础篇"
subtitle:   "基础篇"
author:     "Twany"
header-img: "img/bg-scene/post-bg-ele.jpg"
catalog: true
tags:
    - Java
---

> 感觉PHP很难有进步了，停留在这个阶段太久了。不想继续下去。
> 于是，开始学习Java。
> 2019-02-24


## Scanner，Random，ArrayList 类

***

<b>Java有类似namespace的东西。不同类包含在不同结构下。平时的int,string等都处于 `java.lang` ，如果使用的话不用引用，直接使用即可。</b>
<br></br>
而这次讲的这三个类处于 `java.util` 下，所以，我们需要引用

`import java.util.Scanner`




#### Scanner类

***

Scanner类是用来录入键盘输入的类。类似于c的小黑框print;

使用：

```
//引用
import java.util.Scanner;

//创建对象
Scanner sc = new Scanner(System.in);

//使用
int num = sc.nextInt();
//字符串为： String chars = sc.nextLine();

```

#### Random

***

Random是用来生成随机数字的类。

使用：

```
import java.util.Random;

Random r = new Random();

//生成0~10的随机整数
int num = r.nextInt(10);

```

#### ArrayList

***

ArrayLisy是用来产生一个不定大小的数组，它的长度可变。

使用：

```
import java.util.ArrayList;

ArratList<String> arr = new ArrayList<>();
//<> 尖括号内不能填基本类型，必须填基本类型包装类

String a = "刘备";
String b = "曹操";
String c = "孙权";

//添加
arr.add(a);
arr.add(b);
arr.add(c);

//删除
arr.add(0);//刘备删去

//获得
String d = arr.get(1);//曹操

//数组长度
int size = arr.size();//2

```
<img class="shadow" src="/img/in-post/java/baseClass.png">


## String,static,Array,Math

***

#### String

```
String a = "Hello";
String b = "world";

//判断两个字符串是否相等(bool)
a.equals(b); //false

//判断两个字符串是否相等——忽略大小写(bool)
a.equalsIgnoreCase(b);

//长度
a.length();

//拼接
a.concat("!!!");//Hello!!!

//获取指定索引处字符
a.charAt(0);//H

//indexof(获取索引)，substring(截取字符串)

//toCharArray()——返回数组，replace()——替换

```

## 继承，super，this，抽象类

***

#### 继承

***

#### super，this

***

`super`指向父类，父类成员，父类方法。
`this`指向本类。

#### 抽象类 abstract

```
public abstract class Animal(){
	public abstact void run();
	抽象方法只包含一个方法名，没有方法体
}
```

<b>父类的抽象方法子类必须重写</b>

## 接口，多态

***

#### 接口

```
public interface Api(){
	public abstract void run();

	public default void eat(){
		System.out.println("default类子类可以重写，可以不写。实现时直接实现：new Cat().eat();")
	}

	//简写
	void drink();
}

class Cat implements Api(){
	public void run(){
		
	}
}

```

**注意接口的语法。接口可以联系案例，笔记本USB接口，鼠标，键盘。**

***

#### 多态

```
Fu zi = new Zi();
Fu nv = new Nv();
zi.name();//小明
nv.name();//小红

if(zi instanceof Zi){
	System.out println("儿子的类");
}else if(nv instance of Nv){
	System.out println("儿子的类");
}
```

***

## final,内部类

*** 

#### final

**final即不可更改。**

***

#### 内部类

```
public class One(){
	class Two(){
		public void sout(){
			System.out println("内部类方法")
		}
	}

}

public class Test(){
	public static void main(String[] args){
		One one = new One();
		Two two = one.new Two();

		two.sout();//输出：内部类方法
	}
}
```

***

#### 匿名类

> 匿名类的前提是要继承一个父类或者接口

***

## 引用类型用法总结

#### class作为成员变量

```
class Role(){
	int name;
	int blood;

	//class作为成员类
	Weapon wp;//武器类
	Armour am;//盔甲类

}

class Weapon(){
	int name;
	int hurt;
}

class Armour(){
	int name;
	int protect;
}
```
***

#### interface作为成员变量

***

**一个有趣的案例**

```java
package Weapon;

public class User {
    String name;
    int blood=100;
    int protect=0;
    int hurt=0;

    public User(String name){
        this.name = name;
    }

	//class作为成员变量
    Weapon wp;
    Armour am;

    //装备武器
    public void setWp(Weapon wp){
        this.wp = wp;
        this.hurt += wp.hurt;
    }

    //装备盔甲
    public void setAm(Armour am){
        this.am = am;
        this.protect +=am.protect;
        this.blood +=am.protect;
    }

    public void show(){
        System.out.println(name+"使用"+wp.name+"造成了"+hurt+"，他的防御是"+protect+",他的血量是"+blood);
    }
}


//武器类
public class Weapon {
    String name;
    int hurt;
    public Weapon(String name,int hurt){
        this.name = name;
        this.hurt = hurt;
    }
}

//盔甲类
public class Armour {
    String name;
    int protect;

    public Armour(String name,int protect){
        this.name = name;
        this.protect = protect;
    }
}

public class Test {
    public static void main(String[] args) {
        User solider = new User("普通战士");

        Weapon wp = new Weapon("青龙偃月刀",100);
        Armour am = new Armour("青铜战甲",50);

		//装备
        solider.setWp(wp);
        solider.setAm(am);

        solider.show();
    }
}


```

## 常用API

#### currentTimeMillis

```
//返回当前毫秒数
long now = System.currentTimeMillsis();
```

***

#### arraycopy()

```java
//arraycopy(源数组,源数组起始位置,目标数组,目标数组起始位置,复制长度)

int[] src = [1,2,3,4,5];
int[] dest= [5,6,7,8,9];

System.out.println("复制前:"+Arrays.toString(dest));//此处又有一个知识点Arrays.toString()
//[5,6,7,8,9]

System.arraycopy(src, 0, dest, 0, 3);

System.out.println("复制后:"+Arrays.toString(dest));
//[1,2,3,8,9]

```

***

#### Calendar类

```java
import java.util.Calendar;

public class CalendarUtil {
    public static void main(String[] args) {
        
        // 创建Calendar对象
        Calendar cal = Calendar.getInstance();
        
        // 设置年 
        int year = cal.get(Calendar.YEAR);
        
        // 设置月
        int month = cal.get(Calendar.MONTH) + 1;
        
        // 设置日
        int dayOfMonth = cal.get(Calendar.DAY_OF_MONTH);
        
        System.out.print(year + "年" + month + "月" + dayOfMonth + "日");
        //2019-2-26
    }    
}

```

***

#### StringBuilder

- 普通String
```java
//普通的String每次存储都会另外存储
String a = "Hello";
a += "world";

System.out.println("Helloworld");
//内存中存储了，"Hello","world","Helloworld" 三个字符串，占用内存
```

- StringBuilder类
```java
//StringBuilder类会形成一个缓存区
StringBuilder builder = new StringBuilder("Hello");

//append()可接受各种类型的参数
builder.append(",world,").append(true).append(100);

System.out.println(builder);
//Hello,world,ture100

```

***

#### 包装类

```java
//自动装箱
Integer i = 4;//Interger i = Interger.valueOf(4); i为一个对象

//自动拆箱
i =i+2;//等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5;

//加法运算完成后，再次装箱，把基本数值转成对象。 
```

***

#### 基本类型与字符串之间的转换

```java
//以int为例
int num = Integer.parseInt("100");
```

## 集合和数组

**直接看案例——斗地主**

```java
import java.util.ArrayList;
import java.util.Collections;

public class Poker {
    public static void main(String[] args) {
        //创建总牌
        ArrayList<String> pokerBox = new ArrayList<>();
        //数字集合
        ArrayList<String> number = new ArrayList<>();
        //四个图案
        ArrayList<String> pic = new ArrayList<>();

        pic.add("♣");
        pic.add("♦");
        pic.add("♥");
        pic.add("♠");

        //数字
        for (int i = 1; i <11 ; i++) {
            number.add(i+"");
        }
        number.add("J");
        number.add("Q");
        number.add("K");

        //拼接图案和数字
        for (String a : pic){
            for (String b : number){
                pokerBox.add(a+b);
            }
        }
        pokerBox.add("小王");
        pokerBox.add("大王");

        //打乱顺序
        Collections.shuffle(pokerBox);
        /*
        Collections.addAll(list, 20, 10, 100); //向list数组插入这三个数据
        Collections.shuffle(list);//打乱数组
        Collections.sort(list);//排序

         */

        ArrayList<String> dipai = new ArrayList<>();
        ArrayList<String> one = new ArrayList<>();
        ArrayList<String> two = new ArrayList<>();
        ArrayList<String> three = new ArrayList<>();

        //发牌
        dipai.add(pokerBox.get(0));
        dipai.add(pokerBox.get(1));
        dipai.add(pokerBox.get(2));

        for (int i =3;i<pokerBox.size()-2;i++){
            one.add(pokerBox.get(i));
            two.add(pokerBox.get(i+1));
            three.add(pokerBox.get(i+2));
            i +=2;
        }

        System.out.println(one);
        System.out.println(two);
        System.out.println(three);
        System.out.println(dipai);

    }
}
```


## 异常

```java
import com.sun.deploy.association.RegisterFailedException;

import java.util.Scanner;

public class Throw {

    static String[] name = {"张三","李四","王五"};

    public static void main(String[] args) throws RegisterFailedException {

        Scanner n = new Scanner(System.in);
        System.out.println("输入姓名注册");

        String my = n.next();
        check(my);
    }

    public static void check(String names) throws RegisterFailedException {
        for (String x : name){
            if (names.equals(x)) {
                throw new RegisterFailedException("用户名已注册");
            }
        }
    }
}
```

## 多线程

多线程有两种写法：

- Thread类

```java
//第一种，通过Thread
public class Mythreads extends Thread{
        //run方法为默认执行方法
        public void run(){
            for (int i = 0; i < 10; i++) {
                System.out.println("run"+i+Thread.currentThread().getName());
                try {
                    //进程休息1秒再执行
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
}

public class Threads {
    public static void main(String[] args) {
        //创建了两个线程
        Mythreads ci = new Mythreads();
        Mythreads cis = new Mythreads();

        ci.start();
        cis.start();

        for (int i = 0; i < 10; i++) {
            System.out.println("main"+i);
        }
    }
}
```

- Runnable接口
```java

//第二种，通过Runnable接口
public class MyRunnable implements Runnable(){
	//重写run方法
	public void run(){
		System.out.println("run"+i+Thread.currentThread().getName());
	}
}

public class Demo(){
    public static void main(String[] args) {
    	//创建自定义对象
    	MyRunnable run = new MyRunnable();

    	//创建进程对象
    	Thread td = new Thread(run);
    
    	//开启多进程
    	td.start();

    	/*
    	采用匿名对象
    	new Thread(new Runnable(){
			public void run(){
				System.out.println("run方法")
			}
    	}).start();
    	*/   	

        for (int i = 0; i < 10; i++) {
            System.out.println("main"+i);
        }
    }	
}

```

> 第一种Thread方法是继承，不灵活(只能继承一个)。
> 而使用Runnable就灵活的多了，因为它是使用接口，降低耦合性

![Java知识点汇总](http://ww1.sinaimg.cn/large/006Sc6e5ly1g4r21oak2nj31r1big1ky.jpg)






















