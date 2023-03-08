# Java语法

## 基本概念

- JDK、JRE、JVM的关系：
  
  - JDK：Java Development Kit，Java开发工具包
  
  - JRE: Java Runtime Environment，Java运行环境
  
  - JVM：Java Virtual Machine，Java虚拟机
  
  - JDK包含JRE，JRE包含JVM

- JDK版本选择
  
  - 目前JDK1.8（也叫JDK8，注意不是JDK18）用得最多

- Java代码的编译运行流程
  
  - 将Java源码编译成Java字节码。
  
  - 使用JVM将Java字节码转化成机器码。
  
  - VM作用：跨平台、内存管理、安全。

- JSE、JEE、JME的区别
  
  - JSE: Java Standard Edition，标准版
  
  - JEE：Java Enterprise Edition，企业版
  
  - JME: Java Mirco Edition，移动版
  
  - Spring是JEE的轻量级替代品
  
  - SpringBoot是Spring + 自动化配置

## Java语法

### 变量、运算符、输入与输出

#### 内置数据类型

| 类型      | 字节数 | 数据范围                                                      | 举例             |
| ------- | --- |:---------------------------------------------------------:| -------------- |
| byte    | 1   | -2<sup>7</sup>~~2<sup>7</sup>-1(-128~127)                 | 123            |
| short   | 2   | -2<sup>15</sup>~~2<sup>15</sup>-1(-32768~32767)           | 12345          |
| int     | 4   | -2<sup>31</sup>~~2<sup>31</sup>-1(-2147483648~2147483647) | 123456789      |
| long    | 8   | -2<sup>63</sup>~~2<sup>63</sup>-1                         | 1234567891011L |
| float   | 4   | 1.401e-45~3.4e+38                                         | 1.2F           |
| double  | 8   | 4.9e-324~1.79e+308                                        | 1.2,1.2D       |
| boolean | 1   | true,false                                                | true,false     |
| char    | 2   | 0~65535                                                   | 'A'            |

#### 常量

使用**final**修饰

`final int N = 110;`

#### 类型转换

- 显示转换    
  
  ```java
  int x = (int)'A';
  ```

- 隐式转换    
  
  ```java
  double x = 12, y = 4 * 3.3;
  ```

#### 表达式

与C++、Python3类似：

```java
int a = 1, b = 2, c = 3;
int x = (a + b) * c;
x ++;
```

#### 输入

方式1，效率较低，输入规模较小时使用。

```java
Scanner sc = new Scanner(System.in);
String str = sc.next();  // 读入下一个字符串
int x = sc.nextInt();  // 读入下一个整数
float y = sc.nextFloat();  // 读入下一个单精度浮点数
double z = sc.nextDouble();  // 读入下一个双精度浮点数
String line = sc.nextLine();  // 读入下一行
```

方式2，效率较高，输入规模较大时使用。注意需要抛异常。

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String str = br.readLine();
System.out.println(str);
    }
}
```

#### 输出

方式1，效率较低，输出规模较小时使用。

```java
// 输出整数 + 换行
System.out.println(123);  
// 输出字符串 + 换行
System.out.println("Hello World");  
// 输出整数
System.out.print(123);  
// 输出字符串
System.out.print("yxc\n");  
// 格式化输出，float与double都用%f输出
System.out.printf("%04d %.2f\n", 4, 123.456D);
```

方式2，效率较高，输出规模较大时使用。注意需要抛异常。

```java
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;

public class Main {
    public static void main(String[] args) throws Exception {
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
bw.write("Hello World\n");
bw.flush();  // 需要手动刷新缓冲区
    }
}
```

### 判断语句

#### if-else语句

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int year = sc.nextInt();
        if (year % 100 == 0) {
            if (year % 400 == 0)
                System.out.printf("%d是闰年\n", year);
            else
                System.out.printf("%d不是闰年\n", year);
        } else {
            if (year % 4 == 0)
                System.out.printf("%d是闰年\n", year);
            else
                System.out.printf("%d不是闰年\n", year);
        }
    }
}
```

