---
title: Mybatis学习笔记
date: 2017-02-19 22:56:00
tags: Mybatis
categories: Java EE
---

Mybatis是支持定制化sql，存储过程以及高级映射的优秀的持久层框架。避免所有的jdbc代码和手动设置参数以及获取结果集。可以使用xml或者注解方式进行。将Java和普通的POJO映射成数据库记录。<!--more-->

## 使用方式

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.4.2</version>
</dependency>
```

### 使用xml配置

```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

####  配置文件编写

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

#### 从 SqlSessionFactory 中获取 SqlSession

```Java
SqlSession session = sqlSessionFactory.openSession();
try {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
} finally {
  session.close();
}
```

#### 探究已映射的 SQL 语句

命名空间是必须的。

永远不要拘泥于一种方式，你可以很轻松的在基于注解和 XML 的语句映射方式间自由移植和切换。

## 作用域（Scope）和生命周期

- SqlSessionFactoryBuilder最好创建SqlSessionFactory就被丢弃。

- SqlSessionFactory作用域为应用级别，单例模式

- 每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的。确保每个线程使用完会话就关闭。

- 最好把映射器放在方法作用域（method scope）内。

## XML 映射配置文件

如果属性在不只一个地方进行了配置，那么 MyBatis 将按照下面的顺序来加载：

- 在 properties 元素体内指定的属性首先被读取。
- 然后根据 properties 元素中的 resource 属性读取类路径下属性文件或根据 url 属性指定的路径读取属性文件，并覆盖已读取的同名属性。
- 最后读取作为方法参数传递的属性，并覆盖已读取的同名属性。
- [Mybatis属性设置](http://www.mybatis.org/mybatis-3/zh/configuration.html)
- 要注意 MyBatis 不会窥探数据库元信息来决定使用哪种类型，所以你必须在参数和结果映射中指明那是 VARCHAR 类型的字段， 以使其能够绑定到正确的类型处理器上。 这是因为：MyBatis 直到语句被执行才清楚数据类型。
- ​

