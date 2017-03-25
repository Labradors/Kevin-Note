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

- ```xml
  <!-- 动态sql -->
  	<select id="dynamicFindUserByCustom" parameterType="tech.jiangtao.mybatis.pojo.UserQuery" resultType="tech.jiangtao.mybatis.pojo.User">
  		select * from user where
  		<where>
  			<if test="userCustom!=null">
  				<if test="userCustom.username!=null and userCustom.username!=''">
  					and username like '%${userCustom.username}%'
  				</if>
  				<if test="userCustom.sex!=null and userCustom.sex!=''">
  					and sex=#{userCustom.sex}
  				</if>
  			</if>
  		</where>
  	</select>
  ```

### sql片段

1. 可以将通用的sql语句抽取出来。相同业务抽离更好
2. 建议对单表的查询条件进行抽取

```xml
	<sql id="sql_query_user">
		<if test="userCustom!=null">
				<if test="userCustom.username!=null and userCustom.username!=''">
					and username like '%${userCustom.username}%'
				</if>
				<if test="userCustom.sex!=null and userCustom.sex!=''">
					and sex=#{userCustom.sex}
				</if>
			</if>
	</sql>
```

```xml
	<!-- 动态sql,根据sql片段抽离 -->
	<select id="dynamicFindUserByCustom1" parameterType="tech.jiangtao.mybatis.pojo.UserQuery" resultType="tech.jiangtao.mybatis.pojo.User">
		select * from user where
		<where>
			<!-- 引用的sql片段，如果片段不在同一个文件，前面加入namespace -->
			<include refid="sql_query_user"></include>
		</where>
	</select>
```

### foreach

1. 在statement中通过foreach遍历parameterType中的集合类型
2. 根据多个用户id查询用户信息

```xml
<!-- 解决传入集合，进行遍历取出值进行动态sql -->
				<!-- SELECT id, username,birthday,address FROM mybatis.user where username like '%张三%' and id in(22,24,25); -->
				<!-- 
					open: 开始循环要拼接的串
					close: 最后要拼接的串
					item：每一项的值
					separator： 分隔符
				 -->
				 <if test="userCustom.ids!=null">
					<foreach collection="userCustom.ids" open="and id in(" close=")" item="id" separator="," >
						#{id}
					</foreach>
				</if>
```

## ResultMap高级映射

### 如何分析一个陌生的数据模型

1. 了解表记录了什么信息。
2. 学习单表重要字段的意义（优先了解不能为空的字段，通过外键确定关系）
3. 学习表与表之间的关系（一对一，一对多，多对多，建立在业务之上的关系）

#### 使用ResultType来一对一查询多表

```xml
<!-- 查询订单关联用户信息 -->
	<!-- 使用ResultType进行一对一查询输出 -->
	<select id="findOrdersUserList" resultType="ordersCustom">
		SELECT orders.*,user.username,user.sex FROM orders,USER WHERE orders.user_id = user.id
	</select>
```

#### ResultMap实现一对一查询

直接在主pojo中添加你需要映射的别的pojo类型

添加pojo到主标

```java
package tech.jiangtao.mybatis.pojo;

import java.util.Date;

/**
 * 订单
 * @author kevin
 *
 */
public class Orders {
	private int id;
	private int user_id;
	private String number;
	private Date createtime;
	private String note;
	
	// 使用ResultMap来映射一对一的查询
	private User user;
	
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public int getUser_id() {
		return user_id;
	}
	public void setUser_id(int user_id) {
		this.user_id = user_id;
	}
	public String getNumber() {
		return number;
	}
	public void setNumber(String number) {
		this.number = number;
	}
	public Date getCreatetime() {
		return createtime;
	}
	public void setCreatetime(Date createtime) {
		this.createtime = createtime;
	}
	public String getNote() {
		return note;
	}
	public void setNote(String note) {
		this.note = note;
	}
	public User getUser() {
		return user;
	}
	public void setUser(User user) {
		this.user = user;
	}
}
```

配置ResultMap

```xml
<!-- 查询订单关联用户信息 -->
	<!-- 使用ResultMap进行一对一查询输出 -->
	<resultMap type="orders" id="userListResultMap">
		<id column="id" property="id"/>
		<result column="user_id" property="user_id"/>
		<result column="number" property="number"/>
		<result column="createtime" property="createtime"/>
		<result column="note" property="note"/>
		<!-- 关联信息的映射 用于对关联信息映射到单个的pojo映射-->
			<association property="user" javaType="user">
				<id column="user_id" property="id"/>
				<result column="username" property="username"/>
				<result column="sex" property="sex"/>
			</association>
	</resultMap>
	<select id="findOrdersUserListResultMap" resultMap="userListResultMap">
		SELECT orders.*,user.username,user.sex FROM orders,USER WHERE orders.user_id = user.id
	</select>
```

