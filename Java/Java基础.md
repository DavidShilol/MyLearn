[toc]

## Java小常识
+ 大小写敏感
+ 类是构建所有Java应用程序的构建块，程序中的全部内容必须放置在类中
+ class后面紧跟类名，类名必须以字母开头，后面跟字母或数字的任意组合
+ 类名规范为骆驼命名法，单词首字母大写
+ 源代码文件名必须与公共类的名字相同，以“.java”作为扩展名
+ Java编译器将字节码文件自动命名为与公共类同名的“.class”文件，与源文件存储在同一目录下
+ 运行编译程序时，Java虚拟机将从指定类中的main方法开始执行，因此类的源文件中必须包含一个main方法
+ main方法必须声明为public
+ 代码块用花括号{}
+ 调用方法的通用格式为object.method(parameters)
+ 单行注释“//”，多行注释“/*”、“*/”，自动生成文档的注释“/**”、“*/”

## Java基本程序设计结构
### 数据类型
4整型(int, short, long, byte)，2浮点(float, double)，1种用于表示unicode编码的字符单元的字符类型char，1种表示真值的boolean类型。

### 常量
使用final指示常量，只能被赋值一次

### String
Java没有字符串类型，但在Java类库种提供了预定义类String，字符串用双引号括起来。

String类不提供修改字符串的方法。不可变字符串的优点：编译器可以让字符串共享。

### 大数值
java.math包提供了BigInteger和BigDecimal，实现任意精度的计算，且内置add、substract、multiply、devide、mod方法。

### 数组
声明数组：`int[] a`

创建匿名数组：`new int[] {1,2,3,4}`

### 数组拷贝
直接拷贝，两个变量引用同一个数组

`int[] a = b;`

数组的数值拷贝到新的数组

`int[] a = Arrays.copyOf(b, b.length);`

## 对象与类

### 类、对象、实例的关系

类是构造对象的模板，构造对象的过程称为创建类的实例。对象中的数据称为实例域，操作数据的过程称为方法。

### 对象三大特征
+ 对象的行为
+ 对象的状态
+ 对象标识

### 类之间的关系
+ 依赖
+ 聚合
+ 继承

### 对象与对象变量的区别

对象变量的值是对存储在另一个地方的对象的引用。对象变量不包含对象，只是引用一个对象。

### 静态域、静态常量、静态方法

+ 静态域：static定义变量，域属于类，所有对象共享这一个域。
+ 静态常量：static final定义变量，只能被赋值一次。
+ 静态方法：没有隐式参数的方法，不需要访问对象状态、只访问类的静态域时使用。

### 方法参数

Java总是采用按值调用，方法得到的是所有参数的拷贝。

### 构造器

类可以有多个构造器。多个方法拥有相同的名字、不同的参数，这样的特征叫重载。编译器挑选具体使用哪种方法，根据各方法的参数类型与调用方法所使用的值类型来匹配相应的方法，这个过程叫重载解析。

### 无参数构造器

+ 没有编写构造器，系统会提供一个无参数构造器；
+ 如果编写了构造器，则必须提供一个无参数构造器；

### 包

+ 类可以使用所属包中的所有类，以及其他包的公共类；
+ `import static java.lang.System.*`可以访问静态域和静态方法；
+ 类的源文件放置路径要与声明的包路径一致；
+ 编译器对文件（带有文件分隔符和.java的文件）进行操作，解释器加载类（带有.分隔符）；
+ 没有声明public或private，这个部分（类、方法或变量）可以被包中所有方法访问；

### 类路径
+ `java -classpath /xxxx:.:/qqq`
+ CLASSPATH

## 接口与内部类

### 接口

+ 接口不是类，是对类的一组需求描述
+ 接口中定义必要的方法，但不提供具体的实现
+ 接口中可以定义常量，但不能定义实例域

### 类实现接口

为了让类实现一个接口，需要两个步骤：
1. 将类声明为实现给定的接口（使用implements关键字声明）
2. 对接口中的所有方法进行定义

```Java
// 定义接口
public interface Comparable<T>
{
    int compareTo(T other);
}

// 类中实现接口
public class Employee implements Comparable<Employee>
{
    public int compareTo(Employee other){
        return Double.compare(salary, other.salary);
    }
}
```

*注释：接口中的方法/变量不用提供public，将会自动被设为public，但类中实现接口时需要为方法提供public，否则编译器认为该方法是包可见的。*

### 类实现多接口

```Java
public class Employee implements Cloneable, Comparable
```

### 接口与抽象类

Java只允许有一个超类，不支持多继承，但支持多接口实现。

### 接口变量引用类对象

```Java
// Employee类实现了Comparable接口
Comparable<Employee> c = new Employee();
```

### 接口是一种类型

接口不是类，但接口是类型。

接口不提供具体的方法实现，所以不是类。

接口可以定义变量，所以接口就像int、double一样，是一种类型。

## 泛型

### 定义简单泛型类

一个泛型类就是具有一个或多个类型变量的类

```Java
public class Pair<T, U>{......}
```

Pair类引入了类型变量T和U，用尖括号括起来，放在类名后面，可以有多个类型变量。

Java中，使用E表示集合的元素类型，使用K和V分别表示表的关键字与值的类型，T、U、S表示“任意类型”。

实例化泛型类型：

```Java
Pair<String, Integer>
```

### 泛型方法

```Java
class ArrayAlg
{
    public static <T> T getMiddle(T... a)
    {
        return a[a.length / 2];
    }
}
```

+ 类型变量放在修饰符之后，返回类型之前
+ 泛型方法可以在普通类中定义，可以在泛型类型中定义
+ 调用泛型方法时，在方法名前的尖括号放入具体的类型，通常也可以省略`<String>`这样的类型参数，因为编译器有足够的信息推断出所调用的方法

## 集合

### 集合接口

实现方法时，不同的数据结构会导致实现风格和性能有很大差异。Java集合类库采用接口与实现分离，使用接口类型存放集合的引用，这样只需要在构建集合时关心使用哪种实现。

```Java
Queue<Customer> expressLane = new CircularArrayQueue<>(100);
// Queue<Customer> expressLane = new LinkedListQueue<>();
expressLane.add(new Customer("Harry"));
```

## TODO
+ 面向对象
+ ~~集合~~
+ IO
+ 异常
+ 操作系统
  + 进程与线程
  + 死锁
  + 内存管理
+ 并发编程
+ 虚拟机
  + 内存
  + 垃圾回收
  + 类加载机制