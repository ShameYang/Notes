# Java 8 新特性

- Lambda 表达式
- 函数式接口
- 方法引用
- 接口默认方法
- Stream API
- Optional 类
- 新日期时间 API
- Nashorn 引擎















# Lambda 表达式

> Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。
>
> Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。
>
> 使用 Lambda 表达式可以使代码变的更加简洁紧凑。
>
> 
>
> 本质：函数式接口的实例
>
> 之前用匿名实现类表示的现在都可以用 Lambda 表达式来写





## 语法

`(parameters) -> expression`



或



`(parameters) -> { statements; }`





特征

- 左边 - lambda 形参列表
  - 参数类型：可以不声明，由编译器识别
  - 参数圆括号：只有一个参数时，圆括号可省略
- 右边 - lambda 体
  - 大括号：只有一条语句时，大括号可省略
  - return 关键字：主体只有一条语句（可能是 return 语句）时，可以省略 return



## 实例

```java
// 1. 不需要参数,返回值为 5  
() -> 5  
  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s)
```

[代码实例](##方法引用实例)













# 函数式(Functional)接口

> 如果一个接口，只有一个抽象方法，则此接口称为“函数式接口”
>
> 我们可以在接口上使用 @FunctionalInterface 注解，用来检验一个接口是否为函数式接口





## 四大核心函数式接口

- Consumer<T>      void accept(T t)
- Supplier<T>         T get()
- Function<T, R>     R apply(T t)
- Predicate<T>        boolean test(T t)















# 方法引用

> 方法引用本质上就是 Lambda 表达式
>
> 通过方法的名字指向一个方法





## 使用格式

> 类(或对象) :: 方法名（类比静态成员与非静态成员的调用）
>
> 
>
> 1和2 抽象方法的形参列表和返回值类型要与方法引用的形参列表和返回值类型相同

1. 对象 :: 非静态方法
2. 类 :: 静态方法
3. 类 :: 非静态方法





## 方法引用实例

```java
public class MethodRefTest {
    //情况一 对象::实例方法
    //Consumer 中 void accpet(T t)
    //PrintStream 中 void println(T t)
    @Test
    public void test1() {
        //之前的
        Consumer<String> con = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        con.accept("aaa");
        
        //Lambda 表达式
        Consumer<String> con1 = s -> System.out.println(s);
        con1.accept("abc");
        
        //方法引用
        PrintStream ps = System.out;
        Consumer<String> con2 = ps::println;
        con2.accept("def");
        //结合
        Consumer<String> con3 = System.out::println;
        con3.accept("ghi");
    }
    
    //情况二 类::静态方法
    //Comparator 中 int compare(T t1, T t2)
    //Integer 中 int compare(T t1, T t2)
    @Test
    public void test2() {
        //Lambda 表达式
        Comparator<Integer> com1 = (t1, t2) -> Integer.compare(t1, t2);
        System.out.println(com1.compare(12, 21));
        
        //方法引用
        Comparator<Integer> com2 = Integer::compare;
        System.out.println(com1.compare(21, 12));
    }
    
    //情况三 类::实例方法
    //Comparator 中 int compare(T t1, T t2)
    //String 中 int t1.compareTo(t2)
    @Test
    public void test3() {
        //Lambda
        Comparator<String> com1 = (s1, s2) -> s1.compareTo(s2);
        System.out.println(com1.compare("abc", "def"));
		
        //方法引用
        Comparator<String> com2 = String :: compareTo;
        System.out.println(com2.compare("abc", "def"));
    }
}
```





## 构造器引用

> 语法：`Class::new`

```java
public class ConstructorRefTest {
    //构造器引用
    //Supplier 中的 get()
    //Employee 中的无参构造器
    @Test
    public void test1() {
        //before
        Supplier<Employee> sup = new Supplier<Employee>() {
            @Override
            public Employee get() {
                return new Employee();
            }
        };
        sup.get();

        //Lambda
        Supplier<Employee> sup1 = () -> new Employee();
        sup1.get();

        //MethodRef
        Supplier<Employee> sup2 = Employee::new;
        sup2.get();
    }
}
```















# 默认方法

> Java 8 新增了接口的默认方法。
>
> 简单说，默认方法就是接口可以有实现方法，而且不需要实现类去实现其方法。
>
> 我们只需在方法名前面加个 default 关键字即可实现默认方法。
>
> 
>
> 参考文档：[菜鸟教程](https://www.runoob.com/java/java8-default-methods.html)





## 语法

```java
public interface Vehicle {
    default void print() {
        System.out.println("我是一辆车!");
    }
    
    //静态方法
    static void blowHorn() {
        System.out.println("按喇叭!!!");
    }
}
```

















# Stream API

> Stream 是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列
>
> Stream 关注的是对数据的运算，与 CPU 相关；集合关注的是数据的存储，与内存相关
>
> 参考文档：[菜鸟教程](https://www.runoob.com/java/java8-streams.html)







## Stream 操作三个步骤

1. 创建 Stream

2. 中间操作

3. 终止操作（终端操作）

   执行终止操作后，才执行中间操作，“延迟操作”





## 实例化创建流

- 通过集合
  - default Stream<E> stream() - 返回一个顺序流（串行流）
  - default Stream<E> parallelStream() -  返回一个并行流 
- 通过数组
  - 调用 Arrays 类的 static <T> Stream<T> stream(T[] array) - 返回一个流
- 使用 Stream 的 of()
  - public static<T> Stream of(T... values) - 返回一个流
- 创建无限流
  - public static<T> Stream<T> iterate(final T seed, fianl UnaryOperator<T> f)
  - public static<T> Stream<T> generate(Supplier<T> s)





## 中间操作

> 延迟执行，只有执行终止操作后，中间操作才会执行



- 筛选与切片

  - filter(Predicate p)

    接收 Lambda，通过设置的条件过滤出元素

  - limit(n)

    截断流，是元素不超过给定数量

  - skip(n)

    跳过元素，返回一个去除前 n 个元素的流。若元素不足 n 个则返回空流

  - distinct()

    筛选，通过流生成元素的 hashCode() 和 equals() 去除重复元素

- 映射

  - map(Function f)

    映射每个元素到对应的结果

  - flatMap(Function f)

- 排序

  - sorted() - 自然排序（注意：调用该方法的类需要实现 Comparable 接口）
  - sorted(Comparator com) - 定制排序





## 终止操作

- 匹配与查找

  - allMatch(Predicate p)
  - anyMatch(Predicate p)
  - noneMatch(Predicate p)
  - findFirst()
  - findAny()
  - count() - 返回流中元素总个数
  - max(Comparator c)
  - min(Comparator c)
  - forEach(Consumer c) - 内部迭代

- 归约

  - reduce(T iden, BinaryOperator b)

    将流中元素反复结合，得到一个值，返回 T

  - reduce(BinaryOperator b)

    返回 Optional<T>

  ```java
  //计算1-5的和
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
  Integer sum = list.stream().reduce(0, Integer::sum);
  ```

- 收集

  - collect(Collector c)

    将流转化为其他形式。接收一个 Collector 接口的实现，用于给 Stream 中元素做汇总

- Collectors 类

  Collectors 类实现了很多归约操作，例如将流转换成集合

  ```java
  List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
  List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
  ```

















# Optional 类

> Optional<T> 类是个容器类：它可以保存类型 T 的值，或者仅仅保存 null
>
> Optional 提供很多有用的方法，这样我们就不用显式进行空值检测
>
> Optional 类的引入很好的解决空指针异常





## 常用方法

- 创建 Optional 类对象

  - Optional.of(T t)

    创建一个 Optional 实例，t 必须非空

  - Optional.empty()

    创建一个空的 Optional 实例 

  - Optional.ofNullable(T t)

    t 可以为 null

- 判断 Optional 容器中是否包含对象

  - boolean isPresent()

    判断是否包含对象

  - void idPresent(Comsumer<? super T> consumer)

    如果有值，就执行 Consumer 接口的实现代码，并且该值会作为参数传递

- 获取 Optional 容器的对象

  - T get()

    有值则返回，否则抛出异常

  - T orElse(T other)

    有值则返回，否则返回指定的 other 对象

  - T orElseGet(Supplier<? extends T> other)

    有值则返回，否则返回实现了 Supplier 接口实现提供的对象

  - T orElse Throw(Supplier<? extends X> exceptionSupplier)

    有值则返回，否则抛出 Supplier 接口实现提供的日常





## 实例

```java
import java.util.Optional;
 
public class Java8Tester {
   public static void main(String args[]){
   
      Java8Tester java8Tester = new Java8Tester();
      Integer value1 = null;
      Integer value2 = new Integer(10);
        
      // Optional.ofNullable - 允许传递为 null 参数
      Optional<Integer> a = Optional.ofNullable(value1);
        
      // Optional.of - 如果传递的参数是 null，抛出异常 NullPointerException
      Optional<Integer> b = Optional.of(value2);
      System.out.println(java8Tester.sum(a,b));
   }
    
   public Integer sum(Optional<Integer> a, Optional<Integer> b){
    
      // Optional.isPresent - 判断值是否存在
        
      System.out.println("第一个参数值存在: " + a.isPresent());
      System.out.println("第二个参数值存在: " + b.isPresent());
        
      // Optional.orElse - 如果值存在，返回它，否则返回默认值
      Integer value1 = a.orElse(new Integer(0));
        
      //Optional.get - 获取值，值需要存在
      Integer value2 = b.get();
      return value1 + value2;
   }
}
```

















# 新的日期时间 API

> Java 8 提供了新的 Date-Time API，进一步加强对日期时间的管理
>
> 针对之前的日期时间 API 的不足，在 java.time 包下提供了许多新的 API
>
> 新的java.time包涵盖了所有处理日期，时间，日期/时间，时区，时刻（instants），过程（during）与时钟（clock）的操作





两个比较重要的 API：

- Local（本地）- 简化了日期时间的处理，没有时区的问题
- Zoned（时区）- 通过制定的时区处理日期时间
