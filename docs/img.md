# 图片的上传与加载

## 一.进行环境搭建

### 1.进行maven项目webapp的项目创建

### 2.创建启动类

```java
package com.neuedu.boot;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class);
    }
}
```

### 3.引入依赖

```xml
<dependencies>
  <dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>3.15</version>
  </dependency>
  <dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml-schemas</artifactId>
    <version>3.15</version>
  </dependency>
  <dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>3.15</version>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
  </dependency>
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
  </dependency>

  <dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.8.1</version>
  </dependency>

  <dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.2</version>
  </dependency>

  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
  </dependency>


  <dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.3.2</version>
    <scope>test</scope>
  </dependency>


  <dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
    <version>2.3.31</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.4.0</version>
  </dependency>
  <!-- security -->
  <dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-core</artifactId>
  </dependency>
  <dependency>
    <groupId>com.tencentcloudapi</groupId>
    <artifactId>tencentcloud-sdk-java</artifactId>
    <version>3.1.87</version>
  </dependency>
  <dependency>
    <groupId>com.github.qcloudsms</groupId>
    <artifactId>qcloudsms</artifactId>
    <version>1.0.6</version>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
  </dependency>
  <dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.3.2</version>
    <scope>compile</scope>
  </dependency>

</dependencies>
```

### 4.添加yml文件

在resources中进行配置文件的创建，文件名为application.yml，此文件名不能随意更改

```yml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/imgupload?useSSL=false&characterEncoding=utf8
    driver-class-name: com.mysql.jdbc.Driver
    username: root
    password: root
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
  mvc:
    throw-exception-if-no-handler-found: true
  web:
    resources:
      add-mappings: false
mybatis:
    typeAliasesPackage: com.neuedu.entity
    mapper-locations: classpath*:com/neuedu/springboot/mapper/*.xml
```

至此环境搭建完成

## 二.使用mybatisplus进行三层的创建

```java
import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.core.toolkit.StringPool;
import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;


import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.util.logging.Logger;

// 演示例子，执行 main 方法控制台输入模块表名回车自动生成对应项目目录中
public class CodeGenerator {



    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
//        String projectPath = System.getProperty("user.dir");
        final String projectPath ="D:\\Program Files\\IDEAproject\\img";

        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("647");
        gc.setOpen(false);
        // gc.setSwagger2(true); 实体属性 Swagger2 注解
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/imgupload?useUnicode=true&useSSL=false&characterEncoding=utf8");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("root");
        mpg.setDataSource(dsc);

        // 包配置
        PackageConfig pc = new PackageConfig();
        final String pack = "com.neuedu.boot";
//        pc.setModuleName(scanner("模块名"));
        pc.setParent(pack);
        mpg.setPackageInfo(pc);

        // 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };

//         如果模板引擎是 freemarker
        String templatePath = "/templates/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        // String templatePath = "/templates/mapper.xml.vm";

        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
                return projectPath + "/src/main/resources/com/neuedu/boot/mapper/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        /*
        cfg.setFileCreate(new IFileCreate() {
            @Override
            public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {
                // 判断自定义文件夹是否需要创建
                checkDir("调用默认方法创建的目录，自定义目录用");
                if (fileType == FileType.MAPPER) {
                    // 已经生成 mapper 文件判断存在，不想重新生成返回 false
                    return !new File(filePath).exists();
                }
                // 允许生成模板文件
                return true;
            }
        });
        */
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

        // 配置模板
        TemplateConfig templateConfig = new TemplateConfig();

        // 配置自定义输出模板
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
        // templateConfig.setEntity("templates/entity2.java");
        // templateConfig.setService();
        // templateConfig.setController();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
//        strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");
        strategy.setEntityLombokModel(true);
//        strategy.setRestControllerStyle(true);
        // 公共父类
//        strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
        // 写于父类中的公共字段
//        strategy.setSuperEntityColumns("id");
//        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));  //默认生成所有表的
        strategy.setControllerMappingHyphenStyle(true);
//        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }

}
```

## 三.编写Controller层代码

```java
package com.neuedu.boot.controller;


import com.neuedu.boot.entity.Image;
import com.neuedu.boot.service.IImageService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import org.springframework.stereotype.Controller;
import org.springframework.web.multipart.MultipartFile;
import sun.misc.BASE64Encoder;

import javax.annotation.Resource;
import javax.servlet.http.HttpServletResponse;

/**
 * <p>
 *  前端控制器
 * </p>
 *
 * @author 647
 * @since 2021-06-10
 */
@RestController
@CrossOrigin
public class ImageController {
    @Resource
    private IImageService imageService;

    @PostMapping("/add/image")
    public String addImage(@RequestParam("file") MultipartFile file, @RequestParam("id") Integer id) throws Exception{
        if(!file.isEmpty()){
            //new BASE64Encoder加密算法类
            BASE64Encoder encoder = new BASE64Encoder();
            //将图片的字节数组进行加密，生成加密的字符串
            String imageby = encoder.encode(file.getBytes());
            Image image = new Image();
            image.setId(id);

            image.setImage(imageby);
            imageService.addImage(image);
        }
        return "ok";
    }

    @GetMapping("/get/image")
    public void getImage(@RequestParam("id") Integer id, HttpServletResponse response) throws Exception{
        imageService.getImage(id,response);
    }
}
```

