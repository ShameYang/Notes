#  Java概述



## 什么是程序

> 计算机执行某些操作或解决某个问题而编写的一系列有序指令的集合



## 历史

92年 Sun 公司创建 Java 语言，95年正式发布第一个版本，09年 Oracle 公司收购，11年发布Java7



## 特性

- 面向对象（OOP）

- 健壮性：强类型机制、异常处理、垃圾自动收集

- 跨平台性：编译好的 .class 文件可以在多个操作系统运行

- 解释性语言（了解）



## 运行机制及运行过程

.java文件（）👉 **javac 编译**👉 .class 文件（字节码文件）👉 **java 运行（ .class 加载到 jvm）**👉结果

### **JVM**

- **Java核心机制** - Java虚拟机（JVM）（Java Virtual Machine） 

- JVM 负责执行指令，管理数据、内存、寄存器，包含在 ==JDK== 中

- Java虚拟机机制屏蔽了底层运行平台的差别，实现了“一次 ==编译==，到处 ==运行==”

### **JRE**

- Java Runtime Environment，Java运行环境

- JRE = JVM + Java核心类库（类）

### **JDK**

- Java Development Kit，==Java 开发工具包==

- JDK = JRE + java开发工具





## 开发注意

- 源文件以.java为扩展名。源文件的**基本组成部分是类（class）**

- 执行入口main（）

- 严格区分**大小写**

- 每个语句以 ; 结束

- **大括号成对**出现

- **一个源文件中只能有一个 public 类**

- **public 类名与源文件名一致**

- **非 public 的 main 方法**

  

## 编写规范

- **类，方法使用文档注释**
- **非javadoc注释**，是对代码的说明（**给程序的维护者**）
- **tab 整体移动**
- **运算符两边给空格**
- **源码文件**使用 **utf-8** 编码
- 行宽字符**不超80**
- 代码风格：**行尾**（推荐）、**次行**



## 注释

单行注释 // 

多行注释 ==不能嵌套== /*  */

javadoc文档注释 /**    */

注释内容可以被 JDK 提供的 javadoc 所解析,生成一套以网页文件形式体现的该程序的说明文档，一般写在类

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220329140126433.png" alt="命令"  />



## DOS（了解）

相对路径：从当前所在目录开始定位，形成的路径

绝对路径：从顶级目录（盘符）开始定位，形成的路径

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/e99f2f1ca7c9417999d92e1a96cb943a.gif)

查看所有子级目录：tree



---





# 环境搭建



**环境变量作用**

> 为了在dos的任意目录，可以使用 java 和 javac 命令



**环境变量配置**

> JAVA_HOME = 指向JDK的主目录
>
> Path = %JAVA_HOME%\bin





---





# 变量

> 变量是程序的基本组成单位



## +号的使用

1. 两边都是数值型，做加法运算
2. 两边有一个为字符串，做拼接运算
3. 运算顺序：从左到右



## 数据类型 

bit：计算机中最小存储单位

byte（字节）：计算机中基本存储单元

1 byte = 8 bit

<img src="https://img-blog.csdnimg.cn/c149aa67c3f54ac1b7355fa578f5c99d.gif" style="zoom: 67%;" />

<img src="https://img-blog.csdnimg.cn/cf1ec28944bf4d578313ec896bbd0e18.gif" style="zoom:67%;" />

<img src="https://img-blog.csdnimg.cn/49ff71c5292f49e1a1608ceffb5f332d.gif"  />



## 数值型变量声明的细节

- 声明 float 后加 f 或 F ， 声明 long 后加 l 或 L

- “**保小不保大**”：整形常量默认为 int；整形常声明为 int，不足以表示大数，才用 long



## 浮点型的一些说明

1. 浮点数 = 符号位 + 指数位 + 尾数位

2. 尾数部分可能丢失，造成精度损失（小数都是近似值）

3. 浮点型常量默认为 double

4. 两种表示形式：

   - 十进制数形式：如：5.12	512.0f	.512（必须有小数点）

   - 科学计数法形式：如：5.12e2(5.12 * 10的二次方)	5.12E-2（5.12 / 10的二次方）

5. 通常情况下，应使用 double，它比 float 更精确

6. 浮点数陷阱：2.7 和 8.1 / 3 比较

   > 对运算结果是小数的进行判断应注意，小数都是近似值
   >
   > 恰当的做法为：判断两个数差值的绝对值是否在某个范围👇

```java
double num1 = 2.7;
double num2 = 8.1 / 3;
if(Math.abs(num1 - num2) < 0.00001) {
    System.out.println("相等~");
}
```



## 字符型的本质

**存储**：字符 👉 码值 👉 二进制 👉 存储

**读取**：二进制 👉 码值 👉 字符 👉 显示

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220330112151460.png)



## 布尔类型

> boolean类型数据 ==只允许取值 true 和 false==，无 null，占用1个字节

适用于**逻辑运算**



## 数据类型转换



### 自动类型转换

自动：

- char 👉 int 👉 long 👉 float 👉 double

- byte、short 👉 int 👉 long 👉 float 👉 double



**注意和细节**

1. 混合运算时，系统先自动将所有数据转换为容量最大的那种数据类型，然后再计算

   ```java
   int n1 = 10;
   float d1 = n1 + 1.1; // 错误 => 结果是 double
   //正确👇
   float d1 = n1 + 1.1F;
   double d1 = n1 + 1.1;
   ```

2. 大精度转小精度会报错

3. byte、short、char 之间不会相互自动转换

4. **byte、short、char** 三者可以计算，在 **计算时首先转换为 int**

   ```java
   byte b1 = 1;
   byte b2 = 2;
   byte b3 = b1 + b2; // 错误: b1 + b2 => int
   ```

5. boolean 不参与类型的自动转换





### 强制类型转换

强制：（data type）变量

注：可能造成精度降低或溢出

```java
int i = (int)1.9;
System.out.println(i); // 精度降低

int j = 100;
byte b = (byte)j;
System.out.println(b); // 数据溢出
```

**细节**

1. 数据：大 ——> 小
2. 强转符号只针对于最近的操作数有效，往往使用小括号提升优先级





### 基本数据类型和String类型的转换

基本  =>  String：基本类型值 + " "

```java
int n1 = 100;
String s1 = n1 + "";
```



String  =>  基本：基本类型的[包装类](##包装类)调用parseXX方法

```java
//String->对应的基本数据类型
String s1 = "123";
int num1 = Integer.parseInt(s1);
double num2 = Double.parseDouble(s1);
boolean b = Boolean.parseBoolean("true");
char c = s1.charAt(0); // c = 1
```

**细节**

- 确保格式正确，如果把"hello"，转成整数就会报错（ClassCastException）

  

---





# 运算符

> 运算符是一种特殊的符号，用以表示数据的运算、赋值和比较等



## 算术运算符

> 算术运算符是对数值类型的变量进行运算的

- 加减乘（与数学相同）、除、取模（取余）、自增自减（两种不同方法）、字符串相加

**除法（ / ）**

==与数学不同==，只取整数部分，舍弃小数部分，如：10 / 4 = 2



**取模（ % ）**

```java
// % 的本质：a % b = a - a / b * b;
System.out.println(10 % 3); // 1
System.out.println(-10 % 3); // -1
System.out.println(10 % -3); // 1
System.out.println(-10 % -3); // -1

// -10.5 % 3 => a - (int)a / b * b;
System.out.println(-10.5 % 3); // -1.5 (近似值)
```



**自增（ ++ ）**

前++：++i 先自增后赋值

后++：i++ 先赋值后自增

```java
int i = 8;
int j = ++i; // i = i + 1, j = i;
int m = 8;
int n = m++; // n = m, m = m + 1;
System.out.println(j+","+n); // 9,8
```

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220331141400707.png)



## 关系（比较）运算符

> 关系运算符的结果都是boolean型，只有true或false

- ==、!=、<、>、<=、>=、instanceof（检查是否是类的对象）



## 逻辑运算符

> 用于连接多个条件（多个关系表达式），最终结果也是boolean值

- 短路与 &&，短路或 ||，取反 ！

- 逻辑与 &，逻辑或 |，逻辑异或 ^

**短路**现象：当第一个条件符合时，第二个条件不进行判断，==效率高==



## 赋值运算符

> 将某个运算后的值，赋给指定的变量

基本：=	int a = 10；

复合：+=、-=、*=、/=、%=等

```java
// += 举例，其他同理
int a = 10;
a += 10; // a = a + 10;
```

**细节**

1. 运算顺序从右往左

2. 赋值运算符**左边只能是变量**

3. 复合赋值运算符会进行**类型转换**

   ```java
   byte b = 3;
   b = b + 2; // 报错
   b += 2; // b = (byte)(b + 2);
   b++; // b = (byte)(b + 1);
   ```



## 三元运算符

> 判断 ? 表达式1 : 表达式2 ；
>
> 若判断为真，返回表达式1的值，为假则返回表达式2的值

**细节**

1. 表达式1和表达式2要为可以赋给接收变量的类型（或可以自动转换 / 强制转换）

   ```java
   int a = 1;
   int b = 2;
   int c = a > b ? (int)1.1 : (int)2.2; // 强制转换
   double d = a > b ? a : b + 1; // int -> double
   ```

2. 三元运算符可以转成 if-else 语句

2. 三元运算符是一个整体

**练习**

```java
//三个数的最大值
int n1 = 100;
int n2 = 200;
int n3 = 300;
//思路
//1.先得到两个中的最大数，保存到 max1
//2.然后用 max1 和剩下一个数比较，保存到 max2

int max1 = n1 > n2 ? n1 : n2;
int max2 = max1 > n3 ? max1 : n3;

//后边可以用更好方法，如排序
```





## 运算符优先级

> 优先级则为运算顺序
>
> **只有单目运算符、赋值运算符是从右往左运算的**

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220401125637495.png"  />

---





# 标识符

> 给变量、方法和类等命名时使用的字符序列称为标识符
>
> 凡是可以自己起名字的地方都叫标识符



## 标识符命名规则（必须遵守）

1. 由26个英文字母大小写，0-9，_ 或 $  组成
1. 数字不能开头
1. 不可以使用关键字和保留字，但能包含
1. 严格区分大小写，长度无限制
1. 不能包含空格



## 标识符命名规范

- 包名：所有字母全小写，如：xxyy
- 类名、接口名：所有单词的首字母大写，如XxYy（大驼峰）
- 变量名、方法名：第二个单词开始每个单词的首字母大写，如：xxYyZz（小驼峰，简称驼峰法）
- 常量名：所有字母都大写，单词之间用下划线连接，如：XXX_YYY



---



# 关键字和保留字



## 关键字

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220401185942215.png"  />

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220401190044068.png"  />



## 保留字

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220401190146234.png)

---





# 键盘输入语句

> 扫描器（对象）：Scanner

## 方法

1. 引入 Scanner	**import java.util.Scanner;**或 **import java.util.*;**

2. 创建 Scanner 对象

   ```java
   Scanner sc = new Scanner(System.in); // sc 为 Scanner类的对象
   
   // 当程序执行到 next 方法时，会等待用户输入
   int a = sc.nextInt(); // 接收用户输入
   ```
   
   

---





# 进制转换

> 对于整数，有四种表示方式 
>
> - 二进制：0，1，满2进1。以 0b 或 0B 开头。
> - 十进制：0-9，满10进1。
> - 八进制：0-7，满8进1.以数字0开头表示。
> - 十六进制：0-9及A(10)-F(15)，满16进1.以 0x 或 0X 开头表示。此处A-F不区分大小写



### 其它 **->** 十

- 按权展开求和

> **二转十**
> 每位 * 2的（位数-1）次方，然后求和

> **八转十**
> 每位 * 8的（位数-1）次方，然后求和

> **十六转十**
> 每位 * 16的（位数-1）次方，然后求和



### 十 **->** 其它

- 短除法

> **十转二**
> 不断除2，直到商为0，然后将每步的余数倒过来

> **十转八**
> 不断除8，直到商为0，然后将每步的余数倒过来

>  **十转十六**
> 不断除16，直到商为0，然后将每步的余数倒过来



### 二 **->** 八、十六

- 先分组，再转换

> **二转八**
> 从低位开始，每三位一组，转成对应八进制

> **二转十六**
> 从低位开始，每四位一组，转成对应十六进制



### 八、十六 -> 二

> 二转八 / 十六的逆转



---





# 位运算



## 原码、反码、补码

对于有符号而言：

1. **最高位是符号位**（0 ->0，1-> -）
2. **正数“三码合一”**
3. **负数反码**：符号位不变，其它位按位取反（0 ->1，1->0）
4. **负数补码 = 反码 +1**
5. **0 的反码、补码都是 0**
6. java中的数都是有符号的
7. **计算机以 ==补码的方式== 来运算**
8. **运算结果看 ==原码==**



## 位运算详解

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220401205511486.png)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220401205551732.png)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220401205956785.png)





---





# 程序控制结构



## 顺序控制

> 程序从上到下逐行执行，中间没有任何判断和跳转



## 分支控制

