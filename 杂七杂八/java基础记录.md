## 进制

> JDK7以上版本规则：


- 二进制书写由`0b` 开头
- 十进制为默认，不需要任何前缀
- 八进制书写由`0` 开头
- 十六进制书写由`0x` 开头

## 关键字

### static

> 静态方法不能使用`this` 、`super` 关键字，`static`的方法仅能调用`static` 修饰的变量


- 
调用方式可使用类名调用和对象名调用，推荐类名调用（语义更清楚）

- 
用于工具类，不需要内部成员变量的方法、不需要实例化对象的类方法

   - 示例：

```java
public class LearnUtils {
    private LearnUtils() {
    }

    public static void GetNumberMax() {
        System.out.println("GetNumberMax");
    }
}
```

### final

> final关键字可以用来修饰类、方法和变量（包括成员变量和局部变量）


> 当用final修饰一个类时，表明这个类不能被继承。也就是说，如果一个类你永远不会让他被继承，就可以用final进行修饰。final类中的成员变量可以根据需要设为final，但是要注意final类中的所有成员方法都会被隐式地指定为final方法。


## 基本数据类型

![](https://secure2.wostatic.cn/static/auRg1PNdNYfyjgpF5VJi2b/image.png?auth_key=1677039943-7m8HKgV3UfeeSGADAsH9KM-0-b74411396ddf2720617056bfa288c8c6#alt=)

### 注意事项

`long` 类型定义数据值要加上`L` 或者`l`

定义`float` 数据类型时数据值需加`F` 或者`f`

> java的数据类型分为：基本数据类型、引用数据类型。 证书和小数取值范围：double>float>long>int>short>byte


## 键盘输入监听

### Scanner

```java
import java.util.Scanner;

public class LearnScanner {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int x = scanner.nextInt();
        System.out.println(x);
    }
}
```

> 在运行时，代码会在第六行`int x = scanner.nextInt();` 处等待输入


## 数据类型

### 基础数据类型

- `int`整数型
- `float` 单精度浮点型
- `boolean` 布尔型
- `char`字符型
- `boolean`布尔型
- `long` 长整型
- `double` 双精度浮点型
- `short` 短整型

### 引用数据类型

- 数组
- 类
- 接口
- 字符串

> 引用数据类型变量实际是指针，java中没有指针概念而已。引用数据类型默认为null


## 数据类型转换

### 隐式转换

示例：

```java
public class TakeMold {
    public static void main(String[] args) {
        int a = 12;
        double b = 21.1;
        System.out.println(c);   //double c = a + b;
    }
}
```

> 隐式转换规则一：为取值范围从大到小：byte>short>int>long>float>double 隐式转换规则二：byte、short、char在运算时会先提升为int，然后再运算


### 强制转换

规则：目标数据类型变量名 = (目标数据类型) 被强转的数据

示例：

```java
public class TakeMold {
    public static void main(String[] args) {
        System.out.println("--------------------------------");
        double a = 123;
        int b = 321;
        int c = (int) a + b;
        System.out.println(c);
    }
}
```

## 运算符

### 逻辑运算符

![](https://secure2.wostatic.cn/static/jzJEiQmMQicJLvX2Ltx22L/image.png?auth_key=1677039943-szBHkxSNsw8fhPa8KwaDc1-0-5df7ca141913757f05333f8aabdedb11#alt=)

### 短路逻辑运算符

![](https://secure2.wostatic.cn/static/6jCcrfV8YkPaeHk57TkX1J/image.png?auth_key=1677039943-pkHGrEgMLNDWXZpsyACsQq-0-5f65922222f2776bfb0c6a333de61ef2#alt=)

`||` 两边都为`FALSE` 结果才是`FALSE`

`&&` 两边都为`TRUE` 结果才是`TRUE`

### 三元运算符

> 格式：关系表达式？表达式1：表达式2；


## 数组

### 基础概念

- 是一种容器，可以用来存储同种数据类型的多个值
- 数组容器在存储数据的时候，需要结合隐式转换考虑。

### 初始化

- 直接初始化内容：

```java
int[] list = {1, 3, 4, 5};
```

- 初始化创建Array

```java
String[] str = new String[20];
```

## 方法的重载

> 在同一个类中，定义了多个同名的方法，这些同名的方法具有相同的功能 每个方法具有不同的参数类型或参数个数，这些同名的方法，就构成了重载关系


> 同一个类中，方法名相同，参数不同的方法。与返回值无关


- 触发重载的规则

> 参数不同；个数不同；类型不同；顺序不同


示例：

```java
// 方法的重载
public class HeavyLoad {
    public static String Sun() {
        return "zero";
    }

    public static String Sun(String o) {
        return "hello:" + o;
    }
}
```

## 包装类

> 集合无法直接添加基础数据类型，可通过包装类添加


> 所有的包装类型都是不变类


```

基本类型     对应的引用类型
boolean     java.lang.Boolean
byte        java.lang.Byte
short       java.lang.Short
int         java.lang.Integer
long        java.lang.Long
float       java.lang.Float
double      java.lang.Double
char        java.lang.Character
```
