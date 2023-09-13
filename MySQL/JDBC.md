# JDBC 基本概念

- 什么是 JDBC

  Java DataBase Connectivity，在 Java 语言中编写 SQL 语句，对数据库中数据进行 CRUD 操作

  

- JDBC 相关类库在哪里

  java.sql.*

  

- JDBC 本质

  本质上是 java.sql 包下的一堆接口，由 SUN 公司制定

  JDBC 是体现接口作用的典型案例，降低了耦合度，提高了扩展力

  无需关注哪个数据库，直接面向 JDBC 接口编程







# JDBC 编程步骤

> 前置工作：在项目根目录下创建一个 lib 文件夹，将 mysql.jar 拷贝到该目录下，右键 add as library（等同于为 jar 包配置环境变量）



1. 注册驱动 - 加载 Driver 类

   通知 java 程序要连接哪个数据库

   ```java
   //第一种方式
   DriverManager.deregisterDriver(new com.mysql.cj.jdbc.Driver()); // MySQL 8.0 写法
   
   //第二种方式：类加载注册
   Class.forName(com.mysql.cj.jdbc.Driver); // MySQL 8.0 写法
   ```

   

2. 获取数据库连接 - 得到 Connection

   开启 java 进程和 mysql 进程间的通道

   ```java
   conn = DriverManager.getConnection(String url, String user, String password);
   ```

   

3. 获取数据库操作对象

   这个对象用来执行 SQL语句

   ```java
   stmt = conn.createStatement();
   ```

   

4. 执行 SQL 语句

   执行 CRUD 操作

   ```java
   //DML
   String sql = "DML 语句";
   stmt.execute(sql);
   
   //DQL
   String sql = "select ...";
   rs = stmt.executeQuery(sql);
   ```

   

5. 处理查询结果集

   如果第 4 步是 select 语句，才有这一步

   ```java
   while (rs.next()) {
   	String name = rs.getString("字段名");
   	System.out.println(name);
   }
   ```

   

6. 释放资源

   ```java
   //rs 可替换为任何要释放的对象
   if (rs != null) {
   	try {
   		rs.close();
   	} catch (SQLException e) {
   		e.printStackTrace();
   	}
   }
   ```







# JDBC 编程完整代码

## DML

```java
public class JDBC01 {
    public static void main(String[] args) throws SQLException {
        //分别创建 Connection、Statement、ResultSet 对象
        Connection conn = null;
        Statement stmt = null;
        try {
            //1.注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver"); //这里是 MySQL 8.0 写法
            //2.获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc", "root", "123456");
            //3.获取数据库操作对象
            stmt = conn.createStatement();
            //4.执行 SQL
            String sql = "insert into student(id, name, sex) values(01, '张三', '男')";
			stmt.execute(sql);
            }
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            //5.释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```



## DQL

```java
public class JDBC01 {
    public static void main(String[] args) throws SQLException {
        //分别创建 Connection、Statement、ResultSet 对象
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //1.注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver"); //这里是 MySQL 8.0 写法
            //2.获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc", "root", "123456");
            //3.获取数据库操作对象
            stmt = conn.createStatement();
            //4.执行 SQL
            String sql = "select name as '学生姓名' from student where id = 01";
            rs = stmt.executeQuery(sql);
            //5.处理查询结果集
            while (rs.next()) {
                String name = rs.getString("学生姓名");
                System.out.println(name);
            }
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```









# 读取配置文件完成 JDBC

> src 包下新建 resources 包，新建 db.properties 配置文件
>
> 在原来的程序下创建资源绑定器对象，得到配置文件中的信息
>
> 使用其他数据库时，直接修改配置文件即可



配置文件

```properties
# mysql connectivity configuration
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/jdbc
user=root
password=123456
```

程序代码

```java
public class JDBC_Properties_Test {
    public static void main(String[] args) throws SQLException {
        //资源绑定器
        ResourceBundle bundle = ResourceBundle.getBundle("resources/db");
        //通过配置文件得到信息
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");

        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //1.注册驱动
            Class.forName(driver);
            //2.获取连接
            conn = DriverManager.getConnection(url, user, password);
            //3.获取数据库操作对象
            stmt = conn.createStatement();
            //4.执行 SQL
            String sql = "select name as '学生姓名' from student where id = 01";
            rs = stmt.executeQuery(sql);
            //5.处理查询结果集
            while (rs.next()) {
                String name = rs.getString("学生姓名");
                System.out.println(name);
            }
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```