> 让程序有选择的执行

### 单分支 if

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220402114125693.png)



### 双分支 if-else

```java
if(){
    
}
else{
    
}
```

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220402114429031.png)



### 多分支

```java
if(){
    
}
else if(){
    
}
else{
    
}
```

> 特别说明：1.多分支可以没有else，如果所有条件表达式都不成立，则没有执行入口
>
> 2.如果有else，条件都不成立时则默认执行else

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220402115255823.png)



### 嵌套分支

> 在一个分支结构中完整的嵌套了另一个分支结构。规范：不要超过3层（可读性不好）

```java
if(){
    if(){
        //if-else...
    }else{
        //if-else...
    }  
}
```



### switch分支结构

```java
switch(表达式){
    case 常量1:
        语句块1;
        break;
    case 常量n:
        语句块n;
        break;
    default：
        default语句块;
        break;
}
```

#### 细节

1. 表达式数据类型和case后的常量**类型一致**，或是**可以自动转换**成可以比较的
2. swtich（表达式）中表达式的返回值必须是：byte，short，int，char，enum（枚举），String
3. **case返回值必须是常量**或**常量表达式**
4. default可有可无
5. 如果没有break，会一直执行，直到遇到break为止



### swtich 和 if 的选择

1. 判断的具体值不多，且符合byte、short、int、char、enum、String，建议用**switch**
2. **区间**判断，**结果为boolean**类型，用 **if**



## 循环控制

### for循环

> for (循环变量初始化；循环条件；循环变量迭代) {
>
> ​	循环操作（语句）；
>
> }

#### **执行循序**

> 循环变量初始化👉循环条件👉循环操作👉循环变量迭代

#### 流程图

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220402124655351.png)

#### 细节

1. 循环条件返回boolean类型
2. 循环变量初始化和迭代可以放在 for 条件外，如：for(；循环条件；)
3. for(；；) 死循环 + break

#### 编程思想（例题）

> 1.化繁为简：将复杂的需求，拆解成简单的需求，逐步完成
>
> 2.先死后活：先考虑固定的值，再转换成可灵活变化的值

```java
//输出1-100中所有7的倍数的整数，并统计个数，求出总和
//思路分析
//化繁为简：(1)输出1-100所有整数
//		(2)7的倍数
//		(3)定义count变量统计个数
//		(4)定义sum变量求和
//先死后活：(1)输出范围改变 start end
//		(2)倍数改变 multiple
int count = 0; // 计数
int sum = 0; // 求和
int start = 1;
int end = 100;
int multiple = 7; // 倍数
for (int i = start; i <= end; i++){
    if(i % multiple == 0){
    	System.out.println(i);
        count++;
        sum += i;
    }
}
```





### while循环 （与for类似）

> while (循环条件) {
>
> ​	循环体(语句)；
>
> ​	循环变量迭代；
>
> }

#### 执行循序

循环条件👉循环体👉循环变量迭代

#### 流程图

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220402132445073.png)





### do...while循环

> do{
>
> 循环体(语句)；
>
> 循环变量迭代；
>
> }while(循环条件)；

#### 执行顺序

先执行，再判断

#### 细节

- 最后有一个；分号

- 至少执行一次





### 多重循环

> 循环嵌套，一般使用两层，太多影响可读性
>
> **实质**：内层循环当成外层循环的循环体（内层循环为false时跳出内层）
>
> 循环次数 = 内层次数 * 外层次数

#### 九九乘法表

```java
// 九九乘法表
// 1 * 1 = 1
// 1 * 2 = 2  2 * 2 = 4
// ...
// 内层 * 外层 = 结果
public class MultiplicationTables{
	public static void main(String[] args){
		for (int i = 1; i <= 9; i++){
			for (int j = 1; j <= i; j++){
				System.out.print(j + "*" + i + "=" + (i * j) + '\t');
			}
			System.out.println();
		}
	}
}
```

#### 金字塔

```java
// 打印金字塔
//     *      第一层：四个空格
//    ***     第二层：三个空格
//   *****    第三层：两个空格
//  *******   第四层：一个空格
// *********  第五层：零个空格
//
// 思路分析：
//  1.打印矩形(5 * 5)，确定层数
//  2.打印直角三角形，确定*数
//  3.打印每行空格，空格数 = 总层数 - 当前层数
// 先死后活：
//  层数做变量
import java.util.Scanner;

public class Pyramid{
	public static void main(String[] args){
		Scanner sc = new Scanner(System.in);
		System.out.print("请输入金字塔的总层数：");
		int layer = sc.nextInt(); // 总层数
		for (int i = 1; i <= layer; i++){ // i表示金字塔层数
			
			for (int k = 1; k <= layer - i; k++){ // k表示每层空格数
				System.out.print(" ");
			}

			for (int j = 1; j <= 2 * i - 1; j++){ // j表示每层的*数
				System.out.print("*");
			}
			System.out.println();
		}
	}
}
```

#### 空心金字塔

```java
// 打印空心金字塔
//     *     第一层：一个* 开头三个空格
//    * *    第二层：两个* 开头两个空格
//   *   *   第三层：两个* 开头一个空格
//  *******  第四层：七个* 开头零个空格
// 思路分析:
//  1.层数
//  2.每层开头空格数 = 总层数 - 当前层数
//  3.控制每层打印*数：除最后一层外，当前层第一位和最后一位打印*，其他打印空格
// 先死后活：
//  层数做变量
import java.util.Scanner;

public class HollowPyramid{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
         int layer = sc.nextInt();
         for (int i = 1; i <= layer; i++){ // i表示层数
			// 每层开头空格数
			for (int k = 1; k <= layer - i; k++){
				System.out.print(" ");
			}
			// 控制每层打印*数
			for (int j = 1; j <= 2 * i - 1; j++){
				
				// 只有第一位和最后一位打印*,最后一层全部打印*
				if (j == 1 || j == 2 * i - 1 || i == layer){
					System.out.print("*");
				}else {
					System.out.print(" ");
				}
			}
			// 每层打印完后换行	
			System.out.println();	
		}
	}
}
```

#### 空心菱形

```java
// 打印空心菱形
//   *    第一层：1个* 2个空格
//  * *   第二层：3个* 1个空格
// *   *  第三层：5个* 0个空格
//  * *   第四层：3个* 1个空格
//   *    第五层：1个* 2个空格
public class HollowDiamond{
	public static void main(String[] args) {
		// 上半部分
		for (int i = 1; i <= 3; i++) {
			// 空格
			for (int j = 1; j <= 3 - i; j++)
				System.out.print(" ");
			// *
			for (int k = 1; k <= 2 * i - 1; k++) {
				if(k == 1 || k == 2 * i - 1) {
					System.out.print("*");
				}else{
					System.out.print(" ");
				}
			}
			System.out.println();
		}
		// 下半部分
		for (int i = 2; i >= 1; i--) {
			// 空格
			for (int j = 1; j <= 3 - i; j++) {
				System.out.print(" ");
			}
			// *
			for (int k = 1; k <= 2 * i - 1; k++) {
				if(k == 1 || k == 2 * i - 1) {
					System.out.print("*");
				}else{
					System.out.print(" ");
				}
			}
			System.out.println();
		}
	}
}
```



## 跳转控制语句

> break、continue

### break

> 跳出当前层循环 或 结束case语句

```java
// 用 for + break 实现登录验证
// 有三次机会，如果用户名为“丁真”，密码为“666”提示登录成功，否则提示用户名或密码错误，还有几次机会
import java.util.Scanner;

public class Break {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		for (int i = 1; i <= 3; i++) {
			
			System.out.print("用户名：");
			String username = sc.next();
			
			System.out.print("密码：");
			String password = sc.next();
			
			if(username.equals("丁真") && password.equals("666")) {
				System.out.println("登录成功");
				break;
			}else {
				System.out.println("用户名或密码错误，还有" + (3 - i) + "次机会");
			}
		}
	}
}
```



### continue

> 跳出该次循环



### return

> return使用在方法，表示跳出所在的方法
>
> 如果用在主方法，退出程序



---





# 数组

> 数组：一组数据
>
> 存放多个同一类型的数据，是引用类型



## 一维数组的使用

### 步骤

声明and开辟空间👉元素赋值👉使用

### 1.动态初始化（声明并创建）

> 数据类型 数组名 [ ] = new 数据类型[ 大小 ] 
>
> int[ ] a = new int [2]; // 创建一个数组，存放2个int

### 1.5动态初始化（先声明再创建）

> int[ ]  a;
>a = new  int [2];

### 2.静态初始化

> int[]  a = { 0,1,2 };



## 细节

- 数值型默认值 0 或 0.0，char默认 \u0000，boolean默认 false，String默认 null
- 数组下标(索引)从0开始
- 数组型数据是对象（object）



## 数组的赋值机制

> 引用赋值：数组在默认情况下是以引用传递，赋的值是地址

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220406225408269.png)



## 数组常见算法

### 拷贝

```java
int[] arr1 = {1, 2, 3};
int[] arr2 = new int[arr1.length];
// 遍历arr1，对应元素拷贝到arr2
for (int i = 1; i < arr1.length; i++) {
    arr2 [i] = arr1[i];
}
```



### 翻转

```java
int[] arr = {1, 2, 3, 4, 5, 6};
// a[0] 和 a[5] 交换
// a[1] 和 a[4] 交换
// a[2] 和 a[3] 交换
// 交换了3次，arr.length / 2 次，a[i] 和 a[arr.length - 1 - i]交换
int temp = 0; // 临时变量用于交换
int len = arr.length;
for (int i = 1; i <= len / 2; i++) {
    temp = arr[len - 1 - i];
    arr[len - 1 - i] = arr[i];
    arr[i] = temp;
}
```



### 扩容

```java
int[] arr = {1, 2, 3};
// 创建新数组
int[] arrNew = new int[arr.length + 1];
// 遍历 arr数组，将元素拷贝到 arrNew
for (int i = 1; i < arr.length; i++) {
    arrNew[i] = arr[i];
}
// 将新元素添加到 arrNew
arrNew[arrNew.length - 1] = 4;
// arr 指向 arrNew
arr = arrNew;
```



### 缩减

```java
// 数组缩减
// 当缩减到最后一个元素时，提示结束
import java.util.Scanner;

public class ArrayReduce {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		// 数组初始化
			int[] arr = {1, 2, 3, 4};
		do {
			// 创建新数组，拷贝缩减一次后的 arr数组
			int[] arrNew = new int[arr.length - 1];
			for (int i = 0; i < arr.length - 1; i++) {
				arrNew[i] = arr[i];
			}
			// 输出缩减一次后的数组
			System.out.println("===缩减后的数组===");
			arr = arrNew;
			for (int i = 0; i < arr.length; i++) {
				System.out.print(arr[i] + " ");
			}
             // 询问用户是否继续缩减
			System.out.println("还需要继续缩减吗？ y/n");	
			char key = sc.next().charAt(0);
			if(key == 'n') {
				break;
			}
			if(arr.length == 1) {
				System.out.println("元素不足，缩减结束...");
				break;
			}
		}while(true);
	}
}
```



### 冒泡排序

> 冒泡排序（Bubble Sorting）：像气泡一样一个一个冒出来
>
> 原理：将最大的数排在最后，次大的放在倒数第二位，以此类推
>
> 如果前边的数大于后边的数，就交换
>
> **缺点**：特殊情况下，一次就排好顺序，也会继续循环

```java
// 冒泡排序
public class BubbleSort {
	public static void main(String[] args) {
        int temp = 0;
		int[] arr = {2, 4, 1, 5, 3};
		for (int i = 1; i < arr.length; i++) {
			for (int j = 0; j < arr.length - i - 1; j++) {
				// 相邻元素比较
				if(arr[j] > arr[j + 1]) {
					temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}
		}
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
	}
}
```



### 查找

```java
// 顺序查找
// 查找名字并输出下标
import java.util.Scanner;

public class SeqSearch {
	public static void main(String[] args) {
		// 定义一个字符串数组
		String[] names = {"老二","老六","老八"};
		Scanner sc = new Scanner(System.in);
		
		// 输入名字
		System.out.println("请输入名字");
		String findName = sc.next();
		
		// 遍历数组，逐一比较
		int index = -1;
		for (int i = 0; i < names.length; i++) {
			// equals 字符串比较
			if(findName.equals(names[i])) {
				System.out.println("恭喜你找到" + findName);
				System.out.println("下标 = " + i);
				index = i;
				break;
			}
		}
		
		if(index == -1) {
			System.out.println("哦~没找到");
		}
	}
}
```



## 二维数组

> 一维数组为元素的一维数组，即二维数组
>
> int[ ] [ ] arr = new int [一维长度] [二维长度];（二维长度 = 一维每个元素的长度）

### 原理

![image-20220408135257461](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220408135257461.png)



### 关于使用的例题

> 打印如下：
> 1
> 2 2 
> 3 3 3

