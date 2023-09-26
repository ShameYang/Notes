# Maven 简介

## 传统项目开发存在的问题

- 一个项目做成一个工程，工程比较庞大，需要使用多模块来划分项目
- 项目中需要的许多 jar 包，需要手动下载并引入，并且多个项目的 jar 包会重复
- jar 包版本的兼容问题，需要手动解决
- 一个 jar 包依赖其他 jar 包时，需要手动解决



## Maven 概述

- Maven 是一款自动化构建工具，专注服务于 Java 平台的项目构建和依赖管理
- Maven 解决了传统项目开发存在的问题
- Maven 的作用：
  - 可以整合多个项目之间的引用关系
  - 规范管理各个常用 jar 包及其各个版本，并且可以自动下载并引入项目中
  - 可以根据指定的版本自动解决 jar 包版本兼容问题
  - 可以把一个 jar 包依赖的其他 jar 包自动下载并引入项目
- 类似的工具还有：Ant，Gradle



- 关于构建
  - 构建，是面向过程的，涉及到多个环节的协同工作
  - 构建的主要环节（清理、编译、测试、报告、打包、安装、部署）
    - 清理：删除之前的编译结果，为重新编译做好准备
    - 编译：将 Java 源程序编译为字节码文件
    - 测试：针对项目中的关键点进行测试，确保项目在迭代开发过程中关键点的正确性
    - 报告：在每一次测试后以标准的格式记录和展示测试结果
    - 打包：将一个包含诸多文件的工程封装为一个压缩文件用于安装和部署。Java 对应 jar 包，Web 对应 war 包
    - 安装：将打包的结果（jar 包或 war 包）安装到本地仓库
    - 部署：将打包的结果部署到远程仓库或将 war 包部署到服务器上运行



## Maven 核心概念简介

- POM
  - 一个文件，pom.xml，pom 翻译为项目对象模型（Project Object Model）
- 约定的目录结构
  - maven 项目的目录和文件的位置都是规定的
- 坐标
  - 一个唯一的字符串，用来表示资源
- 依赖管理
  - 管理项目可以使用的 jar 包
- 仓库管理（了解）
  - 资源存放的位置
- 生命周期（了解）
  - maven 工具构建项目的过程
- 插件和目标（了解）
  - 执行 maven 构建时用的工具就是插件
- 继承
- 聚合



## 安装 Maven

官网：https://maven.apache.org/



- 1）解压到非中文目录

  子目录

  - bin：执行程序，主要是 mvn.cmd
  - conf：maven 工具的配置文件

- 2）配置环境变量

  - MAVEN_HOME=Maven的安装路径
  - 将 MAVEN_HOME 添加到 Path 变量
    - %MAVEN_HOME%\bin

- 3）验证命令行 mvn -v







# Maven 核心概念

## Maven 工程约定目录结构

maven 的目录结构

```
Root
  |---src
  	|----main	#主程序java代码和配置文件
  		|---java
  		|---resources
  	|---test	#测试程序代码和文件（可以没有）
  		|---java
  		|---resources
  |---pom.xml	#maven核心文件（maven项目必须有）
```

- 使用 mvn compile 命令可以编译 src/main 目录下的所有 java 文件

  - 1）为什么下载？

    - maven 工具执行的操作需要很多插件

  - 2）下载了什么

    - jar 包--叫做插件--插件是完成某些功能的

  - 3）下载的存放位置

    - 默认仓库（本机仓库）

      C:\User\登录操作系统的用户名\\.m2\repository

  - 设置本机存放资源的位置

    - 修改 maven/conf/settings.xml 中的 \<localRepository>

- 执行完 mvn compile 命令后，会在项目根目录下生成 target 结果目录

- 编译后的 class 文件都会在 target 目录中



## POM 文件

> Project Object Model 项目对象模型。Maven 把一个项目的结构和内容抽象成一个模型，在 xml 文件中进行声明，以方便进行构建和描述。pom.xml 是 Maven 的灵魂



| 基本信息                   |                                                              |
| -------------------------- | ------------------------------------------------------------ |
| **配置项**                 | **描述**                                                     |
| modelVersion               | Maven 模型的版本，对于 Maven 2和3来说，只能是 4.0.0          |
| groupId                    | 组织 id，一般是公司域名的倒写                                |
| artifactId                 | 项目名称，也是模块名称，对应 groupId 中的项目中的子项目      |
| version                    | 项目的版本号。如果项目还在开发中，不是稳定版本，通常在版本后加 -SNAPSHOT |
| packaging                  | 项目打包的类型，可以是 jar、war、rar、ear、pom，默认为 jar   |
| **依赖**                   |                                                              |
| dependencies 和 dependency | 用来配置依赖，相当于 java 中的 import                        |
| **配置属性**               |                                                              |
| properties                 | 定义一些配置属性，例如编码方式                               |
| **构建**                   |                                                              |
| build                      | 与构建相关的配置，例如设置编译插件的 JDK 版本                |



## 仓库

