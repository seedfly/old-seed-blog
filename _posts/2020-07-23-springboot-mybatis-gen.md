---
title: 用mybatis generator在springboot中快速搭建Mybaits框架（图文教程）
author: Old Seed
date: 2019-09-22 16:34:00 +0800
categories: [开发,Java]
tags: [mysql,mybatis,java,springboot]
toc: true

---



# 系统环境

- Intellj 2018
- Java 1.8
- Springboot 2.1.1 Release
- Maven 3.3.9
- mybatis-generator-maven-plugin 1.3.7

# 构建步骤

## 1. 初始化工程

![这里用springboot自带的模板初始化新的工程](/assets/img/20190107163251717.png)
这里用springboot自带的模板初始化新的工程

![填入maven的工程信息](/assets/img/20190107163535833.png)
填入maven的工程信息

![在这里插入图片描述](/assets/img/2019010716364036.png)
勾选springboot版本，并且选择JDBC和MyBatis。 下一步
![点击finish，创建工程](/assets/img/20190107163825626.png)
点击finish，创建工程

![在这里插入图片描述](/assets/img/20190107164441961.png)
这是现在的目录结构，pom文件里写入了基本的依赖。但是还不够，还需要我们进行配置

## 2. pom文件配置
1. 加入mysql jdbc驱动依赖
```xml
 		<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.25</version>
        </dependency>
```

2. 配置mybatis-generator-maven-plugin插件
```xml
			<plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.7</version>
                <executions>
                    <execution>
                        <id>Generate MyBatis Artifacts</id>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                    </execution>
                </executions>
                <dependencies>
                    <!-- mysql -->
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.25</version>
                    </dependency>
                </dependencies>
            </plugin>
```
## 3. 创建generatorConfig.xml文件

在resource目录下创建generatorConfig.xml， 这个文件是mybatis-generator-maven-plugin所需要的。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="application.properties"/>

    <context id="test" targetRuntime="MyBatis3">
        <plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"></plugin>
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"></plugin>
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin"></plugin>
        <commentGenerator>
            <!-- 这个元素用来去除指定生成的注释中是否包含生成的日期 false:表示保护 -->
            <!-- 如果生成日期，会造成即使修改一个字段，整个实体类所有属性都会发生变化，不利于版本控制，所以设置为true -->
            <property name="suppressDate" value="true" />
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="false" />
        </commentGenerator>
        <!--数据库链接URL，用户名、密码 -->
        <jdbcConnection driverClass="${jdbc.driverClassName}" connectionURL="${jdbc.url}" userId="${jdbc.username}" password="${jdbc.password}">
            <!-- 这里面可以设置property属性，每一个property属性都设置到配置的Driver上 -->
        </jdbcConnection>
        <javaTypeResolver>
            <!-- This property is used to specify whether MyBatis Generator should
                force the use of java.math.BigDecimal for DECIMAL and NUMERIC fields, -->
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>
        <!-- 生成模型的包名和位置 -->
        <javaModelGenerator targetPackage="com.chaincomp.quickmybaits.model" targetProject="src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- 生成映射文件的包名和位置 -->
        <sqlMapGenerator targetPackage="com.chaincomp.quickmybaits.mapper" targetProject="src\main\java">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- 生成DAO的包名和位置 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.chaincomp.quickmybaits.mapper" targetProject="src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <!-- 要生成哪些表 -->
        <table tableName="testuser" domainObjectName="SysUser" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" />

    </context>
