## Java基础知识学习

[TOC]



### 1.Java基本数据类型

| 数据类型     | 关键字  | 内存占用（单位字节） |
| ------------ | ------- | -------------------- |
| 字节         | byte    | 1                    |
| 短整型       | short   | 2                    |
| 整形         | int     | 4                    |
| 长整型       | long    | 8                    |
| 单精度浮点数 | float   | 4                    |
| 双精度浮点数 | double  | 8                    |
| 字符         | char    | 2                    |
| 布尔         | boolean | 1                    |

### 2.类型转换

#### 2.1自动类型转换

```java
int a = 1;
byte b = 2;
```

a+b时，b会产生**自动类型转换**

所谓自动类型转换，是指：在某些时候变量会由范围小的到范围大的自动转换的过程。

> byte/boolean-->short/chart-->int/float-->long/double

#### 2.2强制类型转换

强制类型转换是指，范围大的向范围小的类型转换的过程。

#### 2.3包装类

##### 2.3.1基本类型对应的包装类：

| 基本类型 | 对应的包装类  |
| -------- | ------------- |
| byte     | Byte          |
| short    | Short         |
| int      | **Integer**   |
| long     | Long          |
| float    | Float         |
| double   | Double        |
| char     | **Character** |
| boolean  | Boolean       |

##### 2.3.2装箱与拆箱

- 装箱：基本数据类型->包装类

  构造函数：

  ​	`public Integer(int value)`

  ​	`public Integer(String s)`：s必须为数字类型的字符串，否则报错

  静态方法：

  ​	`static Integer valueOf(int i)`

  ​	`static Integer valueOf(String s)`

  代码示例：

```
Integer i = new Integer(4);
Integer i2 = Integer.valueOf(4);

Integer i = new Integer("4");
Integer i2 = Integer.valueOf("4");
```

- 拆箱：包装类->基本数据类型

  常用方法：

  ​	`public int intValue();`

  代码示例：

```
int num = i.intValue();
```

##### 2.3.3自动装箱与自动拆箱

JDK1.5之后基本数据类型和包装类之间可以完成自动装箱与拆箱。如下代码：

```
Integer i = 4;//基本数据类型自动转换为包装类
i = i + 1;//参与加法运算，转换为包装类
```

##### 2.3.4基本类型和字符串之间的转换

**String-->基本类型** 

除了Character类之外，其他所有包装类都具有parseXxx静态方法可以将字符串参数转换为对应的基本类型：

- `public static int parseInt(String s)`：将字符串参数转换为对应的int基本类型。
- `public static double parseDouble(String s)`：将字符串参数转换为对应的double基本类型。

代码示例

```
int i = Integer.parseInt("12350");
```

**基本类型-->字符串**

1.使用**+""**

2.包装类的静态方法toString(参数),不是toString的重载。

​		`static String toString(int i)`

3.String的静态方法valueOf

​		`static String valueOf(int i);`

代码示例：

```
String s = 144 + "";
String s2 = Integer.toString(114);
String s3 = String.valueOf(144);
```



### 3.数组

##### 3.1数组的定义方式

```java
int[] arr = new int[3];
int[] arr = new int[]{1,2,3};
int[] arr = {1,2,3};
```

##### 3.2数组常见的两个异常

​	数组越界异常：**ArrayIndexOutOfBoundsException**

​	数组空指针异常：**NullPointerException**

### 4.访问修饰符

| 关键字  | 访问权限                       |
| ------- | ------------------------------ |
| public  | 所有都可以访问                 |
| protect | 继承的类可以访问               |
| private | 私有，内部对象可以访问         |
| default | 默认可以被同一包中的所有类访问 |

