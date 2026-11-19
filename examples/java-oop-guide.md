# Java 面向对象编程（OOP）核心特性总览

> 面向 Java 初学者与面试复习。本文将“面向对象的核心特性”和 Java 中常见的相关机制放在同一条主线中讲清楚。

## 第 1 页：先建立整体认识

### OOP 是什么？

面向对象编程（Object-Oriented Programming，OOP）不是“把代码写进 class”这么简单，而是一种组织程序的方式：

- **对象（Object）**：拥有状态（fields）和行为（methods）的运行时实体。
- **类（Class）**：创建对象的模板，描述对象有哪些数据和行为。
- **消息/方法调用**：对象通过调用方法协作完成任务。

Java 中常见的 OOP 核心特性可以概括为：

1. **封装（Encapsulation）**：隐藏内部状态，通过受控方法暴露能力。
2. **继承（Inheritance）**：子类复用并扩展父类的状态和行为。
3. **多态（Polymorphism）**：同一个调用，在不同对象上表现出不同的行为。
4. **抽象（Abstraction）**：只暴露必要的概念和接口，隐藏实现细节。

面试中不要只背“四大特性”，还应能说明：Java 如何通过访问修饰符、抽象类、接口、重载和重写实现它们。

---

## 第 2 页：类、对象与封装

### 类和对象

```java
class BankAccount {
    private final String owner;
    private long balanceInCents;

    BankAccount(String owner, long initialBalanceInCents) {
        this.owner = owner;
        if (initialBalanceInCents < 0) {
            throw new IllegalArgumentException("initial balance cannot be negative");
        }
        this.balanceInCents = initialBalanceInCents;
    }

    public String getOwner() {
        return owner;
    }

    public long getBalanceInCents() {
        return balanceInCents;
    }

    public void deposit(long amountInCents) {
        if (amountInCents <= 0) {
            throw new IllegalArgumentException("deposit must be positive");
        }
        balanceInCents += amountInCents;
    }
}

BankAccount account = new BankAccount("Alice", 10_000);
account.deposit(2_000);
```

### 封装的关键点

- 用 `private` 隐藏可变字段，避免外部随意修改对象状态。
- 用 `public` 方法提供有意义的操作，而不是简单暴露所有字段。
- 在构造器和方法中校验不变量，例如余额不能初始化为负数。
- `final` 字段只能被赋值一次；它保证引用或基本值不能再次赋值，但不等于对象本身不可变。

### 访问修饰符

| 修饰符 | 可访问范围 |
|---|---|
| `private` | 当前类 |
| 无修饰符（package-private） | 同一 package |
| `protected` | 同一 package，以及子类 |
| `public` | 任何可见位置 |

封装的目标不是“所有字段都 private”，而是让对象负责维护自己的合法状态。

---

## 第 3 页：继承、`this` 与 `super`

### 继承

```java
class Employee {
    private final String name;

    Employee(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public String describeRole() {
        return "general employee";
    }
}

class Engineer extends Employee {
    private final String mainLanguage;

    Engineer(String name, String mainLanguage) {
        super(name);
        this.mainLanguage = mainLanguage;
    }

    @Override
    public String describeRole() {
        return "engineer using " + mainLanguage;
    }
}
```

### 继承中的重要规则

- Java 类是**单继承**：一个类只能直接继承一个父类。
- 构造子类对象时，会先执行父类构造器，再执行子类构造器。
- `super(...)` 调用父类构造器，且必须是构造器中的第一条语句。
- `super.method()` 可以调用父类版本的方法。
- `this(...)` 调用当前类的另一个构造器，也必须是构造器中的第一条语句。
- `@Override` 能让编译器检查你是否真的重写了父类方法。
- `private` 方法、`static` 方法和 `final` 方法不能按普通实例方法进行重写。

继承表达的是稳定的 **is-a** 关系，例如 `Engineer is an Employee`。如果只是复用代码，却没有真正的类型关系，通常应优先考虑组合。

---

## 第 4 页：多态、重载与重写

### 运行时多态

```java
Employee employee = new Engineer("Bob", "Java");
System.out.println(employee.describeRole());
// engineer using Java
```

变量的**编译时类型**是 `Employee`，对象的**运行时类型**是 `Engineer`。调用可重写的实例方法时，Java 根据运行时对象选择 `Engineer` 的实现，这就是动态分派（dynamic dispatch）。

### 方法重载（Overloading）

同一个类中，方法名相同但参数列表不同：

```java
class Printer {
    void print(String text) {
        System.out.println(text);
    }

    void print(int number) {
        System.out.println(number);
    }
}
```

重载主要由编译器根据参数类型决定，属于**编译时多态**。仅改变返回值类型不能构成重载。

### 方法重写（Overriding）

子类为继承来的实例方法提供新实现：

```java
@Override
public String describeRole() {
    return "engineer using " + mainLanguage;
}
```

重写要求方法签名兼容；返回类型可以是协变返回类型，访问权限不能比父类更严格。静态方法是隐藏（method hiding），不是运行时重写。