#### switch语句

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int day = sc.nextInt();
        String name;
        switch (day) {
            case 1:
                name = "Monday";
                break;
            case 2:
                name = "Tuesday";
                break;
            case 3:
                name = "Wednesday";
                break;
            case 4:
                name = "Thursday";
                break;
            case 5:
                name = "Friday";
                break;
            case 6:
                name = "Saturday";
                break;
            case 7:
                name = "Sunday";
                break;
            default:
                name = "not valid";
        }
        System.out.println(name);
    }
}
```

#### 逻辑运算符与条件表达式

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int year = sc.nextInt();
        if (year % 100 != 0 && year % 4 == 0 || year % 400 == 0)
            System.out.printf("%d是闰年\n", year);
        else
            System.out.printf("%d不是闰年\n", year);
    }
}
```

### 循环语句

#### while循环

```java
int i = 0;
while (i < 5) {
    System.out.println(i);
    i ++ ;
}
```

#### do while循环

```java
int i = 0;
do {
    System.out.println(i);
    i ++ ;
} while (i < 5);
```

***do while语句与while语句非常相似。唯一的区别是，do while语句限制性循环体后检查条件。不管条件的值如何，我们都要至少执行一次循环。***

#### for循环

```java
for (int i = 0; i < 5; i ++ ) {  // 普通循环
    System.out.println(i);
}

int[] a = {0, 1, 2, 3, 4};
for (int x: a) {  // forEach循环
    System.out.println(x);
}
```

### 数组

#### 初始化

初始化定长数组，长度可以是变量，可以在初始化时赋值。

```java
// 初始化长度为5的int数组，初始值为0int[] a = new int[5];  
int n = 10;
// 初始化长度为n的float数组，初始值为0.0F
float[] b = new float[n];  
// 初始化长度为3的char数组，初始值为：'a', 'b', 'c'
char[] c = {'a', 'b', 'c'};  
// d与c地址相同，更改c中的元素，d中的元素也会改变
char[] d = c;  
```

#### 数组元素的读取与写入

```java
int[] a = new int[5];

for (int i = 0; i < 5; i++) {
    a[i] = i;
}
for (int i = 0; i < 5; i ++ ) {
    System.out.println(a[i] * a[i]);
}
```

#### 多维数组

```java
int[][] a = new int[2][3];
a[1][2] = 1;
int[][] b = {
        {1, 2, 3},
        {4, 5, 6},
};
System.out.println(a[1][2]);
System.out.println(b[0][1]);
```

#### 常用API

- 属性length：返回数组长度，注意不加小括号

- `Arrays.sort()`：数组排序

- `Arrays.fill(int[] a, int val)`：填充数组

- `Arrays.toString()`：将数组转化为字符串

- `Arrays.deepToString()`：将多维数组转化为字符串

- 数组不可变长

### 字符串

#### String类

##### 初始化：

```java
String a = "Hello World";
String b = "My name is";
// 存储到了相同地址
String x = b; 
// String可以通过加号拼接
String c = b + "yxc";  
// int会被隐式转化成字符串"18"
String d = "My age is " + 18;  
// 格式化字符串，类似于C++中的sprintf
String str = String.format("My age is %d", 18);  
String money_str = "123.45";
double money = Double.parseDouble(money_str);  // String转double
```

##### 注意

只读变量，不能修改，例如：

```java
String a = "Hello ";
a += "World";  // 会构造一个新的字符串
```

访问String中的字符：

```java
String str = "Hello World";
for (int i = 0; i < str.length(); i ++ ) {
    System.out.print(str.charAt(i));  // 只能读取，不能写入
}
```

##### 常用API：

- `length()`：返回长度

- `split(String regex)`：分割字符串

- `indexOf(char c)`、`indexOf(String str)`：查找，找不到返回-1

- `equals()`：判断两个字符串是否相等，注意不能直接用==

- `compareTo()`：判断两个字符串的字典序大小，负数表示小于，0表示相等，正数表示大于

- `startsWith()`：判断是否以某个前缀开头

- `endsWith()`：判断是否以某个后缀结尾