```java
public class TwoDimensionalArray {
	public static void main(String[] args) {
		int[][] arr = new int[3][]; // 创建二维数组，确定一维个数
		for (int i = 0; i < arr.length; i++) { // 遍历arr每一维数组
			// 给每个数组开辟空间，否则均为null
			arr[i] = new int[i + 1];
			// 遍历一维数组，并给每个元素赋值
			for (int j = 0; j < arr[i].length; j++) {
			arr[i][j] = i + 1;
			}
		}
		// 遍历arr输出
		for (int i = 0; i < arr.length; i++) {
			for (int j = 0; j < arr[i].length; j++) {
				System.out.print(arr[i][j] + " ");
			}
			System.out.println();
		}
	}
}
```



### 杨辉三角

```java
// 打印十行杨辉三角
// 1
// 1	1
// 1	2	1
// 1	3	3	1
// 1	4	6	4	1
// n行有n个数，首尾都是1，除首尾外每个数 = 上一行同一列数 + 上一行上一列数
public class YangHui {
	public static void main(String[] args) {
		// 行数确定，列数等于行数
		int[][] arr = new int[10][];
		for (int i = 0; i < arr.length; i++) { // 遍历二维数组第一维
			// 给每个一维数组（行）开辟空间
			arr[i] = new int[i];
			// 给每个一维数组（行）赋值
			for (int j = 0; j < arr[i].length; j++) {
				if(j == 0 || j == i - 1) { // 每行首尾都是1
					arr[i][j] = 1;
				}else {
					arr[i][j] = arr[i-1][j] + arr[i-1][j-1];
				}
			}
		}
		// 输出杨辉三角
		for (int i = 0; i < arr.length; i++) {
			for(int j = 0; j < arr[i].length; j++){
				System.out.print(arr[i][j] + "\t");
			}
			System.out.println();
		}
	}
}
```



### 特殊声明

> int [ ]x,y[ ] x是int类型一维数组，y是int类型二维数组



---





# 面向对象

> Java最大的特点



## 类和对象

> 类：自定义的数据类型
> int：Java提供数据的类型
>
> 对象：具体的实例（类的一个成员）
>
> 类是对象的模板，对象是类的一个个体



### 定义类

> 类是抽象的，概念的，一大类，例如人类，狗类

```java
// 定义一个角色类
public class Characters {
    // 角色的属性
    String name; // 名称
    String gender; // 性别
    String career; // 职业
}
```



### 创建对象

> 用关键字new创建，对象是具体的，实际的，比如某个人，某条狗

```java
// 创建对象
public class CreateObject {
    // 实例化一个对象（创建一个对象）
    Characters c1 = new Characters();
    
    // 对象的属性
    c1.name = "ShameYang";
    c1.gender = "Man";
    c1.career = "Stu";
}
```



### 匿名对象

> 直接new
>
> new Person();
>
> 匿名对象只能使用一次，之后等待销毁



### 对象在内存中的存在形式

![image-20220410200557594](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220410200557594.png)



### 克隆对象

> 新对象和原来对象**相互独立**，只是属性相同

```java
class CloneObject {
    public Person copyPerson(Person p){
        Person p2 = new Person();
        p2.name = p.name; // 把原来对象的名字赋给p2
        return p2;
    }
}
```





### 内存分配机制

**Java内存结构**

1. 栈：一般存放基本数据类型（局部变量）
2. 堆：存放对象（具体对象，数组等）
3. 方法区：常量池（字符串等常量），类加载信息



**创建对象的简单流程**

1. 先**加载类**信息（属性和方法信息，只会加载一次）
2. **堆中分配空间**，进行默认初始化
3. 把地址赋给对象名，对象名就会**指向对象**
4. 进行**指定初始化**（对象.属性）

![image-20220411120637155](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220411120637155.png)



### 对象创建流程详解（含构造器）

![image-20220415091456534](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220415091456534.png)

1. 加载Person类信息(Person.class)，只加载一次

2. 在堆中分配空间(地址)

3. 完成对象初始化

   3.1 默认初始化 age=0, name=null

   3.2 显式初始化 age=90, name=null

   3.3 构造器初始化 name="小倩", age=20

4. 对象在堆中的地址，返回给p



---



## 作用域

- java中主要的变量是**属性(成员变量)**和**局部变量**

- 局部变量一般指在成员方法中定义的变量

- 作用域分类

> **全局变量**：属性，作用域为整个类体

> **局部变量**：除属性之外的其他变量，作用域为所在代码块

- **全局变量(属性) 有默认值**，可以不赋值直接使用

- **局部变量没有默认值**，必须赋值后才能使用



**注意事项和使用细节**

1. 属性和局部变量可以重名，访问时遵循就近原则

2. 全局变量 / 属性可以加修饰符

   

---



## 类变量和类方法

#### 类变量

**类变量** 又称 **静态变量** (**static**)、**静态属性**

- 类变量 static 是同一个类所有对象共享的，在类加载的时候就生成了

**访问**

**类名.类变量名** (推荐)

对象.类变量名也可，访问修饰符的权限和范围，与普通属性一样

**注意事项**

1. 什么时候用类变量

   需要让某类中所有对象都共享一个变量时

2. 类变量 与 实例变量(普通属性) 的区别

   类变量所有对象共享，实例变量每个对象独享(彼此之间无影响)

3. 不加 ~~static~~ 的变量，称为 实例变量 / 普通变量 / 非静态变量

4. **实例变量不能用类名调用**



#### 类方法

类方法 又称 静态方法

访问 与类变量同理

**类方法的使用**

不需要创建对象时，直接用类调用静态方法，提高开发效率
例如：Math.random();

**当成工具来使用**

**注意事项**

1. 类方法中不能用与对象有关的关键字，如: this, super

2. 实例方法不能用类名调用

3. 静态方法只能调用静态成员，若调用非静态成员，需创建对象

---





## 类的成员

### 属性 / 成员变量

> 成员变量 = 属性 = field(字段)

```java
public class Characters {
    // 角色的属性，成员变量，字段field
    String name; // 名称
    String gender; // 性别
    String career; // 职业
}
```

- 属性有默认值，规则同数组中的默认值

  

---



### 方法 / 成员方法

> 成员方法 = 方法 = 行为 = method

```java
public class Method {
    // public：表示方法公开
    // void：表示方法没有返回值
    // fight()：fight是方法名，()是形参列表
    // {} 方法体，写我们要执行的代码
    // System.....表示输出一句话
    public void fight() {
        System.out.println("战斗");
    }
}
```



#### 定义

```java
访问修饰符 返回数据类型 方法名 （形参列表...）{
    //方法体语句
    return 返回值;
}
```

1. public就是一种访问修饰符，修饰符部分详细说明
2. 形参列表：表示成员方法输入
3. 数据类型（返回类型）：表示成员方法输出，void表示没有返回值
4. 方法体：表示实现某一功能代码块
5. return语句不是必须的



#### 调用机制

![image-20220411130119407](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220411130119407.png)





#### 方法定义细节

**访问修饰符**

- 作用是**控制**方法**使用**的**范围**
- 不写表示默认访问
- 访问修饰符：public，protected，默认，private
- 修饰符部分详细说明

**返回类型**

1. 返回类型可以为任意类型，基本类型或引用类型（数组，对象）
2. 方法有返回类型，必须有 return语句，类型必须 **一致 **或 **兼容**（即可以自动类型转换）
3. void类型，方法体可以没有 return 或 只写 return

**方法名**

- 驼峰命名法（小驼峰），见名知意

**参数列表**

1. 可以有 0个 或 多个，中间用逗号隔开，如 **getSum(int n1,int n2)**
2. 参数类型可以为任意类型，基本类型 或 引用类型，如 **printArr(int[ ] [ ] map)**
3. **调用带参方法 **时，传入类型必须 **一致** 或 **兼容**
4. **定义时** 的参数称为 **形式参数**，简称 **形参**；**调用时** 的参数称为 **实际参数**，简称 **实参**，==实参的个数、顺序必须一致==

**方法体**

- **方法不能嵌套定义**



#### 方法调用细节

1. 同一类中调用：直接调用

2. 跨类调用：通过对象名调用 -> **对象名.方法**

3. 跨类调用与访问修饰符相关，后边详细说明



#### 传参机制

- **基本数据类型**，传递的是值（值拷贝，**形参的改变不影响实参**）

- **引用数据类型**，传递的是地址（**值也是地址**，**形参影响实参**）

  ![image-20220412215741601](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220412215741601.png)



#### 方法重载

> java中允许同一类中，多个同名方法存在，但要求==形参列表不一致==
>
> 如System.out.println();就是方法重载，out是PrintStream类型

- 重载的好处
  1. 减轻了起名的麻烦
  2. 减轻了记名的麻烦

```java
// 演示方法重载
class Overload {
	public int add(int n1, int n2) {
        return n1 + n2;
    }
    public int add(int n1, int n2, int n3) {
        return n1 + n2 + n3;
    }
    public double add(int n1, double n2) {
        return n1 + n2;
    }
}
```

注意事项和使用细节

1. 方法名：必须相同

2. 参数列表：必须不同（参数的类型、个数、顺序，**至少一样不同**，参数名无要求）

3. 返回类型：无要求



#### 可变参数

> 可变参数：java允许同一个类中，**多个同名同功能**但**参数不同**的方法，封装成一个方法

**使用**

```java
// 修饰符 返回类型 方法名(参数类型...参数名) {方法体}
// 多个参数求和，类比数组
public int sum(int... nums) {
    int res = 0;
    for(int i = 0; i < nums.length; i++) {
        res += nums[i];
    }
    return res;
}
// 主方法中,使用sum()方法即可实现多个参数求和
```

**注意事项和使用细节**

1. 可变参数的个数可以是0个或多个

2. **实参可以为数组**

3. 可变参数**本质是数组**

4. 可变参数可以和普通类型参数一起放在形参列表，但**可变参数必须在最后**

5. **一个形参列表中只能出现一个可变参数**

---





### 构造器 / 构造方法

> 构造方法，又叫构造器(constructor)，是类的一种特殊的方法，主要作用是完成**新对象的初始化**

> 修饰符 方法名(形参列表) {
> 		方法体;
> }

- 修饰符可以默认
- 形参列表 和 成员方法规则相同
- **构造器的调用，由系统完成**：创建对象时，系统自动调用该类构造器并完成**对象的初始化**

```java
public class Constructor {
    public static void main(String[] args){
        //new一个对象时，直接通过构造器指定名字和性别
        Charcaters c1 = new Charcaters("ShameYang","Man");
    }
}
//创建角色类的对象时，直接指定这个对象的名字和性别
class Characters {
    String name;
    String gender;
    //构造器
    public Person(String cname, String cgender) {
        name = cname;
        gender = cgender;
    }
}
```



**注意事项和使用细节**

