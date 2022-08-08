# mybatis的使用

## 一、搭建项目

### 1.使用maven的webapp框架搭建环境

### 2.引入依赖

```xml
<dependencies>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
  </dependency>

  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
  </dependency>
</dependencies>
```

## 二、使用编程工具生成代码

### 1、使用mybatis-generator生成mybatis代码

先修改generatorConfiguration.xml中的部分代码，使用run.txt中的命令在当前目录下运行cmd，生成代码在src文件夹中，

把文件对应的放入项目中

## 三、编写配置文件

### 1、在resources目录下创建jdbc.properties文件

```properties
username="root"
password="root"
url="jdbc:mysql://localhost:3306/upload"
driver="com.mysql.jdbc.Driver"
```

### 2、在resources目录下创建SqlMapConfig.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties"/>

    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="UNPOOLED">
                <property name="username"   value="root"/>
                <property name="password"   value="root"/>
                <property name="url"        value="jdbc:mysql://localhost:3306/upload"/>
                <property name="driver"     value="com.mysql.jdbc.Driver"/>
            </dataSource>
        </environment>
    </environments>



    <mappers>

        <mapper resource="com/neuedu/mapper/UploadFilesMapper.xml"/>
    </mappers>

</configuration>
```

## 四、编写测试类进行测试

### 1.使用sqlSessionFactory来进行测试

```java
package com.neuedu.mapper;

import com.neuedu.entity.UploadFiles;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Before;
import org.junit.Test;




import java.io.IOException;
import java.io.InputStream;

public class Testa {
SqlSessionFactory sqlSessionFactory =null;
    @Before
    public void setup() throws IOException {

        InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    }
    @Test
    public void test(){
        SqlSession session = sqlSessionFactory.openSession();
        UploadFilesMapper mapper = session.getMapper(UploadFilesMapper.class);
    UploadFiles uploadFiles=mapper.selectByPrimaryKey(1);
        System.out.println(uploadFiles);


    }
}
```
