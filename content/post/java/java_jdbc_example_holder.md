---
title: "JDBC链接各种数据库的例子 / Java jdbc example holder"
date: 2019-09-13T02:37:24+08:00
draft: false
tags: ["java", "jdbc", "database"]
categories: ["java", "jdbc", "database"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

> java语言下 jdbc 各种数据库的链接方式

## mysql/mariadb

```java

private static Connection getConn() {
    String driver = "com.mysql.jdbc.Driver";
    String url = "jdbc:mysql://localhost:3306/samp_db";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## oracle

- (1)Oracle JDBC Thin using a ServiceName:

```
    #jdbc:oracle:thin:@//<host>:<port>/<service_name>
    #Example: jdbc:oracle:thin:@//192.168.2.1:1521/XE
```

- (2)Oracle JDBC Thin using an SID:

```
    #jdbc:oracle:thin:@<host>:<port>:<SID>
    #Example: jdbc:oracle:thin:@192.168.2.1:1521:X01A
```

- (3)Oracle JDBC Thin using a TNSName:

```
    #jdbc:oracle:thin:@<TNSName>
    #Example: jdbc:oracle:thin:@GL
```

```java

// 获得连接对象
private static Connection getConn() {
    String driver = "oracle.jdbc.driver.OracleDriver";
    String url = "jdbc:oracle:thin:@//127.0.0.1:1521/orcl";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## sqlserver

```java

// sqlserver2012
private static Connection getConn() {
    String driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver";
    String url = "jdbc:sqlserver://localhost:1433;DatabaseName=student";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## db2

```java

private static Connection getConn() {
    String driver = "com.ibm.db2.jcc.DB2Driver";
    String url = "jdbc:db2://10.27.70.33:60000/dbtest:currentSchema=db2inst1";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## PostgreSQL

```java

private static Connection getConn() {
    String driver = "org.postgresql.Driver";
    String url = "jdbc:postgresql://127.0.0.1:5432/test";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## greenplum

```java

private static Connection getConn() {
    String driver = "com.pivotal.jdbc.GreenplumDriver";
    String url = "jdbc:pivotal:greenplum://10.10.10.10:5432;DatabaseName=mfg";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## hana

```java

private static Connection getConn() {
    String driver = "com.sap.db.jdbc.Driver";
    String url = "jdbc:sap://172.23.1.123:39015";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## hive

```java

private static Connection getConn() {
    /**
    hiveserver org.apache.Hadoop.hive.jdbc.HiveDriver
                    jdbc:hive://localhost:10000/default

    hiveserver2 org.apache.Hive.jdbc.HiveDriver
                    jdbc:hive2://localhos:10000/default
    */
    String driver = "org.apache.hive.jdbc.HiveDriver";
    String url = "jdbc:hive2://localhos:10000/default";
    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```

## sybase

```java

private static Connection getConn() {
    String Driver = "com.sybase.jdbc3.jdbc.SybDriver";  //数据库驱动
    String url = "jdbc:sybase:Tds:192.168.2.103:5000/SXABC"; // 连接的数据库是SXABC

    String username = "root";
    String password = "";
    Connection conn = null;
    try {
        Class.forName(driver); //classLoader,加载对应驱动
        conn = (Connection) DriverManager.getConnection(url, username, password);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return conn;
}

```