#### 两者哪个更简单

 ResultType:字段属性对应,这种方法相对较简单，所以应用广泛

ResultMap: 需要配置association，应用有点点复杂，如果要实现延迟加载，就只能用ResultMap来实现，如果方便对关联信息进行解析。则使用更加好。



#### ResultMap实现一对多查询

```xml
	<!-- 查询订单-订单详情-关联用户信息-商品信息 -->
	<!-- 使用ResultMap进行一对多查询输出 -->
	<!-- 用户信息 -->
	<!-- 订单信息 -->
	<!-- 订单详细信息 -->
	<!-- 商品信息 -->
	<resultMap type="user" id="userOrderAndOrdersDetailAndItemsListResultMap">
			<id column="user_id" property="id"/>
			<result column="username" property="username"/>
			<result column="birthday" property="birthday"/>
			<result column="sex" property="sex"/>
			<result column="address" property="address"/>
			<!-- 订单信息 -->
				<collection property="ordersList" ofType="orders">
					<id column="id" property="id"/>
					<result column="number" property="number"/>
					<result column="createtime" property="createtime"/>
					<result column="note" property="note"/>
					<!-- 订单明细 -->
					<collection property="orderdetails" ofType="tech.jiangtao.mybatis.pojo.Orderdetail">
						<id column="orders_id" property="id"/>
						<result column="items_id" property="items_id"/>
						<result column="items_num" property="items_num"/>
						<!-- 商品信息 -->
						<association property="items" javaType="items">
							<id column="items_id" property="id"/>
							<result column="name" property="name"/>
							<result column="price" property="price"/>
							<result column="detail" property="detail"/>
							<result column="pic" property="pic"/>
						</association>
					</collection>
				</collection>
	</resultMap>
	<select id="findUserOrderAndOrdersDetailAndItemsListResultMap" resultMap="userOrderAndOrdersDetailAndItemsListResultMap">
				SELECT USER.*,
				        orders.number,
				        orders.createtime,
				        orders.note,
				        orderdetail.orders_id,
				        orderdetail.items_id,
				        orderdetail.items_num,
				        items.name,
				        items.price,
				        items.detail,
				        items.pic
				FROM USER,orders,orderdetail,items
				WHERE user.id = orders.user_id
				        AND orderdetail.orders_id=orders.id
				        AND items.id = orderdetail.items_id
	</select>
```

#### 延迟加载

在进行数据库查询时，为了提高数据库的性能，尽量使用单表查询。

如果一开始查询能够满足需求，那就单表。当需要关联信息时，再进行关联多表查询。这就叫延迟加载。延迟加载是为了提高数据库性能。

配置延迟加载

```xml
<settings>
	<!-- 开启延迟加载 -->
		<setting name="lazyLoadingEnabled" value="true"/>
		<setting name="aggressiveLazyLoading" value="false"/>
	</settings>
```

```xml
	<!-- 延迟加载 只查询订单，对用户信息进行延迟加载 -->
	<resultMap type="orders" id="ordersCustomLazyLoading">
		<id column="id" property="id"/>
		<result column="user_id" property="user_id"/>
		<result column="number" property="number"/>
		<result column="createtime" property="createtime"/>
		<result column="note" property="note"/>
		<!-- 关联信息的映射 用于对关联信息映射到单个的pojo映射-->
			<association property="user" javaType="user" select="tech.jiangtao.mybatis.mapper.UserMapper.findUserById" column="user_id">
			</association>
	</resultMap>
	<select id="findOrdersUserListLazyLoading" resultMap="ordersCustomLazyLoading">
		SELECT orders.* FROM orders
	</select>
```



## 查询缓存

### 持久层缓存

#### 一级缓存

- myabtis一级缓存是sqlsession级别的。
- select会缓存一级缓存，但是如果有iud，则会删除所有缓存。
- mybatis不需要配置一级缓存。mybatis和spring整合后没有一级缓存。



#### 二级缓存

- mybatis二级缓存是mapper级别的缓存。指的是同一个namespace的缓存。
- mybatis默认没有开启二级缓存。

开启二级缓存:

```xml
<setting name="cacheEnabled" value="true"/>
```

```xml
在mapper中添加<cache/>
pojo实现java.io.Serializable
```

