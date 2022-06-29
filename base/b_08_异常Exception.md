# 异常Exception

---

## 1. 异常概述

### 1.1. 异常概念

程序在执行过程中，出现的非正常的情况，最终会导致JVM的非正常停止。<br />在Java等面向对象的编程语言中，异常本身是一个类，产生异常就是创建异常对象并抛出了一个异常对象。Java处 理异常的方式是中断处理。<br />【注意】异常指的并不是语法错误，语法错误则编译不能通过，不会产生字节码文件，根本不能运行。<br />Java的异常类在java.lang包下。

### 1.2. 异常体系

![异常](https://cdn.jsdelivr.net/gh/haifeng-ck/blogPic/202206291658365.png)

![异常1](https://cdn.jsdelivr.net/gh/haifeng-ck/blogPic/202206291706338.png)

1. 异常分为 编译期异常 和 运行时异常；
2. 运行时异常，编译期检查不出来。一般是指编程时的逻辑错误，是程序员应该避免的。java.lang.RuntimeException类及其子类是运行时异常；
3. 对于运行时异常，可以不做处理，因为这类异常很普遍，若全做处理可能会对程序的可读性和运行效率产生影响；
4. 编译期异常，是编译器要求必须处理的异常。

- 编译时期异常：checked异常。在编译时期,就会检查,如果没有处理异常,则编译失败。(如日期格式化异常)
- 运行时期异常：runtime异常。在运行时期,检查异常.在编译时期,运行异常编译器不会检测(不报错)。(如数学异常)

- Throwable中的常用方法

`public void printStackTrace()` ：打印异常的详细信息。包含了异常的类型,异常的原因,还包括异常出现的位置,在开发和调试阶段,都得使用printStackTrace。JVM默认使用该方法。

`public String getMessage()` ：获取发生异常的原因。提示给用户的时候,就提示错误原因。

`public String toString()` ：获取异常的类型和异常描述信息(不用)。

### 1.3. 常见的运行时异常

1. `NullPointerException` ：空指针异常
1. `ArithmeticException` ：数学运算异常
1. `ArrayIndexOutOfBoundsException` ：数组下标越界异常
1. `ClassCastException` ：类型转换异常
1. `NumberFormatException` ：数字格式不正确异常

### 1.4. 常见的编译期异常

1. `SQLException` ：操作数据库时，查询表可能发生的异常
1. `IOException` ：操作文件时，发生的异常
1. `FileNotFoundException` ：操作一个不存在的文件时，发生异常
1. `ClassNotFoundException` ：加载类时，该类不存在，发生异常
1. `EOFException` ：操作文件，到文件末尾，发生异常
1. `IllegalArgumentException` ：参数异常

## 2. 异常的处理

- Java异常处理的5个关键字

try、catch、finally、throw、throws

### 2.1. 抛出异常

#### 2.1.1. throw

在指定的方法中抛出异常。throw用在方法内，用来抛出一个异常对象，将这个异常对象传递到调用者处，并结束当前方法的执行。

- 格式

`throw new xxxException("异常产生的原因")` 

- 注意
  1. throw必须在方法内部；
  1. new的必须是Exception或其子类；
  1. 运行时期异常可以不处理，有JVM进行处理；编译时期异常需要处理，throws或者try...catch；
  1. 方法中的throw将异常返回给方法的调用者，调用者对该异常进行捕获处理或者throws声明处理。

#### 2.1.2. Objects非空判断

Objects类由一些静态方法组成，这些方法是null-save（空指针安全）或null-tolerant（容忍空指针）的。在源码中，对对象为null的值进行了抛出异常操作。<br />`public static <T> T requireNonNull(T obj)` ：查看指定引用对象是否为null。

- 源码

```java
public static <T> T requireNonNull(T obj) {
	if(obj == null)
        throw new NullPointerException();
    return obj;
}
```

#### 2.1.3. 声明异常throws

声明异常，将异常交给调用者处理。

- 格式

`修饰符 返回值类型 方法名(参数列表) throws 异常类名1, 异常类名2... {}` 

- 注意
  1. throws必须在方法声明上；
  1. 声明的异常必须是Exception或其子类；
  1. 方法内部throw多个异常，throws后面也必须是多个异常；抛出的异常存在继承关系，则声明父类异常即可；
  1. 调用一个声明抛出异常的方法，则必须处理该异常。
     - 继续throws声明，交给调用者处理，最终交给JVM处理；
     - try...catch...捕获异常。

#### 2.1.4. throw 和 throws 的区别

- throws：异常处理的一种方式，用在方法声明处，后面跟 异常类型

- throw：手动生成异常对象的关键字，用在方法体中，后面跟 异常对象

### 2.2. 捕获异常try...catch

Java中对异常进行捕获的语句，可以对出现的异常进行指定方式的处理。

- 格式

```java
try {
	// 可能会出现异常的语句
}catch (异常类型 e) {
	// 处理异常的代码
    // 记录日志/打印异常信息/继续抛出异常
}
```

- 注意
  1. try和catch必须一起使用；
  1. try中抛出多个异常，可以用多个catch处理；
  1. 执行顺序
     - try中异常 --> 执行catch --> 执行后续代码
     - try中没有异常 --> 执行catch后面的代码

#### 2.2.1. finally代码块

在finally代码块中写一些无论是否发生异常都必须要执行的代码。如：在try语句块中打开了一些物理资源(磁盘文件/网络连接/数据库连接等)，无论是否发生异常，最终都必须关闭这些资源。

- 格式

```java
try {
	// 可能出现异常的语句
}catch (异常类型 e) {
	// 异常处理语句
}finally {
	// 最终需要执行的语句
}
```

- 注意
  1. finally不能单独使用；
  1. 一般用于释放资源；
  1. 避免在finally中使用return（永远都会返回finally中的结果）；
  1. 只有在try或者catch中调用退出JVM的相关方法，finally才不会执行，否则finally永远都会执行。

## 3. 异常注意事项

- 对于编译期异常，程序中必须处理，比如 `try-catch` 或者 `throws`

- 对于运行时异常，如果程序中没有处理，默认就是 `throws` 处理方式。

### 3.1. 多异常处理

1. 多个异常分别处理；
1. 多个异常一次捕获，多次处理；
1. 多个异常一次捕获，一次处理。

一般情况下使用一次捕获，多次处理的方式：

```java
try{
    // 编写可能会出现异常的代码
}catch(异常类型A e){ // 当try中出现A类型异常,就用该catch来捕获.
    // 处理异常的代码
    //记录日志/打印异常信息/继续抛出异常
}catch(异常类型B e){ // 当try中出现B类型异常,就用该catch来捕获.
    // 处理异常的代码
    // 记录日志/打印异常信息/继续抛出异常
}
```

【注意】这种处理方法中，要求多个catch中的异常不能相同；如果多个异常之间存在父子类关系，则子类异常必须在上面的catch中处理，父类异常在下面的catch中处理。

### 3.2. 方法重写时的异常处理

- 父类方法声明了异常：子类方法重写时可以抛出父类相同异常或其子类或者不抛出异常；
- 父类方法没有声明异常：子类方法重写时的异常只能使用try...catch捕获处理。

## 4. 自定义异常

### 4.1. 概述

在开发中根据自己业务的异常情况来定义异常类。

- 编译期异常：自定义类，并继承 `java.lang.Exception` 类；
- 运行时异常：自定义异常，并继承 `java.lang.RunTimeException` 类；
- 一般继承运行时异常 `RuntimeException` ，这样就可以使用默认的处理机制，比较方便。
- 格式

```java
public class xxxException extends Exception/RunTimeException {
	// 空参构造
    // 带异常信息的构造
}
```

【注意】类名一般以Exception结尾。

### 4.1. 自定义异常案例

自定义登录异常（编译期异常）

```java
public class LoginException extends Exception {
    public LoginException() {
    }

    public LoginException(String message) {
        super(message);
    }
}
```

```java
public class Login {
    private static String[] names = {"张三", "李四", "王五"};

    public static void main(String[] args) {
        try {
            String name = "小师妹";
            checkUserName(name);
            System.out.println(name + " 登录成功！");
        } catch (LoginException e) {
            e.printStackTrace();
        }
    }

    public static void checkUserName(String name) throws LoginException {
        boolean flag = false;
        for (String s : names) {
            if (s.equals(name)) {
                flag = true;
                break;
            }
        }
        if (!flag) {
            throw new LoginException(name + " 尚未注册！");
        }
    }
}
```
