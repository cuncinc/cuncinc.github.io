---
date: 2018-10-26 17:40
status: public
tag: 技术
title: 【Java】构造函数
---

之前学的类、对象和访问控制符等概念，以后如果有时间会再补上。这一节的主题是构造函数。

----

####引入
创建一个类A来创建一个对象，对其进行初始化，可以用如下代码实现。
```
//Java
class Test_1
{
    public static void main(String[] args)
    {
        A obj = new A();
        obj.set(1, 2);
        obj.show();
    }
}

class A
{
    private int i;
    private int j;

    public void set(int a, int b)
    {
        i = a;
        j = b;
    }
    public void show()
    {
        System.out.printf("i=%d  j=%d \n", i, j);
    }
}
```
运行结果如下
```
i=1  j=2
```
这在创建一个对象时的代码。但是在我们想创建多个对象并对对象进行初始化，如果还是按照以上的方法，会显得略微繁琐，有没有什么更好的方法呢？这时，我们就引进了构造函数( constructor )。

----

####介绍
构造函数是在Java类中的一种用于初始化对象的方法，并非真的是一个函数(当然，把它看成函数是可以的)，它只会在对象被初始化的时候执行一次且仅此一次。它也不是只能用来初始化对象，它就是一个方法，一个只有在对象被构建才会执行的方法。

---

####语法与使用
用构造函数来对以上例子进行改写。
```
//Java
class Test_2
{
    public static void main(String[] args)
    {
        A obj = new A(1, 2);
        obj.show();
    }
}

class A
{
    private int i;
    private int j;

    public A(int a, int b)
    {
        i = a;
        j = b;
        System.out.printf("constructor \n");
    }
    
    public void show()
    {
        System.out.printf("i=%d  j=%d \n", i, j);
    }
}
```
运行结果如下：
```
constructor
i=1  j=2
```
可以很直观地看出例子1与例子2有哪些不同：
在类 A 中，Test_2 比 Test_1 多了一个构造函数，即 pubic A 方法；
2的输出结果比1多了一句，即 counstructor ，且其obj对象的值与例子1相同 这说明2中的 public A 程序块被执行；
在new语句中，例子2比例子1多了两个参数。

事实上，构造函数亦可不接收参数：
```
//Java
class Test_3
{
    public static void main(String[] args)
    {
        A obj = new A();
    }
}

class A
{
    private int i;
    private int j;

    public A()
    {
        System.out.printf("constructor \n");
    }
}
```
```
constructor
```
而且，在类 A 中还可以同时存在两个构造函数：
```
//Java
class Test4
{
    public static void main(String[] args)
    {
        A obj_1 = new A(1, 2);
        A obj_2 = new A(1);
        A obj_3 = new A();
    }
}

class A
{
    private int i;
    private int j;

    public A(int a, int b)
    {
        i = a;
        j = b;
        System.out.printf("有2个参数：%d %d \n", i, j);
    }
    public A(int a)
    {
        i = a;
        System.out.printf("有1个参数：%d \n", i);
    }
    public A()
    {
        System.out.printf("无参数 \n");
    }
}
```
```
有2个参数：1 2
有1个参数：1
无参数
```
可见，虽然存在多个构造函数，但在使用 new 方法时，程序并不能随意执行，而是根据参数的个数来执行参数个数相同的构造函数。
若有多个构造函数，程序自动识别出 new 方法有参数还是没有参数，也能识别出参数的个数，并访问相应参数个数的构造函数。而若无相应构造函数，则程序报错：
```
//Java      //error
class Test5
{
    public static void main(String[] args)
    {
        A obj_1 = new A(1, 2, 3);   //error，无相应构造函数
        A obj_2 = new A(1);
        A obj_3 = new A();
    }
}

class A
{
    private int i;
    private int j;

    public A(int a, int b)
    {
        i = a;
        j = b;
        System.out.printf("有两个参数：%d %d \n", i, j);
    }
    public A(int a)
    {
        i = a;
        System.out.printf("有一个参数：%d \n", i);
    }
    public A()
    {
        System.out.printf("无参数 \n");
    }
}

```
编译信息：
```
5: 错误: 对于A(int,int,int), 找不到合适的构造器
    A obj_1 = new A(1, 2, 3);
                   ^
    构造器 A.A(int,int)不适用
      (实际参数列表和形式参数列表长度不同)
    构造器 A.A(int)不适用
      (实际参数列表和形式参数列表长度不同)
    构造器 A.A()不适用
      (实际参数列表和形式参数列表长度不同)
1 个错误
错误: 编译失败
```



由此，可以总结出Java中构造函数的使用规则：
1.  构造函数的名称为类的名称
2.  构造函数无返回值
3.  构造函数可以接收参数，也可以不接收参数
4.  构造函数可以有多个，**但是永远都只会执行与 new 方法参数个数相同的那一个**

----

####注意

>当使用 new 方法创建一个对象时，系统先为该对象分配内存，然后立即自动调用该对象的构造函数


因此，对象的在定义类时的赋值会被构造函数的赋值覆盖，也就是说。在定义时给变量赋值，创建对象时变量的值会被构造函数的值覆盖。因为用 new 方法声明对象，先定义内存，再初始化，才执行构造函数。
```
//Java
class Test6
{
    public static void main(String[] args)
    {
        A obj = new A(1, 2);
        obj.show();
    }
}

class A
{
    private int i = 3;
    private int j = 4;

    public A(int a, int b)
    {
        i = a;
        j = b;
    }
    public void show()
    {
        System.out.printf("%d %d", i, j);
    }
}
```
```
1 2
```

----

####小结
1.  构造函数用于在对对象定义的同时，完成初始化
2.  在产生对象的同时，执行构造函数
3.  构造函数名为类名
4.  要发送的参数写在 new 方法处
5.  若存在多个构造函数，执行参数个数一致的那个；若无此构造函数，则报错
6.  声明对象，先定义内存，再初始化，才执行构造函数