### 向上转型与向下转型

```java
Employee employee = new Engineer("Bob", "Java"); // 向上转型，安全

if (employee instanceof Engineer engineer) {    // Java 16+ 模式匹配
    System.out.println(engineer.getName());
}
```

向下转型必须确认运行时对象确实属于目标类型，否则会抛出 `ClassCastException`。

---

## 第 5 页：抽象类、接口与组合

### 抽象类：共享基础与部分实现

```java
abstract class Payment {
    public final void pay(long amountInCents) {
        if (amountInCents <= 0) {
            throw new IllegalArgumentException("amount must be positive");
        }
        doPay(amountInCents);
    }

    protected abstract void doPay(long amountInCents);
}

class CardPayment extends Payment {
    @Override
    protected void doPay(long amountInCents) {
        System.out.println("pay by card: " + amountInCents);
    }
}
```

- 抽象类不能直接实例化。
- 可以有字段、构造器、具体方法和抽象方法。
- 适合表达一组对象共享的基础状态、流程和代码。

### 接口：能力契约与多实现

```java
interface Loggable {
    void log(String message);

    default void logError(String message) {
        log("ERROR: " + message);
    }
}

class ConsoleLogger implements Loggable {
    @Override
    public void log(String message) {
        System.out.println(message);
    }
}
```

- 一个类可以 `implements` 多个接口，弥补 Java 类单继承的限制。
- 接口主要描述“能做什么”，而不是“是什么”。
- 接口可以包含抽象方法、`default` 方法、`static` 方法和常量字段。
- 接口方法通常要求实现类提供 `public` 实现。

### 组合（Composition）

```java
class OrderService {
    private final Loggable logger;

    OrderService(Loggable logger) {
        this.logger = logger;
    }

    void createOrder() {
        logger.log("order created");
    }
}
```

`OrderService` 持有 `Loggable`，而不是继承某个日志类。这种“has-a”关系更容易替换实现、测试和扩展。工程实践中常见原则是：**优先组合，而不是继承**。

---

## 第 6 页：Java OOP 中容易漏掉的机制

### `Object`、相等性与字符串表示

所有 Java 类最终都继承自 `Object`。常见方法包括：

```java
@Override
public String toString() {
    return "Employee{name='" + getName() + "'}";
}
```

- `==` 对对象通常比较引用是否相同。
- `equals()` 表达逻辑相等性，应与业务含义一致。
- 重写 `equals()` 时通常必须同时重写 `hashCode()`，否则放入 `HashMap` 或 `HashSet` 时会出现错误行为。

### `static`、`final` 和嵌套类

- `static` 成员属于类，不属于某个具体对象；静态方法不能直接访问实例字段。
- `final` 类不能被继承，`final` 方法不能被重写，`final` 变量不能再次赋值。
- 静态嵌套类不持有外部类实例；非静态内部类会隐式持有外部类实例。
- 枚举（`enum`）是类型安全的有限实例集合，也可以拥有字段、方法和构造器。

### 泛型与类型安全

```java
class Box<T> {
    private final T value;

    Box(T value) {
        this.value = value;
    }

    T get() {
        return value;
    }
}

Box<String> box = new Box<>("hello");
String value = box.get();
```

泛型让类和方法可以复用，同时在编译期检查类型。它不是 OOP 四大特性之一，但在 Java 面向对象设计中非常常见。

### 一段面试式总结

> Java 的 OOP 核心是用类和对象组织状态与行为。封装通过访问控制和方法维护对象不变量；继承用于表达稳定的 is-a 关系；多态通过重载和尤其是重写，让同一个调用可以对应不同实现；抽象通过抽象类和接口隐藏实现细节、暴露稳定契约。实际设计中还要结合组合、依赖注入、`equals/hashCode` 契约以及单继承、多接口等 Java 语言规则。

## 特性对照表

| 特性 | Java 机制 | 主要解决的问题 |
|---|---|---|
| 封装 | `private`、访问器、校验方法 | 防止状态被随意破坏 |
| 继承 | `extends`、`super` | 复用和扩展父类能力 |
| 多态 | 重写、接口引用、动态分派 | 面向抽象编程、减少分支 |
| 抽象 | `abstract`、`interface` | 隐藏实现、稳定契约 |
| 组合 | 成员对象、构造器注入 | 降低耦合、便于替换和测试 |

## QA 总结

- [x] 主题聚焦：Java OOP 核心特性及其语言机制
- [x] 覆盖封装、继承、多态、抽象
- [x] 覆盖类/对象、访问修饰符、构造器、`this`、`super`
- [x] 覆盖重载、重写、向上/向下转型
- [x] 覆盖抽象类、接口、默认方法和组合
- [x] 覆盖 `Object`、`equals/hashCode`、`static`、`final`、嵌套类、泛型
- [x] 示例代码保持为 Java 语法，并尽量体现实际设计原则
- [x] 未把“所有 Java 特性”与“OOP 核心特性”混为一谈

