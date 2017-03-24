---
title: Mybatis学习笔记
date: 2017-02-19 22:56:00
tags: Mybatis
categories: Java EE
---

Mybatis是支持定制化sql，存储过程以及高级映射的优秀的持久层框架。避免所有的jdbc代码和手动设置参数以及获取结果集。可以使用xml或者注解方式进行。将Java和普通的POJO映射成数据库记录。<!--more-->



## jdbc缺陷

- 数据库频繁的创建和关闭，浪费数据库资源，影响操作效率（数据库连接池）

- sql硬编码，不利于维护

- preparestatement设置占位符，存在参数未知不确定问题，系统不易维护

  ​

## Mybatis架构

![](https://ooo.0o0.ooo/2017/03/24/58d4959824390.png)

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


## SqlSession的使用范围

- sqlSession线程不安全，最佳的使用范围是在方法 体内。

## mapper开发规范

- namespace指定为mapper的全限定名。
- mapper的命令方式为表名+mapper
- mapper.xml中的id作为接口的方法名
- 方法参数和映射参数一致
- 返回结果一致

## 问题

- 如果statement返回多条记录，而java中返回pojo。此时会报错。
- 输入参数的问题，使用mapper代理的方式来开发，mapper方法的输入参数只有一个，可拓展性方面没有问题。因为dao层本身就是通用的。

## mybtis全局配置 

1. ```xml
       <properties resource="db.properties"></properties>
   	<typeAliases>
   		<typeAlias type="tech.jiangtao.mybatis.pojo.User" alias="user"/>
   	</typeAliases>
   ```

2. ```xml
   <typeAliases>
   		<typeAlias type="tech.jiangtao.mybatis.pojo.User" alias="user"/>
   		<!-- 批量别名 类名的大小写都成-->
   		<package name="tech.jiangtao.mybatis.pojo"/>
   	</typeAliases>
   ```

3. ```xml
   <!-- 加载mapper.xml -->
   	<mappers>
   		<mapper resource="sqlmap/User.xml" />
   		<mapper resource="mapper/UserMapper.xml" />
   		
   		<!-- 自动扫描mapper接口 -->
         <package name="tech.jiangtao.mybatis.mapper"/>
   	</mappers>
   ```



##  通过拓展pojo来实现包装类型传参

```java
package tech.jiangtao.mybatis.pojo;

public class UserQuery {

	private User user;
	
	private UserCustom userCustom;

	public User getUser() {
		return user;
	}

	public void setUser(User user) {
		this.user = user;
	}

	public UserCustom getUserCustom() {
		return userCustom;
	}

	public void setUserCustom(UserCustom userCustom) {
		this.userCustom = userCustom;
	}
	
	
}
```

```java
package tech.jiangtao.mybatis.pojo;

public class UserCustom extends User {

	public int age;
}
```

```xml
	<!-- 自定义包装类型进行查询 -->
    <select id="findUserByCustom" parameterType="tech.jiangtao.mybatis.pojo.UserQuery" resultType="tech.jiangtao.mybatis.pojo.User">
		select * from user where username like '%${userCustom.username}%'
	</select>
```

## ResultMap

- 使用ResultType，属性名和字段名必须相同，如果sql语句使用别名，查询出来条数正确，但是没有值。无法映射pojo值。这个问题就可以用ResultMap将字段名和属性名做一个映射配置。

- ```xml
  	<!-- 使用resultMap做一个结果映射 -->
  	<!-- 首先需要定义一个映射的resultMap -->
  	<!-- 如果resultmap在别的地方配置，可以使用命名空间.resultmap进行配置 -->
  	<select id="findUserListByResultMap" parameterType="java.lang.String" resultMap="userListByResultMap">
  		select id _id ,username _username, birthday _birthday ,address _address from user where username like '%${value}%'
  	</select>
  ```

- ```xml
  	<resultMap type="user" id="userListByResultMap">
  		<!-- id _id username _username birthday _birthday address _address -->
  		<id column="_id" property="id"/>
  		<result column="_username" property="username"/>
  		<result column="_birthday" property="birthday"/>
  		<result column="_address" property="address"/>
  	</resultMap>
  ```

  ​

## 动态sql

- Mybatis重点是对sql的灵活解析和处理
- 将自定义查询条件查询用户列表和查询用户列表总记录数改为动态sql