```java
@Test
	public void testCache2() {
		SqlSession session = sessionFactory.openSession();
		SqlSession session1 = sessionFactory.openSession();
		UserMapper mapper = session.getMapper(UserMapper.class);
		UserMapper mapper1 = session1.getMapper(UserMapper.class);
		try {
			System.out.println(mapper.findUserById(1).toString());
			session.close();
			System.out.println(mapper1.findUserById(1).toString());
			session1.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
```



#### 关闭二级缓存

```xml
<!-- 根据用户id，查询用户信息，只能是单条数据 -->
	<select id="findUserById" parameterType="int" resultType="tech.jiangtao.mybatis.pojo.User" useCache="false">
		select * from user where id= #{id}
	</select>
```

#### 刷新缓存（全部清空）

```xml
	<!-- 向用户表插入一条记录 -->
	<insert id="insertUser" parameterType="tech.jiangtao.mybatis.pojo.User" flushCache="true">
		<selectKey keyProperty="id" order="AFTER" resultType="int">
			select LAST_INSERT_ID()
		</selectKey>
		insert into user(username,sex,birthday,address) values(#{username},#{sex},#{birthday},#{address});
	</insert>
```

## Mybatis和ehcache整合

通过实现Mybatis的cache接口，将缓存数据托管给别的框架。

添加配置文件ehcache.xml

```xml
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
	<!--diskStore：缓存数据持久化的目录 地址  -->
	<diskStore path="./" />
	<defaultCache 
		maxElementsInMemory="1000" 
		maxElementsOnDisk="10000000"
		eternal="false" 
		overflowToDisk="false" 
		diskPersistent="true"
		timeToIdleSeconds="120"
		timeToLiveSeconds="120" 
		diskExpiryThreadIntervalSeconds="120"
		memoryStoreEvictionPolicy="LRU">
	</defaultCache>
</ehcache>
```

在mapper中添加缓存

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache">
		<!-- 属性设置 -->
		<!-- <property name="" value=""/> -->
	</cache>
```

#### Mybatis缓存的应用场景和局限性

- 细粒度缓存实现不好。
- 使用在查询较多，更改较少的数据进行缓存

## Mybatis和Spring的整合

一波波的整合

- 配置文件:db.properties

```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url1=jdbc:mysql://localhost:3306/mybatis
jdbc.url2=jdbc:mysql://139.59.30.244:3306/mybatis
jdbc.username1=root
jdbc.password1=123456
jdbc.username2=root
jdbc.password2=654321
```

- 配置文件: log4j.properties

```xml
# Root logger option
log4j.rootLogger=DEBUG, stdout, file
log4j.logger.org.hibernate=info

# Redirect log messages to console
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Redirect log messages to a log file, support file rolling.
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./common.log
log4j.appender.file.MaxFileSize=5MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
```

- 配置文件:SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<typeAliases>
		<typeAlias type="tech.jiangtao.mybatis.pojo.User" alias="user"/>
		<!-- 批量别名 -->
		<package name="tech.jiangtao.mybatis.pojo"/>
	</typeAliases>
	<mappers>
		 <mapper resource="sqlmap/user.xml"/>
		 <package name="tech.jiangtao.mybatis.mapper"/>
	</mappers>
	
</configuration>
```

- 配置文件: applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
		http://www.springframework.org/schema/mvc 
		http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd 
		http://www.springframework.org/schema/context 
		http://www.springframework.org/schema/context/spring-context-3.2.xsd 
		http://www.springframework.org/schema/aop 
		http://www.springframework.org/schema/aop/spring-aop-3.2.xsd 
		http://www.springframework.org/schema/tx 
		http://www.springframework.org/schema/tx/spring-tx-3.2.xsd ">
		
	<!-- 加载配置文件 -->
	<context:property-placeholder location="classpath:db.properties" />
	<!-- 数据库连接池 -->
	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName" value="${jdbc.driver}" />
		<property name="url" value="${jdbc.url1}" />
		<property name="username" value="${jdbc.username1}" />
		<property name="password" value="${jdbc.password1}" />
		<property name="maxActive" value="10" />
		<property name="maxIdle" value="5" />
	</bean>
		<!-- SqlsessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 数据源 -->
		<property name="dataSource" ref="dataSource"/>
		<!-- mybatis配置文件 -->
		<property name="configLocation" value="classpath:SqlMapConfig.xml"/>
	</bean>
	
	<!-- 配置dao -->
	<bean id="userDao" class="tech.jiangtao.mybatis.dao.UserDaoImpl">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"/>
	</bean>
	
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="tech.jiangtao.mybatis.mapper"/>
		<!-- 使用sqlSessionFactoryBeanName -->
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>	
	</bean> 
</beans>
```

没什么可配置的.



## Mybatis逆向工程

逆向工程配置:

