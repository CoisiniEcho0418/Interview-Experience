# JDBC八股

------

## **JDBC 的执行步骤**

Java 数据库连接（JDBC）是一个用于执行 SQL 语句的 Java API，它为多种关系数据库提供了统一访问的机制。使用 JDBC 操作数据库通常涉及以下步骤：

#### 1. 加载数据库驱动

在与数据库建立连接之前，首先需要通过`Class.forName()`方法加载对应的数据库驱动。这一步确保 JDBC 驱动注册到了`DriverManager`类中。

```
Class.forName("com.mysql.cj.jdbc.Driver");
```

#### 2. 建立数据库连接

使用`DriverManager.getConnection()`方法建立到数据库的连接。这一步需要提供数据库 URL、用户名和密码作为参数。

```
Connection conn = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/databaseName", "username", "password");
```

#### 3. 创建`Statement`对象

通过建立的数据库连接对象`Connection`创建`Statement`、`PreparedStatement`或`CallableStatement`对象，用于执行 SQL 语句。

```
Statement stmt = conn.createStatement();
```

或者创建`PreparedStatement`对象（预编译 SQL 语句，适用于带参数的 SQL）：

```
PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM tableName WHERE column = ?");
pstmt.setString(1, "value");
```

#### 4. 执行 SQL 语句

使用`Statement`或`PreparedStatement`对象执行 SQL 语句。

执行查询（SELECT）语句时，使用`executeQuery()`方法，它返回`ResultSet`对象；

执行更新（INSERT、UPDATE、DELETE）语句时，使用`executeUpdate()`方法，它返回一个整数表示受影响的行数。

```
ResultSet rs = stmt.executeQuery("SELECT * FROM tableName");
```

或

```
int affectedRows = stmt.executeUpdate("UPDATE tableName SET column = 'value' WHERE condition");
```

#### 5. 处理结果集

如果执行的是查询操作，需要处理`ResultSet`对象来获取数据。

```
while (rs.next()) {
    String data = rs.getString("columnName");
    // 处理每一行数据
}
```

#### 6. 关闭资源

最后，需要依次关闭`ResultSet`、`Statement`和`Connection`等资源，释放数据库连接等资源。

```
if (rs != null) rs.close();
if (stmt != null) stmt.close();
if (conn != null) conn.close();
```

#### 总结

使用 JDBC 操作数据库的过程包括加载驱动、建立连接、创建执行语句、执行 SQL 语句、处理结果集和关闭资源。

在 Java 开发中，通常会使用 JDBC 模板库（如 Spring 的 JdbcTemplate）或 ORM 框架（如 Hibernate、MyBatis、MyBatis-Plus）来简化数据库操作和资源管理。



## **创建连接拿到的是什么对象**

在 JDBC 的执行步骤中，创建连接后拿到的对象是`java.sql.Connection`对象。这个对象是 JDBC API 中用于表示数据库连接的接口，它提供了执行 SQL 语句、管理事务等一系列操作的方法。

`Connection`对象代表了应用程序和数据库的一个连接会话。

通过调用`DriverManager.getConnection()`方法并传入数据库的 URL、用户名和密码等信息来获得这个对象。

一旦获得`Connection`对象，就可以使用它来创建执行 SQL 语句的`Statement`、`PreparedStatement`和`CallableStatement`对象，以及管理事务等。



### statement 和 preparedstatement 的区别

`Statement`和`PreparedStatement`都是用于执行 SQL 语句的接口，但它们之间存在几个关键的区别：

#### 1. 预编译

①、**Statement**：每次执行`Statement`对象的`executeQuery`或`executeUpdate`方法时，SQL 语句在数据库端都需要重新编译和执行。这适用于一次性执行的 SQL 语句。

②、**PreparedStatement**：代表预编译的 SQL 语句的对象。这意味着 SQL 语句在`PreparedStatement`对象创建时就被发送到数据库进行预编译。

之后，可以通过设置参数值来多次高效地执行这个 SQL 语句。这不仅减少了数据库编译 SQL 语句的开销，也提高了性能，尤其是对于重复执行的 SQL 操作。

#### 2. 参数化查询

- **Statement**：不支持参数化查询。如果需要在 SQL 语句中插入变量，通常需要通过字符串拼接的方式来实现，这会增加 SQL 注入攻击的风险。
- **PreparedStatement**：支持参数化查询，即可以在 SQL 语句中使用问号（?）作为参数占位符。通过`setXxx`方法（如`setString`、`setInt`）设置参数，可以有效防止 SQL 注入。

总的来说，`PreparedStatement`相比`Statement`有着更好的性能和更高的安全性，是执行 SQL 语句的首选方式，尤其是在处理含有用户输入的动态查询时。