- `trim()`：去掉首尾的空白字符

- `toLowerCase()`：全部用小写字符

- `toUpperCase()`：全部用大写字符

- `replace(char oldChar, char newChar)`：替换字符

- `replace(String oldRegex, String newRegex)`：替换字符串

- `substring(int beginIndex, int endIndex)`：返回`[beginIndex, endIndex)`中的子串

#### `StringBuilder`、`StringBuffer`

`String`不能被修改，如果打算修改字符串，可以使用`StringBuilder`和`StringBuffer`。

`StringBuffer`线程安全，速度较慢；`StringBuilder`线程不安全，速度较快。

```java
StringBuilder sb = new StringBuilder("Hello ");  // 初始化
sb.append("World");  // 拼接字符串
System.out.println(sb);

for (int i = 0; i < sb.length(); i ++ ) {
    sb.setCharAt(i, (char)(sb.charAt(i) + 1));  // 读取和写入字符
}

System.out.println(sb);
```

##### 常用API

- `reverse()`：翻转字符串

### 函数

`Java`的所有变量和函数都要定义在类中

函数或变量前加`static`表示静态对象，类似于全局变量
静态对象属于`class`，而不属于`class`的具体实例

静态函数中只能调用静态函数和静态变量。

示例：

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        System.out.println(max(3, 4));
        int[][] a = new int[3][4];
        fill(a, 3);
        System.out.println(Arrays.deepToString(a));

        int[][] b = getArray2d(2, 3, 5);
        System.out.println(Arrays.deepToString(b));
    }

    private static int max(int a, int b) {
        if (a > b) return a;
        return b;
    }

    private static void fill(int[][] a, int val) {
        for (int i = 0; i < a.length; i ++ )
            for (int j = 0; j < a[i].length; j ++ )
                a[i][j] = val;
    }

    private static int[][] getArray2d(int row, int col, int val) {
        int[][] a = new int[row][col];
        for (int i = 0; i < row; i ++ )
            for (int j = 0; j < col; j ++ )
                a[i][j] = val;
        return a;
    }
}
```

### 类与接口

#### 类

`class`与`C++`、`Python`类似。

#### 源文件声明规则

- 一个源文件中只能有一个`public`类。

- 一个源文件可以有多个非`public`类。

- 源文件的名称应该和`public`类的类名保持一致。

- 每个源文件中，先写`package`语句，再写`import`语句，最后定义类。

#### 类的定义

- `public`: 所有对象均可以访问

- `private`: 只有自己可以访问

```java
class Point {
    private int x;
    private int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void setX(int x) {
        this.x = x;
    }

    public void setY(int y) {
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    public String toString() {
        return String.format("(%d, %d)", x, y);
    }
}
```

#### 类的继承

每个类只能继承一个类

```java
class ColorPoint extends Point {
    private String color;

    public ColorPoint(int x, int y, String color) {
        super(x, y);
        this.color = color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public String toString() {
        return String.format("(%d, %d, %s)", super.getX(), super.getY(), this.color);
    }
}
```

#### 类的多态

```java
public class Main {
    public static void main(String[] args) {
        Point point = new Point(3, 4);
        Point colorPoint = new ColorPoint(1, 2, "red");

        // 多态，同一个类的实例，调用相同的函数，运行就结果不同
        System.out.println(point.toString());
        System.out.println(colorPoint.toString());
    }
}
```

#### 接口

`interface`与`class`类似。主要用来定义类中所需包含的函数。

接口也可以继承其他接口，一个类可以实现多个接口。

#### 接口的定义

```java
interface Role {
    public void greet();
    public void move();
    public int getSpeed();
}
```

#### 接口的继承

每个接口可以继承多个接口

```java
class Zeus implements Hero {
    private final String name = "Zeus";
    public void attack() {
        System.out.println(name + ": attack!");
    }

    public void greet() {
        System.out.println(name + ": Hi!");
    }

    public void move() {
        System.out.println(name + ": Move!");
    }

    public int getSpeed() {
        return 10;
    }
}
```

#### 接口的多态

```java
class Athena implements Hero {
    private final String name = "Athena";
    public void attack() {
        System.out.println(name + ": attack!");
    }