1. 构造器可以重载
   ```java
   class Characters {
       String name;
       String gender;
    //第一个构造器
       public Person(String cname, String cgender) {
       name = cname;
       gender = cgender;
    }
    //第二个构造器，只指定姓名
       public Person(String cname) {
       name = cname;
       }
   }

2. 构造器名和类名**必须一致**

3. 构造器**没有返回值**

4. 构造器完成对象的**初始化**

5. 创建对象时，系统自动调用该类的构造方法

6. 如果我们没有定义构造器，系统会自动给类生成一个默认无参构造器（默认构造方法），**可以使用javap指令反编译查看**

6. **一旦定义了自己的构造器，默认的构造器就覆盖了**，不能再使用默认的无参构造器，除非显式定义一下，如：Characters( ) { }



#### this关键字

> 根据变量的作用域原则，**当构造器的参数名与属性名相同时**，需要使用**this关键字进行区分**

- 什么是this

> JVM会给每个对象分配 this，代表当前对象

```java
class Characters {
    String name;
    String gender;
    public Characters(String name, String gender) { //构造器
        // this.name 就是当前对象的属性 name
        this.name = name;
        // this.gender 就是当前对象的属性 gender
        this.gender = gender;
    }
}
```



- this的地址与对象的地址相同

![image-20220415111041822](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220415111041822.png)

**哪个对象调用，this就代表哪个对象**



**注意事项和使用细节**

1. this关键字可以用来访问本类属性、方法、构造器

2. this用于区分当前类的属性和局部变量

3. 访问成员方法：this.方法名(参数列表);

4. 访问构造器：this(参数列表); **只能在构造器中使用**

   **访问构造器的语句必须放在第一句**（继承中详细说明）

   ```java
   class This {
       String name;
       
       // 访问构造器 this(参数列表);
       public This() {
           this("ShameYang"); // 访问构造器
       }
       public This(String name) {
           this.name = name;
       }
       
       // 访问成员方法 this.方法名(参数列表);
       public void f1() {
           System.out.println("f1");
       }
       public void f2() {
           System.out.println("f2");
           f1();
           this.f1(); // 访问成员方法f1()
       }
   }
   ```

5.**this只能在类定义的方法中使用**







### 代码块

> 又称初始化块，属于类中的成员
>
> 加载类或创建对象时，隐式调用

**基本语法**

> [static 或 不写] {
> 	代码
> }

**好处**

1. 相当于构造器的补充，可以进行初始化
2. 提高代码复用性

**注意事项**

1. 代码块用于类的初始化，随着类的加载而执行

2. 执行条件及次数

   > 静态代码块，类加载时，只会执行一次；
   > 普通代码块，只要创建对象，就会执行

3. 类什么时候被加载

   > 创建对象实例时(new)
   >
   > 创建子类对象实例，父类也会被加载
   >
   > 使用类的静态成员时

4. 创建对象时，一个类中的调用顺序

   > (1) 先调用静态代码块和静态属性初始化（优先级相同，看定义顺序）
   >
   > (2) 然后普通代码和普通属性
   >
   > (3) 最后构造方法

5. 构造器与代码块

   ```java
   class Father {
       public Father() {
           //隐藏了
           //1.super();
           //2.调用本类普通代码块
           System.out.println("父类构造器被调用...");
       }
   }
   class Son extends Father {
       public Son() {
           //隐藏了
           //1.super();
           //2.调用本类普通代码块
           System.out.println("子类构造器被调用...");
       }
   }
   ```

6. 静态代码块只能调用静态成员
   普通代码块可以调用任意成员






### 内部类

> 类的五大成员之一
>
> 类的五大成员：属性、方法、构造器、代码块、内部类

内部类分为：

> **局部** 内部类、**匿名** 内部类 👈 定义在局部位置
>
> **成员 **内部类、**静态 **内部类 👈 定义在成员位置



#### 局部内部类

> 局部内部类，定义在外部类的局部位置，例如方法中或代码块中，并且有类名

```java
class Outer {
    private int n;
    //方法中局部内部类
    public void example() {
        class Inner {  ...  }
    }
    //代码块中局部内部类
    {
        class Inner01 { ... }
    }
}
```

**注意事项**

1. 可以**直接**访问外部类的所有成员

2. 不能添加访问修饰符，可以使用 final 修饰

   > 局部内部类的地位 👉 局部变量

3. 作用域：所在的 方法体 或 代码块 中（同局部变量）

4. 成员重名

   > 遵循就近原则
   >
   > 内部类中访问外部类成员 👉 外部类.this.成员







#### 匿名内部类

> 我们可以将匿名内部类看作一个对象

**基本语法**

> new 类或接口(参数列表) { 
> 类体 
> }**;**  
>
> **注意：结尾分号不能丢**

- 基于接口的匿名内部类

  ```java
  Interface interf = new Interface() {    @Override...    };
  /* 底层 
  class Outer$1 implements Interface {  
  	@Override...  
  } */
  ```

- 基于类的匿名内部类

  ```java
  Father father = new Father() {   ...   };
  /* 底层
  class Outer$1 extends Father {
  	@Override...
  } */
  ```
  
  注意：参数列表会直接传给构造器，不用再写构造器

**调用**

```java
//第一种调用方式
Father father = new Father() {    @Override...    };
father.方法
//第二种匿名调用
new Father() {    @Override...    }.方法();
```







#### 成员内部类

> 成员内部类定义在外部类的成员位置

1. 可以访问外部类的所有成员

2. 可加访问修饰符

   > 成员内部类即外部类的一个成员

3. 外部类访问内部类

   > 先创建对象，再访问

4. 其它外部类访问

   > 匿名对象：new Outer().new Inner();

   👇第一种方式

   ```java
   Outer outer = new Outer(); //创建对象，用于外部类访问内部类
   Outer.Inner inner = outer.new Inner();
   
   class Outer {
       class Inner{}
   }
   ```

   👇第二种方式：在外部类中定义一个返回内部类对象实例的方法

   ```java
   Outer outer = new Outer();
   Outer.Inner inner = outer.getInner();
   
   class Outer{
       class Inner{}
       public Inner getInner {
           return new Inner();
       }
   }
   ```

5. 访问重名成员时，就近原则

   > 内部访问外部类成员 👉 外部类.this.成员







#### 静态内部类

> 放在外部类成员位置，且有static修饰

1. 无法直接访问外部非静态成员

   > 访问外部非静态成员方法：创建对象

2. 可加访问修饰符

3. 其他外部类访问

   > 匿名对象：new Outer.Inner();

4. 成员重名时，内部类访问外部类成员

   > 外部类.成员








---







## 包(package)

> 包可以区分相同名字的类，利于管理类，而且可以控制访问范围

- 包的本质：创建不同的文件夹，保存类文件



### 命名规则

- 只能包含数字、字母、下划线、小圆点，数字不能开头，不能是关键字或保留字

### 命名规范

- 一般是小写字母+小圆点

  一般是**com.公司名.项目名.业务模块名**



### 常用包

- java.lang.* —— lang包是基本包，默认引入，不需要再引入

- java.util.* —— util 包，系统提供的工具包，工具类 

- java.net.* —— 网络包，网络开发

- java.awt.* —— 做java的界面开发，GUI



### 引入包

> 引入包是为了使用其中的类

- 语法：import 包;

- 例：import java.util.*; —— 表示将java.util 包所有类都引入



### 包的小细节

1. **package 的作用**是声明当前类所在的包，位于类的代码块的最上边，**一个类最多只有一个package语句**
2. **import指令** 放在package语句后边，放在类定义的前边，可以有多句，无顺序要求





## 访问修饰符



![image-20220418212719208](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220418212719208.png)

**注意**：只有默认和public才能修饰类！





## 封装

> 封装(encapsulation)就是把抽象出的数据(属性)和对数据的操作(方法)封装在一起，数据被保护在内部，程序的其它部分只有通过被授权的操作(方法)，才能对数据进行操作

**封装的好处**

1. **隐藏实现细节**：某个方法，使用者可直接调用
2. 可以**对数据进行验证**，保证安全合理

**步骤**

**属性private私有化** 👉 提供public **set()方法**，用于对属性判断并赋值 👉 提供public **get()方法**，用于获取属性的值

- **构造器 + set()方法**，仍然可以验证

  ```java
  public Person{
      private String name;
      public Person() {
          
      }
      public Person(String name) {
          setName(name);
      }
      public void setName(String name) {
          this.name = name;
      }
  }
  ```





## 继承

> 当不同类中有许多相同的属性和方法时，此时利用继承，解决代码复用的问题

### 子父类

- 父类：定义相同的属性和方法

- 子类：通过**extends**声明继承父类

### 继承基本语法

```java
/* 1.子类自动拥有父类定义的属性和方法
 * 2.父类也叫 超类，基类
 * 3.子类也叫 派生类
 */
class 子类 extends 父类 {
}
```



### 原理图

![image-20220419192707168](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220419192707168.png)



###  细节

1. **私有属性不能在子类直接访问**，要通过公共方法访问

   > 在父类中定义一个get()方法

2. 子类调用构造器时，父类的构造器也会被调用

3. 创建子类对象时，**默认**情况下总会**调用父类的无参构造器**，若父类没有提供无参构造器，则必须在子类的构造器中用 **super** 指定使用父类的构造器，完成对父类的初始化

4. 指定调用父类某一构造器，显式调用 - **super(参数列表)**

5. super使用时，必须放在构造器第一行（**super只能在构造器中使用**）

6. **调用构造器**：**super()** 和 **this()** 不能共存

7. java所有类都是 **Object类** 的子类（Object是祖宗）

   > IDEA中 ctrl + H 查看类的继承关系

8. **单继承机制**

   > 子类最多只能继承一个父类

9. **子父类 a-kind-of** 逻辑关系

   > 子类 is a kind of 父类



### 子父类调用

1. 先找本类，如果有，则调用
2. 本类没有，则找父类（如果父类有，则调用）
3. 父类没有，继续向上级父类寻找...以此类推，直到Objext类

**注意：如果找到了，但不能访问，则会报错；如果没有找到，则提示方法不存在**

![image-20220420215906574](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220420215906574.png)

（这里可以加强对Java内存结构的理解）



### super关键字

> super代表父类的引用，用于访问父类的属性、方法、构造器

**基本语法**

1. 访问父类的属性：super.属性名；
2. 访问父类方法：super.方法名(参数列表)；
3. 访问父类构造器：super(参数列表)；

**super的便利and细节**

1. 调用父类构造器的优点：**分工明确**（父类属性有父类初始化，子类属性由子类初始化）

2. 子父类成员**重名时**，必须通过**super访问父类**的成员

3. 多个类同名时，super遵循**就近原则**

**super与this的区别**

![image-20220421221842732](C:\Users\16271\AppData\Roaming\Typora\typora-user-images\image-20220421221842732.png)





### 方法重写 / 覆盖 (override)

> 子类的某个方法，和父类的某方法的**名称**、**返回类型**、**参数**一样，则称子类的这个方法覆盖了父类的方法 (方法重写)

**注意和细节**

1. 参数、方法名要完全一致

2. 返回类型一样，或子类返回类型 是 父类返回类型的子类，如下：

   > 父类 public Object get() { }
   >
   > 子类 public String get()  { }

3. 子类方法不能缩小父类方法的访问权限（但是可以扩大）

   > public > protected > 默认 > private

**方法重写和重载的比较**

![image-20220422151748730](C:\Users\16271\AppData\Roaming\Typora\typora-user-images\image-20220422151748730.png)





## 多态

### 多态的体现

1. 方法的多态

   > 重写和重载体现多态

2. 对象的多态

   > 记住一句话：父类引用指向子类对象👇

### 转型

> 向上转型变父类，方法调用遵循子父类方法调用 (动态绑定)
>
> 向下转型，子类对象指向父类引用，为了调用子类的成员

```java
Father f1 = new Son(); // 这就是向上转型（upcasting），父类引用指向子类对象
Son s1 = (Son)f1; // 这就是向下转型（downcasting），子类引用指向父类引用（子类对象）
```

**转型注意事项**

1. 向上转型后，子类特有成员就无法调用了（儿子变成了爸爸）

2. 向下转型注意，子类引用指向子类对象（强转父类引用），否则不安全，运行报错

   ```
   Exception in thread "main" java.lang.ClassCastException
   ```



### 动态绑定

方法有动态绑定，属性没有

调用对象方法时，该方法会和对象的运行类型（内存地址）绑定

> 编译类型：等号左边
> 运行类型：等号右边



### instanceof 操作符

> 用于判断对象是否为某类的实例或子类的实例，返回类型为boolean

```java
class instanceOf {
    public static void main(String[] args) {
        Son s1 = new Son();
        System.out.println(s1 instanceof Son); // true
        System.out.println(s1 instanceof Father); // true
    }
}
class Father {}
class Son extends Father {}
```



### 多态数组

> 与数组声明方式相同，Father[ ]  fathers = new Father[ length ];

### 多态参数

> 形参为父类，实参为子类





---



## main方法语法

> public static void main(String[] args) { }
>
> main方法由java虚拟机调用
>
> 调用仍遵循静态方法
>
> 注意传参，命令行以及IDEA



---





## final关键字

- 基本使用

> 1.某类不能被继承
>
> 2.父类某方法不能被子类重写
> （访问修饰符 final 返回类型 方法名）
>
> 3.某属性或局部变量不能被修改

- 注意事项

  > 1.final 修饰的属性又叫常量，一般 XX_YY_ZZ 命名
  >
  > 2.final 修饰的普通属性，必须在 初始化时（定义 / 构造器 / 代码块） 赋值
  >    fianl 修饰的静态属性只能在 定义时 或 静态代码块中 赋值 ~~不能在构造器中赋值~~
  >
  > 3.非final 类的 final 方法，不能重写，但可以继承
  >
  > 4.final + static ，不会导致类的加载，比如：只想获取静态属性，而不调用类的其它部分
  >
  > 5.包装类和 String 都是 final 类，不能被继承
  >
  > 6.final 类的方法不用再加 final 修饰

---



# Object 类

## equals方法

**== 与 quals 的比较**

> ==：比较运算符，用于判断**基本数据类型的值**或**引用类型的地址**是否相同，即是否为同一个对象
>
> equals：Object类中的方法，用于判断两个对象的内存地址是否相同

**注意事项**

1. 重写equals方法：用于比较内容是否相同
   不重写：比较引用（地址）是否相同

```java
// 比较两个Person对象
class Person {
    private String name;
    // 重写equals方法
    public boolean equals(Object obj) {
    	if(this == obj) { retuen true; } // 比较的对象是同一个对象，直接返回true（this为调用方法的对象）
        // 类型判断
    	if(obj instanceof Person) { // 对象是Person类，才进行比较
            // 向下转型，为了获取obj属性
            Person p = (Person)obj;
            return this.name.equals(p.name);
        }
        return false;
	}
}
```



## hashCode方法

**基本介绍**

> 1. 提高具有哈希结构的容器的效率
>
> 2. 两个引用，如果指向同一个对象，则哈希值相同
>    两个引用，指向不同对象，哈希值不同
>
> 4. 哈希值根据地址号而来，但不完全等价于地址
>
> 5. 后边集合部分中，hashCode如果需要，也需重写



## toString方法

**基本介绍**

>  默认返回：全类名 (包名+类名) + @ + 哈希值的十六进制

- 子类重写 toString 方法，用于返回对象的属性信息

- 重写 toString 方法，打印或拼接对象时，都会自动调用该对象的 toString 形式
  对象名 { 属性1，属性2... }

- 重写后，直接输出一个对象，默认调用 toString 方法，否则输出 hashCode 值



## finalize方法

**基本介绍**

> 对象没有引用后，jvm会使用垃圾回收机制，调用该对象的 finalize 方法，来销毁该对象
>
> 默认处理：调用 Object 类的 finalize
>
> 可以重写 finalize方法，用来实现自己的逻辑





---





# Debug

> 断点调试 (Debug)：用来分析程序执行过程，找bug，有助于我们理解代码
>
> 调试过程中，可以看到变量的当前值

👇**快捷键**

> F7 (Step Into)：跳入方法内
>
> F8 (Step Over)：逐行执行代码
>
> shift + F8 (Step Out)：跳出方法
>
> F9 (Resume Program)：执行下一个断点





---



# 房屋出租系统

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/houserent.png)





---

# 抽象类

> abstract修饰的类
>
> 1. 抽象类主要用于设计，让子类继承实现抽象类
>
> 2. 较多用于框架和设计模式

```java
abstract Person {
    String name;
    int age;
    public abstract void eat();
}
```



## 细节

1. abstract 只能修饰类和方法

2. 抽象类可以没有抽象方法，但是抽象方法必须在抽象类里

3. 抽象类不能被实例化

   > 不能 new 抽象类

4. 抽象方法没有方法体，没有主体(即大括号)

   > public abstract void method();

5. 抽象方法不能用 private、final、static 修饰

   > 否则方法不能被重写
   
5. 继承抽象类，必须实现所有的抽象方法，否则子类也为抽象类

   > 实现抽象方法 👉 重写抽象方法，加上主体(大括号)
   >





---





# 接口

## 基本介绍

> 接口给出一些没有实现的方法，封装到一起，某类使用时，根据需要重写方法

```java
interface 接口名 {
    //属性
    //方法(1.抽象 2.静态 3.默认),接口中，abstract可省略
}
class 类名 implements 接口 {
    自己的属性
	自己的方法
    必须实现接口的所有抽象方法
}
```

> JDK 7.0之前，接口里的方法都是抽象方法，没有方法体
>
> JDK 8.0后，接口可以有 static / default 方法



## 注意事项

1. 接口不能被实例化

   > '...' is abstract; cannot be instantiated 	不难看出，接口与抽象类相似

2. 接口中所有方法都是公开的 (public)，接口中的抽象方法可以省略abstract

   > void method();   实际上👉  **public abstract** void method();

3. 普通类实现接口

   > 必须实现接口中所有方法，而且必须是 public (不能改变访问范围)
   >
   > IDEA快捷键 ctrl + i

4. 抽象类实现接口

   > 可以不用实现接口方法

5. 一个类可以同时实现多个接口，接口间用逗号隔开

   ```java
   interface A { void hi(); }
   interface B { void hello(); }
   class X implements A,B { public void hi ();  public void hello(); }
   ```

6. 接口属性必须是 final 的，且必须初始化！必须初始化！必须初始化！

   > int i = 0;  实际上👉  public static final int i = 0; 

7. 一个接口可以继承多个接口，但**不能继承类**

   ```java
   interface A extends B,C { }
   ```

8. 接口的访问修饰符只能 public 或 默认，与类相同





## 接口VS继承

> 由于单继承机制，子类只能继承一个父类，拥有一个父类的功能
>
> 因为一个子类可以有多接口，所以子类通过实现接口的方式，可以扩展功能
>
> 实现接口，是对单继承机制的补充

- 解决的问题不同

  > 继承的价值：解决代码复用性和可维护性
  >
  > 接口的价值：设计更规范的代码(方法)，让其它类实现方法

- 接口比继承更灵活

  > 单继承机制，只能继承一个类中所有可继承成员
  >
  > 接口可以有多个，范围更广，而且只实现方法(重写接口方法)

接口一定程度上实现代码解耦（接口规范性 + 动态绑定），学完集合再试着理解





## 接口的多态

1. 多态参数

   > 方法的形参为接口类型，体现了接口的多态
   >
   > ```java
   > interface Chicken { void fly(); }
   > class Pig implements Chicken { 
   > 	@Override
   > 	public void fly() {
   > 		System.out.println("猪学会了飞");
   > 	}
   > }
   > class Animal {
   > //形参为接口
   > //who()接收:实现Chicken接口的类的对象实例
   > 	public void whoLearn(Chicken chicken) {
   >   		chicken.fly();
   > 	}
   > }
   > public static void main() {
   > 	Animal animal = new Animal();
   > 	Pig pig = new Pig();
   > 	animal.whoLearn(pig); //输出猪学会了飞
   > }
   > ```



- 向上转型

  ```java
  interface Person { }
  class Stu implements Person {  }
  
  	Person person = new Stu();
  ```

  

2. 多态数组

   > 接口 [ ] = new 接口 [ 数组长度 ];
   >
   > ```java
   > interface Person { }
   > class Stu implements Person { }
   > class Worker implements Person { }
   > 
   > Person[] persons = new Person[2];
   > persons[0] = new Stu();
   > persons[1] = new Worker();
   > ```



3. 多态传递

   > 类实现接口，接口继承另一个接口



---





# 枚举

> 枚举(enumeration)，简写enum
>
> 枚举是一组常量的集合
>
> 枚举是一种特殊的类，其中包含一组特定的对象，即 枚举对象/枚举值



## 定义

```java
enum 类名 {
    常量名(实参列表), 常量名(实参列表); //枚举对象/枚举值
	