# 模拟用户登录（SQL 注入问题）

> 1. 提供一个输入界面，可以让用户输入用户名和密码
> 2. 底层数据库中需要一张用户表，表中存储用户信息
> 3. 当 java 程序接收到用户名和密码时，连接数据库验证用户名和密码



## 程序代码

> 存在问题：SQL 注入
>
> 问题主要原因：用户提供的信息参与了 SQL 语句的编译
>
> 例如：
>
> 用户名 abc，密码 aaaaa ' or ' 1 = 1 也会显示登录成功

```java
public class SimulateLogin {
    public static void main(String[] args) {
        //初始化一个界面，可以让用户输入用户名和密码
        Map<String, String> userLoginInfo = initUI();

        //连接数据库验证用户名和密码是否正确
        boolean ok = checkNameAndPwd(userLoginInfo.get("loginName"), userLoginInfo.get("loginPwd"));

        System.out.println(ok ? "登录成功" : "登录失败");
    }

    /**
     * 验证用户名和密码
     * @param loginName 用户名
     * @param loginPwd 密码
     * @return true 表示登录成功，false 表示登录失败
     */
    private static boolean checkNameAndPwd(String loginName, String loginPwd) {
        boolean ok = false;
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        //资源绑定器
        ResourceBundle bundle = ResourceBundle.getBundle("resources/db");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");

        try {
            //1.注册驱动
            Class.forName(driver);
            //2.获取数据库连接
            conn = DriverManager.getConnection(url, user, password);
            //3.获取数据库操作对象
            stmt = conn.createStatement();
            //4.执行 SQL 语句
            String sql = "select * from t_user where login_name = '" + loginName
                    + "' and login_pwd = '" + loginPwd + "'";
            rs = stmt.executeQuery(sql);
            //5.处理查询结果集
            //如果以上 sql 语句中用户名和密码正确，那么结果最多返回一条记录，所以无需循环
            if (rs.next()) {
                ok = true;
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return ok;
    }

    /**
     * 初始化界面，接收用户输入
     * @return 存储用户名和密码的 Map 集合
     */
    private static Map<String, String> initUI() {
        //登录界面
        System.out.println("--- 欢迎使用模拟系统！请输入用户名和密码 ---");
        Scanner sc = new Scanner(System.in);
        System.out.print("用户名：");
        String loginName = sc.nextLine();
        System.out.print("密码：");
        String loginPwd = sc.nextLine();

        //将用户名和密码存放到 Map 集合中
        Map<String, String> userLoginInfo = new HashMap<>();
        userLoginInfo.put("loginName", loginName);
        userLoginInfo.put("loginPwd", loginPwd);

        //返回 Map
        return userLoginInfo;
    }
}
```

## sql 文件

```sql
# 用户信息表，模拟用户登录使用
drop table if exists t_user;

create table t_user
(
    id         int primary key auto_increment,
    login_name varchar(255) unique,
    login_pwd  varchar(255) not null,
    real_name  varchar(255)
);

insert into t_user(login_name, login_pwd, real_name)
values ('admin', '123', '管理员'),
       ('ZhangSan', '123', '张三');

select * from t_user;
```



## 解决 SQL 注入问题

> SQL 注入的根本原因：先进行了字符串的拼接，再进行的 SQL 编译
>
> - java.sql.Statement
>
>     先进行字符串的拼接，再进行 SQL 编译
>
>   - 优点：可以进行 SQL 语句的拼接
>   - 缺点：SQL 注入问题
>
> - java.sql.PreparedStatement
>
>     先进行 SQL 编译，再进行 SQL 传值
>
>   - 优点：避免 SQL 注入
>   - 缺点：只能给 SQL 语句传值



步骤

- 将 Statement 更换为 PreparedStatement

