# Java快速入门

---

## 1.开发步骤

1. 编写名为 `Hello.java` 的源代码文件；
2. `javac Hello.java` 命令编译该文件，生成 `.class` 字节码文件；
3. `java Hello` 命令运行生成的字节码文件。 

## 2.代码演示

```java
/*
	1. public class Hello：表示 Hello 是一个 public 公共类
	2. Hello {}：表示Hello类的开始和结束
	3. public static void main(String[] args)：表示主方法，是程序的入口
	4. main(){}：表示方法的开始和结束
	5. System.out.println("Hello Java !")：输出 “Hello Java !” 到屏幕
	6. ; 表示语句结束
*/
public class Hello {
	public static void main(String[] args) {
		System.out.println("Hello Java !");
	}
}
```

## 3.代码执行过程分析

![Java执行流程](https://cdn.jsdelivr.net/gh/haifeng-ck/blogPic/202206221448222.png)

## 4.注意事项

> 1. Java源文件以 .java 为扩展名。源文件的基本组成部分是类（class）
>
> 2. main()方法是程序的入口，固定写法：public static void main(String[] args) {...}
>
> 3. Java语言严格区分大小写
>
> 4. 方法的每一条语句以“;”结尾
>
> 5. 括号必须成对使用，缺一不可
>
> 6. 一个源文件中最多只能有一个 public 修饰的类，其他类的个数不限
>
> 7. 如果源文件中包含一个 public 类，则文件名必须和该类名一致
>
> 8. 可以将 main() 方法写在非public类中，然后指定运行 非 public类