    类成员...
}
```



## enum关键字使用须知

1. 定义枚举类时，默认继承 Enum类

   > 可以用 javap 反编译证明

2. 枚举对象与构造器要对应

3. 枚举对象之间用逗号间隔

4. 枚举对象必须放在第一行



## enum常用(内建)方法

- **toString**

  > Enum类中方法，用于返回当前对象名，子类可以重写方法，返回对象属性

  

- **name**

  > 返回当前对象名，子类不能重写

  

- **ordinal**

  > 返回当前对象的位置号，默认0开始

  

- **values**

  > 返回当前枚举类中所有常量
  
  ```java
  枚举类[] 数组名 = 枚举类.values();
  //增强for循环遍历得到枚举对象
  for(枚举类 i : 数组名) {
   System.out.println(i);
  }
  ```

  

- **valueOf**

  > 将字符串转换成枚举对象，**该字符串必须为已定义的常量名**

  

- **compareTo**

  > 比较两个枚举常量（的位置号）
  >
  > return self.ordinal - other.ordinal;



## 注意事项

1. 使用enum关键字后不能继承其他类

   >  隐式继承了Enum类，java单继承机制

2. 枚举类和普通类一样，能实现接口

   > enum 类名 implements 接口 { }







---







# 注解



**基本注解**

- @Override

  只能修饰方法

  

- @Deprecated

  表示某个程序元素已过时

  

- @SuppressWarnings( { "抑制条件" , "..." } )

  抑制编译器警告



如果发现 @interface，表示一个注解类



**元注解**

> 元注解用于修饰注解

- @Retention  注解的保留范围
- @Target  注解能修饰的程序元素
- @Documented  javadoc生成文档时，会保留被修饰的注解
- @Inherited  子类继承父类注解





---







# 异常处理

> 程序执行中的不正常情况即为“异常“（开发时语法错误和逻辑错误不是异常）



## 两类异常

- Error

  ​	Java虚拟机无法解决的严重问题，程序会崩溃

- Exception
      分为非受检异常(运行异常)和受检异常(编译异常)

  ​    编程错误或偶然的外在因素导致的一般性问题，可以使用针对性代码解决



## 异常体系图

IDEA中 `ctrl + alt + B `可以查看 Throwable 类的类图

（下图列举的部分异常中，红色为编译异常，蓝色为运行异常）

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%BC%82%E5%B8%B8%E4%BD%93%E7%B3%BB%E5%9B%BE.jpeg" style="zoom: 50%;" />

## 异常处理方式

- 编译异常必须处理

- 运行时异常默认使用 throws

  > 例如主方法中：public static void main(String[] args) throws Exception { ... }
  >
  > 👆这就是为什么没有写异常处理代码时，会输出异常信息



### try-catch-finally

> 程序员在代码中捕获发生的异常，自行处理
>
> IDEA中，ctrl + alt + T 生成



- 异常发生后，异常后边代码不会执行，直接进入 catch
- try - finally = throws



👇单 catch

```java
try {
	//可能有异常的代码...
}catch(Exception e) {
    //有异常时，执行下列步骤
    //1.捕获到异常
    //2.异常发生时，系统将异常封装成 Exception 对象 e，传递给catch
    //3.得到异常对象后，程序员自行处理
}finally {
    //无论是否有异常，finally总是执行
}
```

👇多 catch（子类在前，父类在后）

```java
try {
    int n1 = 10;
    int n2 = 0;
    int res = n1 / n2;
}catch(ArithmeticException e) {
    System.out.println("算数异常 = " + e.getMessage());
}catch(Exception e) {
    System.our.println(e.getMessage());
}finally {
    
}
```





### throws

> 将发生的异常抛出，交给调用者（方法）处理，顶级处理者为 JVM

- try-finally = throws

- 子类重写父类方法时，抛出的异常与父类一致或是父类异常的子类型

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/throws%E5%A4%84%E7%90%86%E6%9C%BA%E5%88%B6.gif" style="zoom:67%;" />

```java
class A throws Exception {
    //...
}
```





## 自定义异常

步骤

1. 定义类，类名自起，继承 Exception 或 RuntimeException
2. 继承 Exception 属于编译异常
3. 继承 RuntimeException 属于运行异常（一般继承 RuntimeException）

👇例题

```java
//继承RuntimeException
public class CustomException {
    public static void main(String[] args) {
        int age = 130;
        //要求年龄在18~120之间，否则抛出异常
        if (age >= 120 || age <=18) {
            throw new AgeException("年龄范围应在18~120");
        }
        System.out.println("年龄范围正确");
    }
}
public class AgeException extends RuntimeException{
    public AgeException(String message) {
        super(message);
    }
}
//继承Exception
public class CustomException {
    public static void main(String[] args) throws AgeException{
        int age = 130;
        //要求年龄在18~120之间，否则抛出异常
        if (age >= 120 || age <=18) {
            throw new AgeException("年龄范围应在18~120");
        }
        System.out.println("年龄范围正确");
    }
}
public class AgeException extends Exception{
    public AgeException(String message) {
        super(message);
    }
}
```





## throw与throws

![image-20220709222903265](https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220709222903265.png)









---







# 常用类



## 包装类

> 与八种基本数据类型对应的引用类型—包装类

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8C%85%E8%A3%85%E7%B1%BB.gif" style="zoom:67%;" />



### 装箱和拆箱

1. JDK 5之前，手动拆装箱

   ```java
   //手动装箱 int -> Integer
   int n1 = 100;
   //第一种方式 创建对象
   Integer integer1 = new Integer(n1);
   //第二种方式 valueOf方法
   Integer integer2 = Integer.valueOf(n1);
   
   //手动拆箱 Integer -> int
   //intValue方法
   int i1 = integer1.intValue();
   int i2 = integer2.intValue();
   ```

2. JDK 5之后，可以自动拆装箱

   ```java
   //自动装箱 int -> Integer
   int n1 = 100;
   Integer integer = n1; //底层使用 Integer.valueOf(n1)
   
   //自动拆箱 Integer -> int
   int n2 = integer; // 底层使用 integer.intValue()
   ```

   

### 包装类与String类转换

> 类比[基本数据类型与String转换](###基本数据类型和String类型的转换)

1. 包装类(Integer) 👉 String

   ```java
   Integer i = 10;
   //第一种方式
   String str1 = i + "";
   
   //第二种方式
   String str2 = i.toString();
   
   //第三种方式
   String str3 = String.valueOf(i);
   ```

2. String 👉 包装类(Integer)

   ```java
   String str = "123456";
   //第一种方式 parseInt方法
   Integer i1 = Integer.parseInt(str);
   
   //第二种方式 创建对象
   Integer i2 = new Integer(str);
   ```

   

### 注意 

1. 阅读源码可知，valueOf 方法，先看传递的参数的范围（Integer ：-128 ~ 127），再 return，超出范围则返回对象

2. == 比较时，只要有有基本数据类型，则判断值是否相等

   ```java
   Integer i1 = 128;
   int i2 = 128;
   System.out.println(i1 == i2); //true
   ```







## String类



### 结构

> String 对象用于保存字符串
>
> 字符串的字符使用Unicode字符编码，一个字符占两个字节
>
> String类有许多构造器
>
> 实现了 Serializable 接口 👉 String可以串行化（可以在网络传输）
>
> 实现了 Comparable 接口 👉 String 对象可以比较大小
>
> String 类的属性 private final char value[ ]; 👉 用于存放字符串内容，不能修改（value 不能指向其他地址，但是单个字符内容可以修改）



### 创建对象

1. 直接赋值

   > String str = "abc";
   >
   > 指向常量池

2. 调用构造器

   > String str = new String("abc");
   >
   > 指向堆中对象



### 字符串特性

1. String 是 final 类，代表不可变的字符序列

2. 字符串是不可变的。一个字符串对象一旦被分配，其内容不可变
   <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/image-20220506202455822.png" alt="image-20220506202455822" style="zoom: 67%;" />

3. 常量相加 👉 池

   变量相加 👉 堆
   ( 底层使用 StringBuilder 的 append 方法 ) 

4. intern() 指向常量池



### 常用方法

- - equals

  - equalsIgnoreCase

  - compareTo

    > 注意区分长度

- length

- - indexOf

    > 获取第一次出现的索引，从0开始，找不到则返回-1

  - lastIndexOf

    > 与indexOf相反，获取最后一次出现的索引

  - charAt

    > 获取指定索引处的字符

- trim

  > 去前后空格

- - toUpperCase
  - toLowerCase
  - toCharArray

- - concat

    > 拼接字符串

  - substring

    > 截取指定范围的子串

  - replace

    > 替换

  - split

    > 分割





## StringBuffer类

> 可变的字符序列，可以对字符串进行扩充和修改
>
> StringBuffer是一个容器



### String与StringBuffer

- String

  > 保存字符串常量，里边的值不能修改，每次更新就是更改地址，效率较低
  > private final char value[ ]; 👈常量池中

- StringBuffer

  > 保存字符串变量，里边的值可以修改，不用每次都更改地址，效率较高
  > char value[ ]; 👈堆中

**转换**

String -> StringBuffer

```java
//方法一 构造器
StringBuffer buffer = new StringBuffer(String str);