## 四.编写实体类

```java
package com.neuedu.boot.entity;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import java.sql.Blob;
import java.io.Serializable;
import lombok.Data;
import lombok.EqualsAndHashCode;

/**
 * <p>
 * 
 * </p>
 *
 * @author 647
 * @since 2021-06-10
 */
@Data
@EqualsAndHashCode(callSuper = false)
public class Image implements Serializable {

    private static final long serialVersionUID = 1L;

    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;

    private Object image;


}
```

## 五.编写Mapper层代码

```java
package com.neuedu.boot.mapper;

import com.neuedu.boot.entity.Image;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;

/**
 * <p>
 *  Mapper 接口
 * </p>
 *
 * @author 647
 * @since 2021-06-10
 */
@Mapper
public interface ImageMapper extends BaseMapper<Image> {
    void insertImage(Image image);

    Image selectImageById(Integer id);
}
```

## 六.编写Service层接口

```java
package com.neuedu.boot.service;

import com.neuedu.boot.entity.Image;
import com.baomidou.mybatisplus.extension.service.IService;

import javax.servlet.http.HttpServletResponse;

/**
 * <p>
 *  服务类
 * </p>
 *
 * @author 647
 * @since 2021-06-10
 */
public interface IImageService extends IService<Image> {
    String addImage(Image image);
    String getImage(Integer id, HttpServletResponse response);
}
```

## 七.编写Service层实现类

```java
package com.neuedu.boot.service.impl;

import com.neuedu.boot.entity.Image;
import com.neuedu.boot.mapper.ImageMapper;
import com.neuedu.boot.service.IImageService;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import sun.misc.BASE64Decoder;

import javax.annotation.Resource;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletResponse;

/**
 * <p>
 *  服务实现类
 * </p>
 *
 * @author 647
 * @since 2021-06-10
 */
@Service
public class ImageServiceImpl extends ServiceImpl<ImageMapper, Image> implements IImageService {
    @Resource
    public ImageMapper imageMapper;

    public String addImage(Image image){
        imageMapper.insertImage(image);
        return "ok";
    }

    public String getImage(Integer id, HttpServletResponse response){
        try {
            Image image = imageMapper.selectImageById(id);
            //此处取出来的是图片加密后的字节数组
            byte[] imageby = (byte[])image.getImage();
            //将加密字节数组构造成字符串
            String value = new String(imageby,"UTF-8");
            //new BASE64Decoder解密算法类
            BASE64Decoder decoder = new BASE64Decoder();
            //将字符串解密后生成字节数组
            byte[] bytes = decoder.decodeBuffer(value);
            for(int i=0;i<bytes.length;i++){
                if(bytes[i]<0){
                    bytes[i]+=256;
                }
            }
            response.setContentType("image/jpeg");
            ServletOutputStream out = response.getOutputStream();
            out.write(bytes);
            out.flush();
            out.close();
        }catch (Exception e){
            e.printStackTrace();
        }

        return "ok";
    }
}
```

## 八.编写resources中的Mapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.neuedu.boot.mapper.ImageMapper">


    <resultMap id="TestResultMap" type="com.neuedu.boot.entity.Image">
        <id column="id" jdbcType="INTEGER" property="id" />
        <result column="image" jdbcType="BLOB" property="image" />
    </resultMap>

    <insert id ="insertImage" parameterType="com.neuedu.boot.entity.Image">
        REPLACE  into image(id,image) values(#{id},#{image})
    </insert>

    <select id="selectImageById" parameterType="Integer" resultType="com.neuedu.boot.entity.Image">
        select * from image where id = #{id}
    </select>
</mapper>
```

## 九.进行测试

### 1.启动类执行

### 2.使用HbuilderX新建test1.html

```html
<!DOCTYPE>
<html>
    <head></head>
    <body>
        <form action="http://localhost:8080/add/image" method="post" enctype="multipart/form-data">
            <input type="text" name="id" /><br/>
            <input type="file" name="file" /><br/>
            <input type="submit" name="" id="" value="提交" />
        </form>
    </body>
</html>
```

### 3.使用HbuilderX新建test2.html

```html
<!DOCTYPE>
<html>
    <head></head>
    <body>
        <image src="http://localhost:8080/get/image?id=3">
    </body>
</html>
```