- 修改之前 JDBC 步骤的第3步（获取数据库对象）和第4步（执行 SQL），如下：

  ```java
  //1.注册驱动
  Class.forName(driver);
  //2.获取数据库连接
  conn = DriverManager.getConnection(url, user, password);
  
  //3.获取预编译的数据库操作对象
  //一个问号 ? 是一个占位符，每个占位符只能接收 1 个值或数据
  String sql = "select * from t_user where login_name = ? and login_pwd = ?";
  stmt = conn.prepareStatement(sql);
  //给占位符传值（JDBC 中所有下标都从 1 开始）
  stmt.setString(1, loginName); //1 代表第一个 ?
  stmt.setString(2, loginPwd); // 2 代表第二个 ?
  
  //4.执行 SQL 语句
  rs = stmt.executeQuery(); //去掉参数
  ```

完整代码

```java
public class SimulateLogin {
    public static void main(String[] args) {
        //初始化一个界面，可以让用户输入用户名和密码
        Map<String, String> userLoginInfo = initUI();

        //连接数据库验证用户名和密码是否正确
        boolean ok = checkNameAndPwd(userLoginInfo.get("loginName"), userLoginInfo.get("loginPwd"));

        System.out.println(ok ? "登录成功" : "登录失败");
    }

    /**
     * 验证用户名和密码
     * @param loginName 用户名
     * @param loginPwd 密码
     * @return true 表示登录成功，false 表示登录失败
     */
    private static boolean checkNameAndPwd(String loginName, String loginPwd) {
        boolean ok = false;
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;

        //资源绑定器
        ResourceBundle bundle = ResourceBundle.getBundle("resources/db");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");

        try {
            //1.注册驱动
            Class.forName(driver);
            //2.获取数据库连接
            conn = DriverManager.getConnection(url, user, password);
            //3.获取预编译的数据库操作对象
            //一个问号 ? 是一个占位符，每个占位符只能接收 1 个值或数据
            String sql = "select * from t_user where login_name = ? and login_pwd = ?";
            ps = conn.prepareStatement(sql);
            //给占位符传值（JDBC 中所有下标都从 1 开始）
            ps.setString(1, loginName); //1 代表第一个 ?
            ps.setString(2, loginPwd); // 2 代表第二个 ?
            //4.执行 SQL 语句
            rs = ps.executeQuery();
            //5.处理查询结果集
            //如果以上 sql 语句中用户名和密码正确，那么结果最多返回一条记录，所以无需循环
            if (rs.next()) {
                ok = true;
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (ps != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return ok;
    }

    /**
     * 初始化界面，接收用户输入
     * @return 存储用户名和密码的 Map 集合
     */
    private static Map<String, String> initUI() {
        //登录界面
        System.out.println("--- 欢迎使用模拟系统！请输入用户名和密码 ---");
        Scanner sc = new Scanner(System.in);
        System.out.print("用户名：");
        String loginName = sc.nextLine();
        System.out.print("密码：");
        String loginPwd = sc.nextLine();

        //将用户名和密码存放到 Map 集合中
        Map<String, String> userLoginInfo = new HashMap<>();
        userLoginInfo.put("loginName", loginName);
        userLoginInfo.put("loginPwd", loginPwd);

        //返回 Map
        return userLoginInfo;
    }
}
```







# JDBC 事务

> 实际开发中需要将自动提交机制关闭，改为手动提交



1. 得到 Connection 连接后，关闭自动提交机制

   ```java
   conn = DriverManager.getConnection(url, user, password);
   conn.setAutoCommit(false); // 关闭自动提交机制
   ```

2. 执行 SQL 以后，手动提交

   ```java
   conn.commit(); // 手动提交
   ```

3. 如果出现异常，手动回滚

   ```java
   try {
       ...
   } catch (Exception e) {
       try {
       	if (conn != null) {
           	conn.rollback(); // 出现异常，手动回滚
       	}
       } catch (SQLException ex) {
           ex.printStackTrace();
       }
   }
   ```







# JDBC 工具类封装

> 为了方便开发，可以封装一个 JDBC 工具类

```java
public class DBUtil {
    private DBUtil() {

    }

    //类加载时绑定属性资源文件
    private static final ResourceBundle bundle = ResourceBundle.getBundle("resources/db");

    //注册驱动
    static {
        try {
            Class.forName(bundle.getString("driver"));
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接对象
     * @return 新的连接对象
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = DriverManager.getConnection(url, user, password);
        return conn;
    }

    /**
     * 释放资源
     * @param conn 连接对象
     * @param stmt 数据库操作对象
     * @param rs 查询结果集
     */
    public static void close(Connection conn, Statement stmt, ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

