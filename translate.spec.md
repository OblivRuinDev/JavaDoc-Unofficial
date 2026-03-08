# Java 翻译术语规范

本文档定义 Java 中常见术语的中文翻译规则，旨在保持翻译的一致性和专业性。  
如有异议或希望补充新术语，欢迎通过 Issue 或 PR 讨论。

## 常见术语

### 一、 成员 (Member)
**定义：** 类或接口中声明的所有组成部分，包括字段、方法、成员类、成员接口、枚举常量等。
```java
public class classA {
    public String memberField = "我是成员变量";
    public static String staticMemberField = "我是成员变量";
    public void memberMethod() {
        System.out.println("我是成员方法");
    }
    public void staticMemberMethod() {
        System.out.println("我是成员方法");
    }
    public class innerClassB {
        // 我是成员类
    }
    public static class innerClassC {
        // 我是成员类
    }
}
```
  1. 成员字段
  2. 成员方法
  3. 成员类

### 二、 字段 (Field)
**定义：** 指在类或接口中声明的变量，如下方代码所示：
```java
public class classD {
    public String               instanceField = "我是实例字段";
    public final String      instanceConstant = "我是实例常量，属于实例字段";
    public static String          staticField = "我是静态字段";
    public static final String staticConstant = "我是静态常量，属于静态字段";
}
public interface interfaceE {
    // 接口中的所有字段默认强制修饰为 public static final ,即静态常量
    String staticConstant = "我是静态常量";
}
```

- 实例字段 (Instance Field)：无`static`修饰<br>
  每个对象都拥有一份独属于自己的该字段。
  - 在此基础上，如该字段被`final`修饰，可称其为`实例常量`，**但不可称为`常量`**

- 静态字段：有`static`修饰
  静态字段不属于对象，而是属于类。
  - 在此基础上，如该字段被`final`修饰，可称其为`静态常量`, 也可称为`常量`。
    - 此时，若该字段类型为`boolean` `byte` `char` `double` `float` `int` `long` `short` `java.lang.String`
      中一种，且通过等号直接赋值字面量，则为编译时常量，否则为运行时常量。

> [!NOTE]
> 在中文社区中，有时又称`字段`为`成员变量`<br>
> 为避免歧义，应统一使用`字段`

### 三、 方法 (Method)

### 四、 形式参数 (Formal Parameter)

### 五、 枚举(Enum)
