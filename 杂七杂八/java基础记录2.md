## 面向对象基础

### 修饰符

- private（内部变量）
- public（公共变量）

### 构造方法

> 方法名与类名必须完全一致、没有返回值，包括`void` 、没有具体的返回值，不能携带return。用于初始化对象作用。


- 调用时机为创创建对象的时候调用，每次创建对象都会调用一次构造方法

> 默认情况创建类会自动添加无参构造方法，如果自己写了有参构造方法则默认不添加无参构造需要手写


## 标准的javabean类

- 类名见名知意
- 成员变量全部`private`修饰符
- 提供至少两个构造方法
- 成员方法要对应getXxx和setXxx

## 成员变量和局部变量

## String类

### 字符串比较

new创建的字符串和直接赋值的变量所存储在内存的不同区域，所以使用`==` 结果为`FALSE` ，如果要比较字符串内容可使用equals（区分大小写）或者equalsIgnoreCase（不区分大小写）

示例：

```java
String a = "hello";
String b = new String("hello1");
System.out.println(a.equals(b));
```

### 注意事项

- 创建字符串后变量不可变
- 定义在`java.lang`包下，所以不需要导入

### StringBuilder

## 封装

> 只要隐藏了全部类的变量使用方法操作就是封装


> 隐藏对象的属性和实现细节，仅对外公开访问方法，控制在程序中属性的读和写的访问级别。


> 增强安全性和简化编程，使用者不必了解具体的实现细节，而只要通过对外公开的访问方法，来使用类的成员。


示例：

```java
public class Phone {
    private String name;

    public void SetName(String name) {
        this.name = name;
    }

    public void GetName() {
        System.out.println(this.name);
    }
}
```

## 继承

- 继承使用`extends` 关键字继承父类
- Java只支持单继承，不支持多继承，但支持多层继承。
- 子类继承父类方法只能继承虚方法，非static、final、private
- 子类调用父类的成员变量时，使用`super` 关键字调用。调用使用递归向上层查找成员，super为一直向上级查找，如父级和父级的父级都有同名变量则只能查找到父级

```java
System.out.println(super.name);
```

### 方法重写

- 需要添加`@Override` ，代码安全，更优雅
- 子类重写父类方法时，访问权限子类必须大于等于父类（暂时了解︰空着不写<protected< public)
- 子类重写父类方法时，返回值类型子类必须小于等于父类
- 重写方法的名称、形参列表必须与父类中的一致。
- 私有方法不能被重写
- 子类不能重写父类的静态方法，会报错

### super()

> 用于调用父类的构造函数


### this

> 理解为一个变量，表示当前方法调用者的地址值;


### super

> 代表父类的存储空间


## 多态

#### 多态的前提（继承的多态）

- 有继承的关系
- 有父类引用指向子类对象
- 有方法重写

> 父类方法必须存在调用的方法，在方法重写时，才会在多态中调用重写的方法


## 包和final

### package

- 使用同一个包中的类时，不需要导包
- 使用`java.lang` 包中的类时，不需要导包
- 如果同时使用两个包中的同名类时，需要使用全类名

### final

> 可用于修饰一个类、方法、变量。


- 方法：表示不能被重写的方法
- 类：表示不能被继承
- 变量：表示不能被修改；常量

## 内部类

> 类的五大成员：属性、方法、构造方法、代码块、内部类


> 写在一个类的里面的类叫内部类


示例（非静态内部类）：

```java
//调用
Person.ZeroWoman z = new Person().new ZeroWoman();
//内部类
package polymorphic;

public class Person {
    String name = "zero1";

    class ZeroWoman {
        String name = "zero2";

        public void getName() {
            System.out.println(name);
        }

    }
}
```

### 静态内部类

> 静态内部类是一种特殊的成员内部类


### 局部内部类

### 匿名内部类

## 抽象类

> 使用`abstract` 修饰


### 注意事项

- 
抽象类不能实例化

- 
抽象类中不一定有抽象方法，有抽象方法的类一定是抽象类

- 
可以有构造方法

- 
抽象类的子类

   - 要么重写抽象类的所有抽象方法
   - 要么抽象类

## 接口

### 定义接口

```java
public interface <interface name>{}
```

### 注意

- 接口没有构造方法
- 成员只能是常量：`final`修饰
- 成员方法只能是抽象方法：`abstract` 修饰
- JDK7之前接口中只能定义抽象方法
- JDK8、9分别新增可以定义有方法体的方法和定义私有方法

### 接口的默认方法

JDK8新增

格式：

```java
public interface Person {
    public default void show() {
        System.out.println("hello");
    }
}
```

### 接口的静态方法

JDK8新增

> 允许在接口中定义定义静态方法，需要用static修饰


- 
接口中静态方法的定义格式：

   - 格式：public static返回值类型方法名(参数列表)（}
   - 范例: public static void show( (}
- 
接口中静态方法的注意事项：

   - 静态方法只能通过接口名调用，不能通过实现类名或者对象名调用
   - public可以省略，static不能省略

### 接口的私有方法

JDK9新增

> 私有方法分为普通私有方法和静态私有方法


格式1:private i 返回值类型方法名(参数列表){ } 范例1： private void show(){ }

格式2：private static返回值类型方法名(参数列表){ } 范例2: private static void method(){ }

## 常用Api

### Runtime

## 适配器设计模式

> 当一个接口中的抽象方法过多，但只需要使用一部分的时候，就可以使用适配器设计模式


## Object

> 空参构造


### 成员方法

- toString
- equals
- clone

### 隐式转换自动提升规则

- 从小到大排列

```
byte——short——int——long——float——double
```