- 仓库，用来存放 maven 的插件（各种 jar）和项目使用的 jar 包（第三方工具）
- 仓库的分类
  - 本地仓库
  - 远程仓库
    - 中央仓库
      - 最权威的，所有开发人员都共享使用
    - 中央仓库的镜像
      - 中央仓库的备份，各大洲，重要的城市都是镜像
    - 私服
      - 公司内部的，不对外使用

- 仓库的使用

  开发人员需要使用 mysql 驱动 --> 首先查本地仓库 --> 私服 --> 镜像 --> 中央仓库



## 坐标

坐标在仓库中可以唯一指定一个 Mavn项目

- groupId：组织 id，一般是公司域名的倒写
- artifactId：模块名，通常是工程名
- version：版本号



## 依赖

一个 Maven 项目的运行需要其他项目的支持，这就是依赖。Maven 会根据坐标自动到本地仓库进行查找，我们需要在 pom.xml 中加入依赖

```xml
<dependency>
    <groupId>xxx</groupId>
    <artifactId>xxx</artifactId>
    <version>x.x.x</version>
</dependency>
```



## Maven 常用命令

| 命令             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| mvn clean        | 清理（删除原来编译和测试的目录，即 target 目录，但是已经 install 到仓库里的包不会删除） |
| mvn compile      | 编译主程序（会在当前目录生成一个 target 目录，存放主程序中编译生成的字节码文件，拷贝 resources 中的文件到 target） |
| mvn test-compile | 编译测试程序（会在当前目录生成一个 target 目录，存放测试程序中编译生成的字节码文件） |
| mvn test         | 测试（生成一个 surefire-reports，保存测试结果）              |
| mvn package      | 打包主程序（按照 pom.xml 配置把主程序打包成 jar 包或 war 包） |
| mvn install      | 安装主程序（把本工程打包，按照坐标保存到本地仓库）           |
| mvn deploy       | 部署主程序（把本工程打包，按照坐标保存到本地仓库，并且会保存到私服仓库中，还会自动把项目部署到 web 容器中） |

- 自上而下是构建的过程，执行下边的命令时，上边的命令都会执行一次







# Maven 在 IDEA 中的应用

## 配置 Maven

- 以 2023.2.1 版本的 IDEA 为例，不同版本自行查阅即可

- 当前工程设置

  - Settings --> Build, Execute... --> Build Tool --> Maven
  - <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/Maven%E9%85%8D%E7%BD%AE1.png" style="zoom:67%;" /> 

  - 点击上图中的 Runner，输入 -DarchetypeCatalog=internal
  - <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/Maven%E9%85%8D%E7%BD%AE2.png" style="zoom: 67%; float:left" />



- 新工程设置
  - File --> New Projects Setup --> Settings for New Projects...
  - 打开后，设置同上



## 创建 JavaSE 项目

- <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/IDEA%E5%88%9B%E5%BB%BAMaven2.png" style="zoom:67%; float:left" />



## 创建 WEB 项目

- <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/IDEA%E5%88%9B%E5%BB%BAweb%E9%A1%B9%E7%9B%AE.png" style="zoom:67%; float:left" />



- 创建后，如果爆红就刷新一下：右键 pom.xml --> Maven --> Reload...

  <img src="A:\picture\Maven\IDEA创建web项目2.png" style="zoom:67%;float:left" />







# 依赖管理

## 依赖的范围

以 junit 为例

- ```xml
  <dependency>
  	<groupId>junit</groupId>
  	<artifactId>junit</artifactId>
  	<version>3.8.1</version>
  	<scope>test</scope> <!-- 这个就是依赖的范围 -->
  </dependency>
  ```

- 他的依赖范围是 test，说明就在测试中起作用

| scope 取值 | 描述                       |
| ---------- | -------------------------- |
| compile    | 默认值，在所有阶段都会使用 |
| test       | 只在测试阶段起作用         |
| provided   | 不参与打包与部署           |







# Maven 常用设置

## 属性设置

```xml
<properties>
    <!-- maven 构建项目使用编码，避免中文乱码 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!-- 编译代码使用的 jdk 版本 -->
    <maven.compiler.source>JDK 版本</maven.compiler.source>
    <!-- 运行代码使用的 jdk 版本 -->
    <maven.compiler.target>JDK 版本</maven.compiler.target>
    
    <!-- 自定义变量,这里以 spring.version 为例 -->
    <spring.version>5.2.0</spring.version>
</properties>

<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
        <!-- 这里使用 ${变量名} 就可以使用自定义的变量 -->
		<version>${spring.version}</version>
	</dependency>
    <dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-aop</artifactId>
		<version>${spring.version}</version>
	</dependency>
</dependencies>
```



## 指定资源位置

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory> <!-- 所在的目录 -->
            <includes> <!-- 目录下的 .properties .xml 文件都会扫描到 -->
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <!-- 不启用过滤器，*.property 已经起到了过滤作用  -->
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

- 作用（mybatis 中会用到）
  - 默认没有使用 resources 时，maven 执行 compile 后，会把 src/main/resources 中的文件拷贝到 target/classes 中，src/main/java 中的非 java 文件不处理
  - 使用 resources 后，在 compile 后就可以把指定的资源，一同拷贝到 target/classes 中