</generatorConfiguration>
```

## 4. 添加数据库连接信息
为了便于管理，数据的连接信息写在了application.properties文件里面。
```
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/yourMysqlDbName
jdbc.username=root
jdbc.password=root
```

## 5. 运行maven插件，构建工程结构
![在这里插入图片描述](/assets/img/20190107165924558.png)

这里用了inteljj内建的maven, 运行这个mybatis-generator:generate，或者也可以在命令行中运行
```bash
mvn mybatis-generator:generate
```

![在这里插入图片描述](/assets/img/20190107170400222.png)
这时候的目录结构是这样的。model下面的SysUser是自动通过mysql中的表结构生成的。这个功能很有用，因为很多时候是先有表结构再开始写代码的，这个功能可以省去很多写model的工作。

## 6. 运行mybatis
现在只是有了基本的目录结构，mybatis还不能跑起来。需要我们建立mybaitis的配置文件，并编写相应的代码。


### 创建mybaits-config.xml
在resources目录下创建mybaitis-config.xml。文件内容如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration
                PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

<properties resource="application.properties"/>

<environments default="test">
    <environment id="test">
        <transactionManager type="JDBC" />
        <dataSource type="POOLED"> <!-- 表示用连接池 -->
            <property name="driver" value="${jdbc.driverClassName}" />
            <property name="url" value="${jdbc.url}" />
            <property name="username" value="${jdbc.username}" />
            <property name="password" value="${jdbc.password}" />
        </dataSource>
    </environment>
</environments>

<mappers>
    <mapper class="com.chaincomp.quickmybaits.mapper.SysUserMapper"/>
</mappers>

</configuration>
```

### 创建mybaits工具类 MybatisUtil 
```java
package com.chaincomp.quickmybaits;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtil {
    public static SqlSession openSqlSession(){
        String sqlConfigXML = "mybaits-config.xml";
        InputStream in = null;
        try {
            in = Resources.getResourceAsStream(sqlConfigXML);
        } catch (IOException e) {
            e.printStackTrace();
        }
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(in,"test");
        return build.openSession();
    }
}
```

### 创建运行类 Test.java
```java
package com.chaincomp.quickmybaits;

import com.chaincomp.quickmybaits.mapper.SysUserMapper;
import com.chaincomp.quickmybaits.model.SysUser;
import org.apache.ibatis.session.SqlSession;

public class Test {
    public static void main(String args[]){
        System.out.println("start");
        run();
    }

    private static void run() {
        try {
            // 连接数据库，并得到数据库操作对象
            SqlSession openSession = MybatisUtil.openSqlSession();

            SysUserMapper mapper=openSession.getMapper(SysUserMapper.class);

            // 得到mapper对象（该对象在mybatis-config.xml中要配置正确）
            // 然后调用方法执行封装的sql语句
            SysUser user=new SysUser();
            user.setAge(14);
            user.setGender("Male");
            user.setId(1);
            user.setName("Jack");
            mapper.insert(user);
            openSession.commit();
            // 最后关闭连接
            openSession.close();
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```

## 最后一步
运行这个带有main函数的Test类，发现报错了：
```
org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): com.chaincomp.quickmybaits.mapper.SysUserMapper.insert
	at org.apache.ibatis.binding.MapperMethod$SqlCommand.<init>(MapperMethod.java:227)
	at org.apache.ibatis.binding.MapperMethod.<init>(MapperMethod.java:49)
	at org.apache.ibatis.binding.MapperProxy.cachedMapperMethod(MapperProxy.java:65)
	at org.apache.ibatis.binding.MapperProxy.invoke(MapperProxy.java:58)
	at com.sun.proxy.$Proxy2.insert(Unknown Source)
	at com.chaincomp.quickmybaits.Test.selectMember(Test.java:27)
	at com.chaincomp.quickmybaits.Test.main(Test.java:10)
```

调研一下发现是因为mapper的xml资源文件没有被打包进去，所以在pom文件的build标签里还需要加入：

```xml
 		<resources>
            <resource>
                <directory>src/main/java</directory><!--所在的目录-->
                <includes><!--重要！！！！！目录下的.properties,.xml都会扫描到-->
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>

```

## 成功了
运行之后，在数据库中看到了用mybatis插入的记录，说明mybatis搭建完成。
![在这里插入图片描述](/assets/img/20190107173252151.png)

## 后续
搭建成功之后回头看，发现这个教程并没有用到springboot的特性。下一个教程将会研究怎么把springboot的特性跟mybaits结合起来。pom文件已经引入了mybatis-spring-boot-starter，只不过配置的方法不同而已。

