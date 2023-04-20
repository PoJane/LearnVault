### 1. 项目创建
##### 1.1 基本依赖
- 语言：Java
- 类型：Maven
- JDK：17
- Spring Boot：3.0.5
- 依赖：Lombok，Spring Web，MyBatis FrameWork，MySQL Driver

##### 1.2 添加依赖
添加jdbc连接所需的包commons-logging, commons-pool2, commons-dbcp2，pom表述如下：
```xml
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->  
<dependency>  
    <groupId>org.apache.commons</groupId>  
    <artifactId>commons-dbcp2</artifactId>  
    <version>2.9.0</version>  
</dependency>  
  
<!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->  
<dependency>  
    <groupId>commons-logging</groupId>  
    <artifactId>commons-logging</artifactId>  
    <version>1.2</version>  
</dependency>  
  
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-pool2 -->  
<dependency>  
    <groupId>org.apache.commons</groupId>  
    <artifactId>commons-pool2</artifactId>  
    <version>2.11.1</version>  
</dependency>
```

### 2. 项目结构
##### 2.1 包
1. 项目的层次如下
|   包   |   说明   |
|:----|:---|
|config|存放配置类|
|controller|存放Controller类|
|domain|存放实体类|
|mapper|存放数据库操作类|
|service|存放业务逻辑类|
|utils|存放工具类

2. resources层
|目录|说明|
|:----|:---|
|com.xxx.xxx.mapper|存放mapper接口对应xml|
|config|存放配置文件|
注意存放mapper xml文件的资源文件夹完整限定名必须与mapper包名一致。这样在编译后，mapper接口与对应的xml才能在同一个文件夹下。（编译后的类可在target/classes/com/xxx/xxx/mapper文件夹下查看）

### 3. 配置
##### 3.1 配置application.properties
首先项目添加了数据库、MyBatis等依赖，为使项目初始化成功，需要添加数据库相关驱动。
- 在application.properties添加如下内容
在本项目的配置中，数据库是MySQL中的project_db，可根据自己的情况填写数据库、用户以及密码。
```properties
# port:8081  
server.port=8081  
# driver  
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  
# database  
spring.datasource.url=jdbc:mysql://localhost:3306/project_db?serverTimezone=UTC  
# Mysql user  
spring.datasource.username=root  
# Mysql password  
spring.datasource.password=root123456
```

##### 3.2 配置数据库设置
在resources/config下新建db.properties，在其中写数据库相关设置，方便在spring配置文件中使用。
```db.properties
jdbc.driver=com.mysql.jdbc.Driver  
jdbc.url=jdbc:mysql://localhost/project_db  
jdbc.username=root  
jdbc.password=root123456  
jdbc.maxTotal=30  
jdbc.maxIdle=10  
jdbc.initialSize=5
```

##### 3.2 spring配置
在resources/config下新建applicationContext.xml，在其中配置Spring的DataSource、事务、注解、MyBatis等。
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:aop="http://www.springframework.org/schema/aop"  
       xmlns:tx="http://www.springframework.org/schema/tx"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
       http://www.springframework.org/schema/beans/spring-beans.xsd       http://www.springframework.org/schema/aop       http://www.springframework.org/schema/aop/spring-aop.xsd       http://www.springframework.org/schema/tx       http://www.springframework.org/schema/tx/spring-tx.xsd       http://www.springframework.org/schema/context       http://www.springframework.org/schema/context/spring-context.xsd">  
  
    <!--读取db.properties-->  
    <context:property-placeholder location="classpath:config/db.properties" />  
  
    <!--配置数据源dataSource-->  
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">  
        <!--数据库驱动-->  
        <property name="driverClassName" value="${jdbc.driver}" />  
        <!--数据库连接url-->  
        <property name="url" value="${jdbc.url}" />  
        <!--数据库用户名-->  
        <property name="username" value="${jdbc.username}" />  
        <!--数据库密码-->  
        <property name="password" value="${jdbc.password}" />  
        <!--最大连接数-->  
        <property name="maxTotal" value="${jdbc.maxTotal}" />  
        <!--最大空闲连接-->  
        <property name="maxIdle" value="${jdbc.maxIdle}" />  
        <!--初始化连接数-->  
        <property name="initialSize" value="${jdbc.initialSize}" />  
    </bean>  
  
    <!--事务管理器-->  
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">  
        <!--注入数据源-->  
        <property name="dataSource" ref="dataSource" />  
    </bean>  
    <!--开启事务注解-->  
    <tx:annotation-driven transaction-manager="transactionManager" />  
  
    <!--配置MyBatis工厂SqlSessionFactory-->  
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">  
        <!--注入数据源-->  
        <property name="dataSource" ref="dataSource" />  
        <!--指定MyBatis核心配置文件路径-->  
        <property name="configLocation" value="classpath:config/mybatis-config.xml" />  
    </bean>  
    <!--配置mapper扫描器-->  
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">  
        <property name="basePackage" value="com.ling.project_backend.mapper" />  
    </bean>  
  
    <!--扫描Service-->  
    <context:component-scan base-package="com.ling.project_backend.service" />  
  
</beans>
```

##### 3.3 配置MyBatis
1. mybatis核心配置
在resources/config下新建mybatis-config.xml，配置MyBatis相关设置，由于在spring配置文件中已经配置了MyBatis以及mapper自动扫描器，在这个文件中无需在配置mapper的位置。为了方便使用，给实体层domain配置别名。
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <!--别名配置-->  
    <typeAliases>  
        <package name="com.ling.project_backend.domain"/>  
    </typeAliases>  
</configuration>
```

2. log4j.properties
配置好该文件，可以使MyBatis操作显示在控制台日志中，方便查看，也可以不配置。
```properties
# Global logging configuration  
log4j.rootLogger=ERROR, stdout  
# MyBatis logging configuration...  
log4j.logger.com.ling.project_backend=DEBUG  //将本项目设为DEBUG级别
# Console output...  
log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%
```

3. MyBatis配置文件、mapper映射文件、log4j大致结构都可在MyBatis官方文档中查看，具体可前往GitHub下载对应版本查看。