//方法二 append方法
String str = "a string";
StringBuffer buffer = new StringBuffer();
buffer = buffer.append(str);
```



StringBuffer -> String

```java
//方法一 构造器
String str = new String(StringBuffer buffer);

//方法二  toString方法
StringBuffer buffer = new StringBuffer("string");
String str = buffer.toString();
```



### 常用方法

- 增 append
- 删 delete
- 改 replace
- 查 indexOf
- 插 insert
- 长度 length





## StringBuilder类

> [StringBuilder](https://baike.baidu.com/item/StringBuilder)是一个可变的字符序列。此类提供一个与 StringBuffer 兼容的 API，但不保证同步。该类被设计用作 StringBuffer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候（这种情况很普遍）。

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/String%E6%AF%94%E8%BE%83.gif"  />

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/String%E9%80%89%E6%8B%A9.gif)







## Math类

### 常用方法（静态）

- abs - 绝对值

- pow - 幂

- ceil - 向上取整

- floor - 向下取整

- round - 四舍五入

- sqrt - 平方根

- random - 随机数

  > 获取 a ~ b 之间的随机数（a <= x <= b）
  >
  > (int) (Math.random() * (b - a + 1) + a)

- max
- min





## Arrays类

### 常用方法

- toString

- sort

  > 排序，分为自然排序和定制排序

- binarySearch

  > 二分查找，必须有序

- copyOf

  > 数组元素拷贝

- fill

  > 数组元素填充（替换）

- equals

- asList

  > 转换成List集合



### sort定制排序（Comparator）

> 传入 Comparator 实现定制排序

```java
Arrays.sort(arr , new Comparator() {
    @Override
    public int compare(Object o1, Object o2) {
        Integer i1 = (Integer)o1;
        Integer i2 = (Integer)o2;
		return i1 - i2; //返回的值影响排序
    }
});
```

**compare()函数返回值**

- return 0:不交换位置，不排序
- return 1:交换位置
- return -1:不交换位置
- return o1-o2:升序排列
- return o2-o1:降序排列





## System类

### 常用方法

- exit - 退出当前程序

  > exit(0) 表示程序退出，0表示一个正常的状态

- arraycopy

  > 复制数组元素，比较适合底层调用，一般使用 Array.copyOf 来复制数组

- currentTimeMillis

  > 获得自1970-1-01 00:00:00.000 到当前时刻的时间距离,类型为long

- gc

  > 运行垃圾回收机制



### 计算程序运行时间

```java
long startTime = System.currentTimeMillis();
//...一段代码
long endTime = System.currentTimeMillis();
System.out.println("该程序运行时间 = " + (endTime - startTime));
```







## BigInteger BigDecimal类

> 用于大数处理



### 对象创建

```java
//整数
BigInteger n1 = new BigInteger("1234567899999999999999");
//小数
BigDecimal n2 = new BigInteger("123.456789999999999999");
```



### 注意

- BigInteger 和 BigDecimal 进行加减乘除时，需要使用相应方法，不能直接加减乘除

- 👇方法

  - add 加

  - subtract 减

  - multiply 乘

  - divide 除

    > BigDecimal 在使用 divide 方法时如果除不尽，会抛出异常 ArithmeticException
    >
    > 解决办法：调用方法时指定精度
    >
    > ```java
    > BigDecimal n1 = new BigDecimal("123.456789999999999");
    > //如果是无限循环小数，结果会保留分子的精度
    > n1.divide(new BigDecimal(7), BigDecimal.ROUND_CEILING);
    > ```







## 日期类

### 第一代日期类 - Date

> 很多方法已被弃用，了解即可

**输出日期**

```java
Date d1 = new Date();
System.out.println(d1); // 默认输出日期为国外的格式

//1.使用 SimpleDateFormat 类的format方法可以指定格式
//2.格式使用的字母已经规定（SimpleDateFormat 类中查阅），不能随意写
SimpleDateFormat sdf = new SimpleDateFotmat("yyyy年MM月dd日 hh:mm:ss E"); // 指定格式
String format = sdf.format(d1); // 将日期转换成指定格式的字符串
System.out.println(format);
```





### 第二代日期类 - Calendar

> Calendar 类是一个抽象类，为日历字段之间的转换 and 操作日历字段（如获得下星期的日期）提供了一些方法



- 由于 Calendar 为抽象类，需要使用 getInstance 方法获取对象
- Calendar 没有专门的格式化方法，程序员自己组合显示即可

**获取某个日历字段**

```java
Calendar c = Calendar.getInstance();
//获取某个日历字段
System.out.println("年：" + c.get(Calendar.YEAR));
System.out.println("月：" + (c.get(Calendar.MONTH) + 1); // 返回月时，从0开始编号，需+1
System.out.println("日：" + c.get(Calendar.DAY_OF_MONTH));
```





### 第三代日期类

> 针对前两代日期类的不足，JDK 8加入了第三代日期类（LocalDate，LocalTime，LocalDateTime...）

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%AC%AC%E4%B8%89%E4%BB%A3%E6%97%A5%E6%9C%9F%E7%B1%BB.gif" style="zoom: 80%;" />

前两代日期类的不足：

> 第一代(Date)：JDK 1.0中包含 java.util.Date 类，但是它的大多数方法在 JDK 1.1引入 Calendar 类之后被弃用
>
> 第二代(Calendar)：
>
> ​	1.可变性：日期时间这样的类应该不可变
>
> ​	2.偏移性：月份都从0开始
>
> ​	3.格式化：Calendar 不能格式化
>
> ​	4.不是线程安全的



**关于输出日期和时间**

- now 方法

- DateTimeFormatter 格式日期类

  > 类似于 SimpleDateFormat
  >
  > 使用 ofPattern 方法格式化



**Instant 时间戳**

类似于 Date，提供了一系列与 Date 转换的方式

- Instant 👉 Date

  ```java
  Instant instant = new Instant();
  Date date = Date.from(instant);
  ```

- Date 👉 Instant

  ```java
  Date date = new Date();
  Instant instant = date.toInstant();
  ```





## 习题

1.String 翻转

```java
public class Practice01 {
    public static void main(String[] args) {
        String str = "abcdef";
        System.out.println("翻转前: " + str);
        try {
            String newStr = reverse(str, 1, 4);
            System.out.println("翻转后: " + newStr);
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }

    public static String reverse(String str, int start, int end) {
        if (!(str != null && start >= 0 && start < end && end < str.length())) {
            throw new RuntimeException("参数不正确");
        }
        char[] chars = str.toCharArray();
        char temp = ' '; // 交换辅助
        for (int i = start, j = end; i < j; i++, j--) {
            temp = chars[i];
            chars[i] = chars[j];
            chars[j] = temp;
        }
        return new String(chars);
    }
}
```








---







# 集合

> 可以动态保存多个对象，与数组相比，更加方便



## 集合体系图

👇黄色为接口，蓝色为实现类

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/Collection%E6%8E%A5%E5%8F%A3.gif"  />

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/Map%E6%8E%A5%E5%8F%A3.gif)







## Iterator迭代器

> 1. Iterator 对象称为迭代器，主要用于遍历 Collection 集合中的元素
> 2. 所有实现 Collection 接口的集合类都有一个 iterator 方法，可以返回一个迭代器
> 3. Iterator **仅用于遍历集合**，本身并不存放对象



IDEA 中，itit 或 ctrl + j (查看所有快捷键) 可生成 while 循环遍历

```java
Collection col = new ArrayList();
//得到 col 对应的迭代器
Iterator iterator = col.iterator();
//while 循环遍历
while (iterator.hasNext()) { //判断是否还有数据
    Object obj = iterator.next();
    System.out.println("obj = " + obj);
}
```





## 增强for循环

> 1. 可以代替迭代器遍历，简化版 Iterator，本质相同
> 2. **只能用于遍历集合和数组**



**格式**

IDEA 中，大写 i 可以生成

```java
for ( 元素类型 元素名 : 集合名或数组名 ) {
    //对元素的操作
}
```





## Collection 接口

**常用方法**

- add

  addAll

- remove

  removeAll

- contains

  containsAll

- size

- isEmpty

- clear



### List 接口

> Collection 的子接口，List 中元素**有序且可重复**
>
> 常用的实现类：ArrayList，LinkedList，Vector



**常用方法**

- add

  addAll

- remove

- indexOf
  lastIndexOf

- get (int  index)

  > 获取指定位置的元素

- set (int  index ,  E  element)

  > 设置指定位置的元素，相当于替换

- subList (int  fromIndex ,  int  toIndex)

  > 返回指定范围的子集合





#### ArrayList 类

- ArrayList 可以加入 null，并且多个
- 是由数组来实现数据存储的
- 基本等同于 Vector，除了ArrayList 是线程不安全的（执行效率高）

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/ArrayList%E5%BA%95%E5%B1%82%E6%93%8D%E4%BD%9C%E6%9C%BA%E5%88%B6.gif)





#### Vector 类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/Vector%E7%B1%BB.gif)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/ArrayList%E5%92%8CVector%E6%AF%94%E8%BE%83.gif)





#### LinkedList 类

1. LinkedList 底层实现双向链表和双端队列特点
2. 可以添加任意元素（可重复），包括 null
3. 线程不安全，没有实现同步

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/LinkedList.gif)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/ArrayList%E5%92%8CLinkedList%E6%AF%94%E8%BE%83.gif)









### Set 接口

1. Collection 的子接口，与 List 相反，Set 中的元素**无序且不可重复**

2. 无序：添加顺序和取出顺序不一致

3. 常用实现类：HashSet，TreeSet





#### HashSet 类

> Set 接口的实现类
>
> HashSet 的底层是 HashMap

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/HashSet.gif)



👇**底层**

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/HashSet%E5%BA%95%E5%B1%82.gif)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/HashSet%E6%89%A9%E5%AE%B9.gif)





**去重问题**

> IDEA 中，可以 alt + insert 快速重写方法，找到 equals() and hashCode() 即可

1. 当创建两个属性相同的不同对象时，如果不想添加到集合中，需要重写 equals 和 hashCode
2. 重写后，如果两个对象的属性相同，则 hashCode 相同，因此不会添加到 HashSet 集合中

```java
	@Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return age == employee.age && Objects.equals(name, employee.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age); // 这里认为只有name和age两个属性
    }
```





##### LinkedHashSet 类

> HashSet 的子类，实现了 Set 接口
>
> 底层是 LinkedHashMap

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/LinkedHashSet.gif)





#### TreeSet 类

> 底层是 TreeMap

- 使用无参构造器，创建 TreeSet 对象时，仍然无序
- 需要排序时，可以使用带 [Comparator](###sort定制排序（Comparator）) 的构造器

```java
TreeSet treeset = new TreeSet(new Comparator() {
    @Override
    public int compare(Object o1, Object o2) {
        return o1 - o2; // 根据需求返回值
    }
});
```







## Map 接口

> Map 用于保存具有映射关系的数据（Key - Value）
>
> key 不可以重复，value 可以重复（添加的 key 相同时， 替换 value）
>
> 常用实现类：Hashtable，Properties , HashMap，TreeMap

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/Map%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E7%B1%BB.gif)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/Map%E6%8E%A5%E5%8F%A3%E7%89%B9%E7%82%B9.gif)



### 常用方法

- put

  > 添加

- remove

  > 根据键删除映射关系

- get

  > 根据键获取值

- size

- isEmpty

- clear

- containsKey

- keySet

  > 获取所有键

- entrySet

  > 获取所有关系

- values

  > 获取所有值



### 遍历方式

```java
// 创建 Map 对象
Map map = new HashMap();

// 一：取出所有的 key
Set keyset = map.keySet();
// 1.增强for
for (Object key : keyset) {
    System.out.println(key + "-" + map.get(key));
}
// 2.迭代器
Iterator iterator1 = keyset.iterator();
while (iterator1.hasNext()) {
    Object key = iterator1.next();
    System.out.println(key + "-" + map.get(key));
}

// 二：取出所有 value
Collection values = map.values();
// 1.增强for
for (Object value : values) {
    System.out.println(value);
}
// 2.迭代器
Iterator iterator2 = values.iterator();
while (iterator2.hasNext()) {
    Object value = iterator2.next();
    System.out.println(value);
}

// 三：entrySet 获取 k-v
Set entrySet = map.entrySet();
// 1.增强for
for (Object entry : entrySet) {
    // 将 entry 转为Map.Entry
    Map.Entry m = (Map.Entry) entry;
    System.out.println(m.getKey() + "-" + m.getValue());
}
// 2.迭代器
Iterator iterator3 = entrySet.iterator();
while (iterator3.hasNext()) {
    Object entry = iterator3.next();
    Map.Entry m = (Map.Entry) entry;
    System.out.println(m.getKey() + "-" + m.getValue());
}
```





### HashMap 类

> Map 接口使用频率最高的实现类
>
> HashMap 以 k-v 对的方式来存储数据
>
> 线程不安全

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/HashMap%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6.gif)







### HashTable 类

> Map 接口的实现类
>
> 存放元素为键值对：K - V，键和值都不能为 null
>
> 线程安全
>
> 使用基本和 HashMap 一样

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/HashTable.gif)

![](A:\picture\java\note\HashTable和HashMap对比.gif)







### Properties

- HashTable 的子类，实现了 Map 接口，也是使用键值对的形式来保存数据，key 和 value 都不能为空

- 使用特点与 HashTable 类似

- 可以从 .properties 文件中，加载数据到 Properties 类对象，进行读取或修改







### TreeMap 类

- 使用无参构造器，创建 TreeMap 对象时，仍然无序
- 需要排序时，可以使用带 [Comparator](###sort定制排序（Comparator）) 的构造器

```java
TreeSet treeset = new TreeSet(new Comparator() {
    @Override
    public int compare(Object o1, Object o2) {
        return o1 - o2; // 根据需求返回值
    }
});
```







## 集合实现类的选择

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E9%9B%86%E5%90%88%E5%AE%9E%E7%8E%B0%E7%B1%BB%E7%9A%84%E9%80%89%E6%8B%A9.gif)







## Collections 工具类

> Collections 是一个操作 Set、List、Map 等集合的工具类
>
> 提供了一系列静态方法，对集合元素进行操作



**排序**

- reverse (List)：元素反转
- shuffle (List)：随机排序
- sort (List)：根据元素的自然顺序，按升序排序
- sort (List,  Comparator)：根据 Comparator 指定的顺序排序
- swap (List,  int,  int)：将指定的两处元素进行交换



**查找**

- max (Collection)：根据自然顺序，返回最大元素

  max (Collection,  Comparator)：根据 Comparator 指定的顺序，返回最大元素

- min (Collection)

  min (Collection,  Comparator)

- frequency (Collection,  Object)：返回指定集合中，指定元素出现的次数



**替换**

- replaceAll (List list,  Object oldVal,  Object newVal)：新值替换旧值



**拷贝**

- copy (List dest,  List src)：src 中的内容复制到 dest







---









# 泛型

> 泛型又称参数化类型，jdk 5.0 出现的新特性，解决数据类型的安全问题
>
> 编译时，泛型起到检查元素类型的作用，提高了安全性
>
> 减少了类型转换次数，提高效率

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E6%B3%9B%E5%9E%8B.gif)



**注意**

- 泛型指向的数据类型为引用类型，不能是基本数据类型

- 泛型指定具体类型后，可以传入该具体类型的子类型
- 如果不指定类型，默认为 Object





## 自定义泛型

### 类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%9B%E5%9E%8B.gif)

**代码演示**

```java
public class CustomGeneric {
    public static void main(String[] args) {
		// S = Integer, Y = String
        Person<Integer, String> p = new Person<>("shameyang", 10, "string");
    }
}

class Person<S, Y> {
    String name;
    S s;
    Y y;

    public Person(String name, S s, Y y) {
        this.name = name;
        this.s = s;
        this.y = y;
    }

    public String getName() {
        return name;
    }

    public S getS() {
        return s;
    }

    public Y getY() {
        return y;
    }
}
```





### 接口

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%9B%E5%9E%8B%E6%8E%A5%E5%8F%A3.gif)





### 方法

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%9B%E5%9E%8B%E6%96%B9%E6%B3%95.gif)





## 继承问题

泛型没有继承性！！





## 通配符

- < ? >：支持任意泛型类型
- < ? extends A >：支持 A 类和 A 类的子类，规定了泛型的上限
- < ? super A >：支持 A 类和 A 类的父类，规定了泛型的下限





---



# JUnit

> JUnit 是一个 Java 语言的单元测试框架
>
> 多数 Java 开发环境都已经集成了 JUnit

IDEA 中：

1. 方法前加 @Test

2. alt + enter 

3. 于是就能很方便的单独运行方法咯





---





# 坦克大战（1.0）



## 绘图原理

- Component 类提供了两个绘图相关的重要方法：

  1. paint (Graphics g)：绘制组件外观
  2. repaint ( )：刷新组件外观

  

- paint方法何时被调用？

  1. 当组件第一次在屏幕显示时，程序自动调用 paint() 方法来绘制组件
  2. 窗口最小化，再最大化
  3. 窗口大小发生改变
  4. repaint 方法被调用



**演示画圆**

```java
public class DrawCircle extends JFrame { // 窗口
    
    private MyPanel myPanel = null;

    public static void main(String[] args) {
        new DrawCircle();
    }

    public DrawCircle() {
        myPanel = new MyPanel();
        this.add(myPanel);
        this.setSize(1000,500);
        this.setLocation(200,200);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

// 定义 MyPanel 类，继承 JPanel类，在面板上画图形
class MyPanel extends JPanel {
    // 1.MyPanel 对象就是一个画板
    // 2.Graphics g 可以把 g 理解为一支画笔
    // 3.Graphics 提供了许多绘图方法
    @Override
    public void paint(Graphics g) {
        super.paint(g);
        g.drawOval(20, 20, 200, 200);
    }
}
```



![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/Graphics.gif)







---





# 事件处理机制



小球移动案例

```java
public class BallMove extends JFrame {
    MyPanel mp = null;

    public static void main(String[] args) {
        new BallMove();
    }

    public BallMove() {
        mp = new MyPanel();
        this.add(mp);
        this.setLocation(50, 50);
        this.setSize(1000, 700);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        //窗口 JFrame 对象可以监听键盘事件
        this.addKeyListener(mp);
    }
}

//画板，可以画出小球
//KeyListener 是监听器，可以监听键盘事件
class MyPanel extends JPanel implements KeyListener {
    //为了让小球移动，将左上角设置为变量
    int x = 100;
    int y = 100;

    @Override
    public void paint(Graphics g) {
        super.paint(g);
        g.fillOval(x, y, 20, 20);
    }

    //有字符输出时触法
    @Override
    public void keyTyped(KeyEvent e) {

    }

    //某个键按下时触发
    @Override
    public void keyPressed(KeyEvent e) {
//        System.out.println(e.getKeyCode());

        //根据用户按下的不同键，处理小球的移动
        if (e.getKeyCode() == KeyEvent.VK_W) {
            y -= 10;
        }
        if (e.getKeyCode() == KeyEvent.VK_S) {
            y += 10;
        }
        if (e.getKeyCode() == KeyEvent.VK_A) {
            x -= 10;
        }
        if (e.getKeyCode() == KeyEvent.VK_D) {
            x += 10;
        }
        //重绘面板
        this.repaint();
    }

    //某个键释放（松开）时触发
    @Override
    public void keyReleased(KeyEvent e) {

    }
}
```













---





# 多线程（基础）



## 线程相关概念

- 程序（program）

  > 是为完成特定任务、用某种语言编写的一组指令的集合
  >
  > 简单来说：就是我们写的代码

- 进程

  > ”进行中的程序“
  >
  > 进程是程序的一次执行过程，或是一个正在运行的程序

- 线程

  > 由进程创建，是进程的一个实体
  >
  > 一个进程可以有多个线程

- 单线程

  > 同一时刻，只允许执行一个线程

- 多线程

  > 同一时刻，可以执行多个线程

- 并发

  > 同一时刻，多个任务交替执行

- 并行

  > 同一时刻，多个任务同时执行



**查看 CPU 个数**

```java
public class CpuNums {
    public static void main(String[] args) {
        Runtime runtime = Runtime.getRuntime();
        int cpuNums = runtime.availableProcessors();
        System.out.println("当前计算机的cpu个数 = " + cpuNums);
    }
}
```







## 多线程机制



当主线程开启一个子线程时，主线程不会堵塞，会继续执行

可以使用 jconsole 监控线程执行情况

所有线程结束以后进程才会结束



为什么使用 start？

> 直接调用 run，就是 main 线程调用方法，阻塞了，下边代码不会先执行，即串行化了，不是多线程了











## 线程的基本使用



底层都是 start -> start0





**两种使用方法：**

1. 继承 Thread 类，重写 run 方法

   ```java
   package com.shameyang.thread;
   
   /**
    * @author ShameYang
    * @version 1.0
    * @description TODO演示继承 Thread 类的线程的使用：每隔1s输出一次，10次后停止运行
    * @date 2022/7/25 19:42:03
    */
   public class ThreadDemo {
       public static void main(String[] args) {
           Computer computer = new Computer();
           computer.start();
       }
   }
   
   class Computer extends Thread {
       int times = 0;
   
       @Override
       public void run() {
           while (true) {
               System.out.println("运行中..." + (++times) + Thread.currentThread().getName());
               try {
                   Computer.sleep(1000);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
               if (times == 100) {
                   System.out.println("程序结束...");
                   break;
               }
           }
       }
   }
   ```

   

2. 实现 Runnable 接口，重写 run 方法

   ```java
   package com.shameyang.runnable;
   
   /**
    * @author ShameYang
    * @version 1.0
    * @description TODO演示实现 Runnable 接口的线程的使用
    * @date 2022/7/27 22:38:22
    */
   public class RunnableDemo {
       public static void main(String[] args) {
           RunDemo runDemo = new RunDemo();
           Thread thread = new Thread(runDemo);
           thread.start();
       }
   }
   
   class RunDemo implements Runnable {
       int times = 0;
   
       @Override
       public void run() {
           while (true) {
               System.out.println("继承了Runnable接口..." + (++times) + Thread.currentThread().getName());
               try {
                   Thread.sleep(1000);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
               if (times == 10) {
                   System.out.println("程序结束...");
                   break;
               }
           }
       }
   }
   ```



![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%BA%BF%E7%A8%8B%E4%B8%A4%E7%A7%8D%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%E7%9A%84%E5%8C%BA%E5%88%AB.gif)





**通知线程退出**

```java
package com.shameyang.thread;

/**
 * @author ShameYang
 * @version 1.0
 * @description TODO演示通知线程退出
 * @date 2022/7/28 13:05:43
 */
public class ExitDemo {
    public static void main(String[] args) throws InterruptedException {
        Exit exit = new Exit();
        exit.start();
        // 主线程休眠 1s
        Thread.sleep(10 * 100);
        // 通知线程退出
        exit.setLoop(false);
    }

}

class Exit extends Thread {
    private int count = 0;
    private boolean loop = true;

    @Override
    public void run() {
        while (loop) {
            try {
                Exit.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("运行中..." + (++count) + getName());
        }
    }

    public void setLoop(boolean loop) {
        this.loop = loop;
    }
}
```





## 线程常用方法

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%BA%BF%E7%A8%8B%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%951.gif)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%BA%BF%E7%A8%8B%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95%E7%BB%86%E8%8A%82.gif)



![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%BA%BF%E7%A8%8B%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%952.gif)





## 用户线程和守护线程

守护线程：setDaemon 方法

守护线程可以监控管理用户线程

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%94%A8%E6%88%B7%E7%BA%BF%E7%A8%8Band%E5%AE%88%E6%8A%A4%E7%BA%BF%E7%A8%8B.gif)







## 线程状态

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81.gif)

<img src="A:\picture\java\note\线程状态转换图.gif" style="zoom: 80%;" />







## 线程同步



线程同步可以保证数据在同一时刻，最多有一个线程访问，以保证数据的完整性

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/Synchronized.gif)





## 互斥锁



this 对象锁是非公平锁



![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E4%BA%92%E6%96%A5%E9%94%81.gif)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E4%BA%92%E6%96%A5%E9%94%81%E7%BB%86%E8%8A%82.gif)







## 死锁

多个线程占用了对方的锁资源时，就会导致死锁

一定要避免死锁的发生







## 释放锁

同步代码块或同步方法中，释放锁的操作：

1. 执行结束
2. 遇到 break，return
3. 异常结束
4. 执行 wait 方法



不会释放锁：

- sleep 或 yield 方法
- 其他线程调用 suspend 方法，将当前线程挂起



提示：尽量避免使用 suspend() 和 resume()，方法已弃用







---







# 坦克大战（2.0）



## 我方发射子弹

1. 当发射一颗子弹后，就相当于启动一个线程
2. MyTank 有子弹的对象，按下 J 键时，就启动一个发射行为（线程）
3. MyPanel 不停重绘子弹，形成移动效果
4. 子弹到边缘时，就应销毁

```java
public class Shot implements Runnable {
    int x; // 子弹 x 坐标
    int y; // 子弹 y 坐标
    int direct = 0; // 子弹方向
    int speed = 5; // 子弹速度
    boolean isLive = true; //子弹是否存活

    //构造器
    public Shot(int x, int y, int direct) {
        this.x = x;
        this.y = y;
        this.direct = direct;
    }

    @Override
    public void run() { //射击

        while (true) {
            //休眠50毫秒
            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //根据方向改变 x, y 坐标
            switch (direct) {
                case 0: //上
                    y -= speed;
                    break;
                case 1: //右
                    x += speed;
                    break;
                case 2: //下
                    y += speed;
                    break;
                case 3: //左
                    x -= speed;
                    break;
            }

            //子弹碰到边缘或敌人坦克时，就应销毁
            if (!(x >= 0 && x <= 1000 && y >= 0 && y <= 750 && isLive)) {
                isLive = false;
                break;
            }
        }
    }
}
```





## 敌方发射子弹

1. 敌人坦克类中，使用 Vector 保存多个 Shot
2. 每创建一个坦克，给该坦克初始化一个 Shot 对象，同时启动 Shot
3. 当子弹 isLive = false 时，就从 Vector 移除

```java
public class EnemyTank extends Tank {

    Vector<Shot> shots = new Vector<>();
    boolean isLive = true;

    public EnemyTank(int x, int y) {
        super(x, y);
    }
}
```





## 敌方坦克随机移动

```java
public class EnemyTank extends Tank implements Runnable {

    Vector<Shot> shots = new Vector<>();
    boolean isLive = true;

    public EnemyTank(int x, int y) {
        super(x, y);
    }

    @Override
    public void run() {
        while (isLive) {
            //根据坦克的方向来继续移动
            switch (getDirect()) {
                case 0: //向上
                    //让坦克保持一个方向移动 30 个像素
                    for (int i = 0; i < 30; i++) {
                        moveUp();
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
                case 1: //向右
                    for (int i = 0; i < 30; i++) {
                        moveRight();
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
                case 2: //向下
                    for (int i = 0; i < 30; i++) {
                        moveDown();
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
                case 3: //向左
                    for (int i = 0; i < 30; i++) {
                        moveLeft();
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
            }

            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //随机改变坦克方向 0-3
            setDirect((int) (Math.random() * 4));

        }
    }
}
```











---







# IO 流

> I/O 是 Input/Output 的缩写，用于处理数据传输
>
> 如：读/写文件，网络通讯等



## 文件基础

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E6%96%87%E4%BB%B6%E6%B5%81.gif)





### 创建文件

**构造器**

1. new File (String pathname)
2. new File (File parent, String child)
3. new File (String parent, String child)

方法：createNewFile

- 只有使用了 createNewFile 方法，才会创建文件到磁盘





### 获取文件信息

**常用方法**

- getName
- getAbsolutePath
- getParent
- length
- exists
- isFile
- isDirectory





### 目录操作和文件删除

- mkdir () ：创建一级目录
- mkdirs () ：创建多级目录

- delete () ：删除空目录和文件





## IO流原理

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/IO%E6%B5%81%E5%8E%9F%E7%90%86.gif)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/IO%E6%B5%81%E5%8E%9F%E7%90%862.gif)





## 流的分类

- 按操作数据单位分为：字节流（8 bit），字符流（按字符）

- 按数据流的流向分为：输入流，输出流
- 按流的角色分为：节点流，处理流（包装流）

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E6%B5%81%E7%9A%84%E5%88%86%E7%B1%BB.gif)



![](https://www.runoob.com/wp-content/uploads/2013/12/iostream2xx.png)





## 字节流

### FileInputStream

> 文件字节输入流

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/FileInputStream1.gif)

程序读取数据

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/FileInputStream2.gif)

最后需要释放流

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/FileInputStream3.gif)



### FileOutputStream

> 文件字节输出流

使用构造器时，append 决定文件内容是覆盖还是追加

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/FileOutputStream1.gif)

程序将数据写入到文件

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/FileOutputStream2.gif)







## 字符流

### FileReader

> 文件字符输入流

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/FileReader.png)



### FileWriter

文件字符输出流

使用后必须 close 或 flush，否则无法写入到指定文件

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/FileWriter.png)







## 节点流和处理流

> 节点流可以从特定数据源（存放数据的地方）读取数据，如 FileReader、FileWriter
>
> 处理流（包装流）：是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。
> 如：BufferedReader 处理流的构造方法总是要带一个其他的流对象做参数。
>
> 一个流对象经过其他流的多次包装，称为流的链接。

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E8%8A%82%E7%82%B9%E6%B5%81%E5%92%8C%E5%A4%84%E7%90%86%E6%B5%81.png)



BufferedReader 处理流中，封装了节点流

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/BufferedReader02.png)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E8%8A%82%E7%82%B9%E6%B5%81%E5%92%8C%E5%A4%84%E7%90%86%E6%B5%812.png)







## 字符处理流

> 不要操作二进制文件（声音、视频、文档等），可能会造成文件损坏

### BufferedReader

```java
public class BufferedReader_ {
    public static void main(String[] args) throws Exception {
        //文件路径
        String filePath = "a:\\practice\\javase\\13_io\\write02.txt";
        //创建 BufferedReader 对象
        BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
        //按行读取
        String line;
        while ((line = bufferedReader.readLine()) != null) {
            System.out.println(line);
        }
        bufferedReader.close();
    }
}
```



### BufferedWriter

```java
public class BufferedWriter_ {
    public static void main(String[] args) throws IOException {
        //文件路径
        String filePath = "a:\\practice\\javase\\13_io\\bw.txt";
        //创建 BufferedWriter 对象
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath));
        //写入
        bufferedWriter.write("你好01");
        bufferedWriter.newLine();
        bufferedWriter.write("你好02");
        bufferedWriter.newLine();
        bufferedWriter.write("你好03");

        //关闭外层流 BufferedWriter
        bufferedWriter.close();
    }
}
```



## 文件拷贝

字符流

```java
public class BufferedCopy {
    public static void main(String[] args) throws Exception {
        String line;
        String srcFilePath = "a:\\practice\\javase\\13_io\\bw.txt"; //源路径
        String destFilePath = "a:\\practice\\javase\\13_io\\bwcopy.txt"; //目标路径
        BufferedReader br = new BufferedReader(new FileReader(srcFilePath));
        BufferedWriter bw = new BufferedWriter(new FileWriter(destFilePath));
        //边读边写
        while ((line = br.readLine()) != null) {
            bw.write(line);
            bw.newLine();
        }
        //关闭流
        br.close();
        bw.close();
    }
}
```



字节流

```java
public class BufferedCopy02 {
    public static void main(String[] args) throws IOException {
        String srcFilePath = "a:\\practice\\javase\\13_io\\bug.jpg";
        String destFilePath = "a:\\practice\\javase\\13_io\\bugcopy.jpg";

        //创建对象
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcFilePath));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFilePath));

        //循环读取文件
        byte[] buf = new byte[1024];
        int readLen = 0;
        while ((readLen = bis.read(buf)) != -1) {
            bos.write(buf, 0, readLen);
        }

        //关闭流
        bis.close();
        bos.close();
    }
}
```





## 对象处理流

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%BA%8F%E5%88%97%E5%8C%96%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%9601.png)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%BA%8F%E5%88%97%E5%8C%96%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96.png)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%AF%B9%E8%B1%A1%E5%A4%84%E7%90%86%E6%B5%81.png)

![](A:\picture\java\note\对象处理流细节.png)





## 标准输入输出流

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E6%A0%87%E5%87%86%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA.png)





## 转换流

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E8%BD%AC%E6%8D%A2%E6%B5%81.png)





## 打印流

打印流只有输出流，没有输入流

PrintStream 和 PrintWriter





## Properties

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/properties01.png)

![](A:\picture\java\note\properties02.png)



Properties 类读取文件

```java
package properties;

import java.io.FileReader;
import java.io.IOException;
import java.util.Properties;

/**
 * @author ShameYang
 * @version 1.0
 * @description TODO 使用 Properties 读取文件
 * @date 2023/3/22 16:43:37
 */
public class Properties01 {
    public static void main(String[] args) throws IOException {
        //使用 Properties 类读取 mysql.properties 文件

        //1. 创建 Properties 对象
        Properties properties = new Properties();
        //2. 加载指定配置文件信息
        properties.load(new FileReader("src\\mysql.properties"));
        //3. 控制台显示 k-v
        properties.list(System.out);
        //4. 根据指定 key 获取对应值
        String name = properties.getProperty("user");
        System.out.println("用户名 : " + name);
    }
}
```



Properties 类创建配置文件

```java
package properties;

import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

/**
 * @author ShameYang
 * @version 1.0
 * @description TODO 使用 Properties 类创建配置文件
 * @date 2023/3/22 16:53:11
 */
public class Properties02 {
    public static void main(String[] args) throws IOException {
        //使用 Properties 类创建配置文件 mysql02.properties

        Properties properties = new Properties();

        //创建
        properties.setProperty("user", "tom");
        properties.setProperty("pwd", "123456");
        properties.setProperty("ip", "中国");

        //将 k-v 存储
        properties.store(new FileOutputStream("src\\mysql02.properties"), null);
        System.out.println("配置文件保存成功");
    }
}
```







---



# 反射



反射可以通过外部配置文件，在不修改源码的情况下来控制程序，符合设计模式的 ocp 原则（开闭原则：不修改源码，扩展功能）





## 反射机制

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6.png)

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/java%E7%A8%8B%E5%BA%8F%E4%B8%89%E4%B8%AA%E9%98%B6%E6%AE%B5.png)





### 反射相关类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E7%9B%B8%E5%85%B3%E7%B1%BB.png)





### 反射调用优化

优缺点

- 优点：反射可以动态的创建和使用对象，也是框架底层核心
- 缺点：反射是解释执行，对执行速度有影响



调用优化 - 关闭访问检查

- 使用 setAccessible() 方法





## Class 类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/class%E7%B1%BB.png)



### 常用方法

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/class%E7%B1%BB%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95.png)



### 获取 Class 类对象

1. Class.forName()
   应用场景：多用于配置文件，读取全路径，加载类
   
2. 类.class
   应用场景：多用于参数传递
   
3. 对象.getClass()
   应用场景：有对象实例
   
4. 类加载器得到 Class 对象

   > ClassLoader cl = 对象.getClass().getClassLoader();
   >
   > cl.loadClass(classAllPath);

5. 基本数据类型.class

6. 包装类.TYPE



![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/class%E7%B1%BB%E5%9E%8B%E5%AF%B9%E8%B1%A1.png)





## 类加载

通过反射可以实现类的动态加载



静态加载：编译时加载相关的类，如果没有编写则报错，依赖性太强

动态加载：运行时加载需要的类，如果没有编写，动态加载类时，才会报错，降低了依赖性



### 类加载时机

> 1-3 均为静态加载，4 为动态加载

1. 创建对象时（new）
2. 子类被加载时，父类也会加载
3. 调用类中静态成员
4. 通过反射



### 类加载流程图

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%B5%81%E7%A8%8B%E5%9B%BE.png)





## 反射获取类结构信息



### java.lang.Class 类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E8%8E%B7%E5%8F%96%E7%B1%BB%E7%BB%93%E6%9E%84%E4%BF%A1%E6%81%AF1.png)





### java.lang.reflect.Field 类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E8%8E%B7%E5%8F%96%E7%B1%BB%E7%BB%93%E6%9E%84%E4%BF%A1%E6%81%AF2.png)





### java.lang.reflect.Method 类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E8%8E%B7%E5%8F%96%E7%B1%BB%E7%BB%93%E6%9E%84%E4%BF%A1%E6%81%AF3.png)



### java.lang.reflect.Constructor 类

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E8%8E%B7%E5%8F%96%E7%B1%BB%E7%BB%93%E6%9E%84%E4%BF%A1%E6%81%AF4.png)







## 反射创建对象实例

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E9%80%9A%E8%BF%87%E5%8F%8D%E5%B0%84%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1.png)







## 反射爆破

setAccessible(true)，使用反射可以访问 private 成员



### 访问属性

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E7%88%86%E7%A0%B41.png)



### 访问方法

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%8F%8D%E5%B0%84%E7%88%86%E7%A0%B42.png)