    public void greet() {
        System.out.println(name + ": Hi!");
    }

    public void move() {
        System.out.println(name + ": Move!");
    }

    public int getSpeed() {
        return 10;
    }
}

public class Main {
    public static void main(String[] args) {
        Hero[] heros = {new Zeus(), new Athena()};
        for (Hero hero: heros) {
            hero.greet();
        }
    }
}
```

#### 泛型

类似于`C++`的`template`，`Java`的类和接口也可以定义泛型，即同一套函数可以作用于不同的对象类型。
泛型只能使用对象类型，不能使用基本变量类型。

### 常用容器

#### List

接口：`java.util.List<>`

实现：

- `java.util.ArrayList<>`：变长数组

- `java.util.LinkedList<>`：双链表

函数

- `add()`：在末尾添加一个元素

- `clear()`：清空

- `size()`：返回长度

- `isEmpty()`：是否为空

- `get(i)`：获取第i个元素

- `set(i, val)`：将第i个元素设置为val

#### 栈

类：`java.util.Stack<>`

函数：

- `push()`：压入元素

- `pop()`：弹出栈顶元素，并返回栈顶元素

- `peek()`：返回栈顶元素

- `size()`：返回长度

- `empty()`：栈是否为空

- `clear()`：清空

#### 队列

接口：`java.util.Queue<>`

实现：

- `java.util.LinkedList<>`：双链表

- `java.util.PriorityQueue<>`：优先队列

- 默认是小根堆，大根堆写法：`new PriorityQueue<>(Collections.reverseOrder())`

函数：

- `add()`：在队尾添加元素

- `remove()`：删除并返回队头

- `isEmpty()`：是否为空

- `size()`：返回长度

- `peek()`：返回队头

- `clear()`：清空

#### Set

接口：`java.util.Set<K>`

实现：

- `java.util.HashSet<K>`：哈希表
- `java.util.TreeSet<K>`：平衡树

函数：

- `add()`：添加元素

- `contains()`：是否包含某个元素

- `remove()`：删除元素

- `size()`：返回元素数

- `isEmpty()`：是否为空

- `clear()`：清空

`java.util.TreeSet`多的函数：

- `ceiling(key)`：返回大于等于key的最小元素，不存在则返回null

- `floor(key)`：返回小于等于key的最大元素，不存在则返回null

#### Map

接口：`java.util.Map<K, V>`

实现：

`java.util.HashMap<K, V>`：哈希表
`java.util.TreeMap<K, V>`：平衡树
函数：

- `put(key, value)`：添加关键字和其对应的值
- `get(key)`：返回关键字对应的值
- `containsKey(key)`：是否包含关键字
- `remove(key)`：删除关键字
  `size()`：返回元素数
- `isEmpty()`：是否为空
- `clear()`：清空
- `entrySet()`：获取Map中的所有对象的集合
- `Map.Entry`<K, V>：Map中的对象类型
  
  - `getKey()`：获取关键字
  
  - `getValue()`：获取值
- `java.util.TreeMap`<K, V>多的函数：
  
  - `ceilingEntry(key)`：返回大于等于key的最小元素，不存在则返回null
  - `floorEntry(key)`：返回小于等于key的最大元素，不存在则返回null

# 小案例

## JAVA发送邮件报错：

```
javax.mail.MessagingException: Could not connect to SMTP host: smtp.qq.com, port: 465;
  nested exception is:
	javax.net.ssl.SSLHandshakeException: No appropriate protocol (protocol is disabled or cipher suites are inappropriate)
```

修改jdk1.8中的/jre/lib/security/java.security 中 

```
找到对应的SSLv3，删除掉，重启项目就好了。（删掉SSLv3就是允许SSL调用）。
注：不知道是不是不同邮箱提供商是用的是不同的协议，我连接阿里云企业邮箱 smtp.qiye.163.com:465发现去掉SSLv3没有效果，必须去掉TLSv1，TLSv1.1
```

重新运行,可以正常使用了
