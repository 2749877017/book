# SpirngMVC笔记

[SSM流程图](https://gitee.com/Ama_deus/imgAll/raw/master/img/image-20210607141607376.png)

## 一.第一个SpringMVC程序

### 1.添加依赖（pom）

```xml
 <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.6</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
    <dependency>
      <groupId>taglibs</groupId>
      <artifactId>standard</artifactId>
      <version>1.1.2</version>

    </dependency>
    <!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-impl -->
    <dependency>
      <groupId>org.apache.taglibs</groupId>
      <artifactId>taglibs-standard-impl</artifactId>
      <version>1.2.5</version>
    </dependency>

  </dependencies>
```

### 2.配置前端控制器（WEB-INF/web.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">


  <!--1. 配置DispatcherServlet核心控制器  -->
  <!--前端控制器-->
<servlet>
  <servlet-name>springMVC</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

<!--自定义配置文件的位置，如果不写init-param，默认在WEB-INF目录下文件名为"前端控制器名-servlet.xml"-->
 <init-param>
   <param-name>contextConfigLocation</param-name>
   <param-value>classpath:springMVC-servlet.xml</param-value>
 </init-param>
  <!--优先级-->
  <load-on-startup>1</load-on-startup>
</servlet>


  <servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

### 3.添加配置文件

- 文件默认位置在WEB-INF下，文件名为 “前端控制器-servlet.xml“

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd  ">
<!--2.配置HandleMapping-->
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>

<!--3.配置HandleAdapter-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter" />

    <!--4.配置Handle-->
    <bean  name="/hello"    class="com.neuedu.HelloController" />

    <!--5.配置ViewResolver-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
        <property name="prefix" value="/WEB-INF/view/" />
        <property name="suffix" value=".jsp" />
<!--6.配置View-->
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />


    </bean>
</beans>
```

- 注解方式的配置文件,一般习惯写在resources目录下面

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd  ">

    <!--2.配置HandleMapping-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping" />
    <!--3.配置HandleAdapter-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
    <!--4.配置Handle-->
    <context:component-scan base-package="com.neuedu" />
    <!--5.配置ViewResolver-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/view/" />
        <property name="suffix" value=".jsp" />
        <!--6.配置View-->
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />

    </bean>
<!--静态资源处理-->
<mvc:default-servlet-handler />
    <!--静态资源处理-->
<mvc:resources mapping="/img/**" location="/WEB-INF/img/" />
</beans>
```

- 可以是用注解驱动来替代配置HandleMapping与HandleAdapter
  
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
  
  <!--2.配置注解驱动-->
      <mvc:annotation-driven/>
  
      <!--3.配置Handle-->
      <context:component-scan base-package="com.neuedu" />
      <!--4.配置ViewResolver-->
      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="prefix" value="/WEB-INF/view/" />
          <property name="suffix" value=".jsp" />
          <!--5.配置View-->
          <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
      </bean>
  <!--静态资源处理-->
  <mvc:default-servlet-handler />
      <!--静态资源处理-->
  <mvc:resources mapping="/img/**" location="/WEB-INF/img/" />
  ```

      </beans>

```
### 4.创建一个类用于测试

- 基于xml的配置测试

```java
package com.neuedu;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloController implements Controller {

  @Override
  public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse res) throws Exception {
      System.out.println("success");
     String name= req.getParameter("name");
     ModelAndView mav=new ModelAndView();
     mav.addObject("msg","Hello"+name);
     mav.setViewName("hello");



      return mav;
  }
}
```

- 基于注解的配置测试
  
  ```java
  package com.neuedu;
  ```

  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.servlet.ModelAndView;

  @Controller
  @RequestMapping("/auto")
  public class AutoController {
      @RequestMapping("/hello")
      public ModelAndView Hello(String name){
          ModelAndView mav=new ModelAndView();
          mav.addObject("msg",name);
          mav.setViewName("hello");

          return mav;
      }

  }

```
### 5.在WEB-INF的目录下创建一个jsp文件

```jsp
<%--
Created by IntelliJ IDEA.
User: lenovo
Date: 2021/6/7
Time: 14:26
To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
  <title>hello</title>
</head>
<body>
          msg:${msg}
</body>
</html>
```

### 6.最后的页面展示

[http://localhost:8080/mvc/hello](http://localhost:8080/mvc/hello)

- 基于xml配置的页面展示

[xml页面展示](https://gitee.com/Ama_deus/imgAll/raw/master/img/%E9%A1%B5%E9%9D%A2%E5%B1%95%E7%A4%BA.png)

[http://localhost:8080/mvc/auto/hello?name=aaaa](http://localhost:8080/mvc/auto/hello?name=aaaa)

- 基于注解配置的页面展示

[注解页面展示](https://gitee.com/Ama_deus/imgAll/raw/master/img/image-20210607180009517.png)

## 二.静态资源处理

### 1.解决访问不了静态资源

- 当前端控制器的url-pattern标签为/的时候，会拦截一切资源，包括图片、样式等静态资源，我们可以通过在spring的配置
  
  文件中加入以下语句来进行解决
  
  ```xml
  <!--静态资源处理-->
  <mvc:default-servlet-handler />
  ```

​       此方法只能访问webapp目录下的资源，访问不了WEB-INF里面的资源

- 当想要访问WEB-INF里面的静态资源时，可以在spring配置文件中加入以下语句,可添加多个
  
  ```xml
  <!--静态资源处理-->
  <mvc:resources mapping="/img/**" location="/WEB-INF/img/" />
  ```

## 三.访问jsp页面

### 1.直接访问jsp页面

- 默认不能直接访问WEB-INF里面的页面，一般是在Controller做地址转发，我们也可以在spring配置中加入以下，用于直接
  
  访问
  
  ```xml
  <!--直接访问jsp-->
  <mvc:view-controller path="/see" view-name="hello" />
  ```

## 四.关于Controller

### 1.方法的返回值

- ModelAndView表示返回的是数据模型和视图

- String表示返回的是视图
  
  ​    1.普通字符串------------表示视图名称
  
  ​    2."forward:url"---------内部跳转
  
  ​    3."redirect:url"---------重定向

- void表示把RequestMapping中的地址当做视图

- Object表示返回的是数据模型（一般为json数据）

### 2.SpringMVC注解

| 注解                | 解释                                         |
| ----------------- | ------------------------------------------ |
| @Controller       | 将类映射为Controller层，添加到ioc容器中                 |
| @RequestMapping   | 配置请求的映射路径，即URL                             |
| @RequestParam     | 表示参数来源于请求参数                                |
| @PathVariable     | 表示参数来源于URL                                 |
| @RequestHeader    | 表示参数来源于请求头                                 |
| @CookieValue      | 表示参数来源于Cookie                              |
| @RequestBody      | 表示参数来源于请求体                                 |
| @ModelAttribute   | 将请求数据转换为对象                                 |
| @Valid            | 后台校验                                       |
| @InitBinder       | 类型转换，注册属性编辑器                               |
| @ControllerAdvice | 统一异常处理，处理全局异常                              |
| @ExceptionHandle  | 异常处理器，处理特定异常的方法                            |
| @ResponseBody     | 结合返回值为Object的方法使用，返回json数据                 |
| @RestController   | 将类映射为Controller层,默认为所有方法都添加@ResponseBody注解 |

### 3.@RequestMapping

- 基本用法：可以注释在方法上，也可以注释在类上，表示层级关系
  
  当在return的路径上加  /  时，会在全包下寻找URL
  
  当不在return的路径上加  /  时，会在本类里寻找URL

- URL的多种写法
  
  1.Ant风格
  
  ```java
  @RequestMapping("/out/*")
  ```
  
   其中*表示单层目录，可以匹配任意字符，可以没有字符，但是需要加  /  ，在有字符的情况下在最后也可以加一个  /  ,（必须有一层目录）
  
  ```java
  @RequestMapping("/out/**")
  ```
  
  **表示0到多级目录，可以没有字符，也可以没有  /
  
  ```java
  @RequestMapping("/out/?")
  ```
  
  ？表示一个字符，必须有

​       

​        
