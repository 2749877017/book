# spring配置redis注解缓存

## 一、**spring的主配置中声明注解缓存**

### **1.spring的主配置中声明注解缓存:<cache:annotation-driven cache-manager="redisCacheManager"/>**

**注意:此步骤必须做，必须声明采用的缓存管理器是自己配置的redisCacheManager，否则会报错。**

```xml
<cache:annotation-driven cache-manager="redisCacheManager"/>
```

## 二、maven的pom.xml文件导入jar包

### 1.pom文件中导入jar包

###### 注意:引入jackson是为了手动添加缓存

```xml
        <!-- jedis依赖 -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
            <version>1.8.4.RELEASE</version>
        </dependency>
        <!-- jackson -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.1.0</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.1.0</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.1.0</version>
        </dependency>
```

## 三、添加配置redis.properties

### 1.在resources资源文件夹中创建配置文件

```properties
#服务地址
redis.host=localhost
#端口
redis.port=6379
#密码，默认为空
redis.password=123456
# use dbIndex
redis.database=0
redis.maxActive=600
#连接池中的最大空闲连接
redis.maxIdle=300
#获取连接最大等待时间（毫秒）
redis.maxWait=3000
#在获取连接时，是否验证有效性
redis.testOnBorrow=true
#连接超时时间（单位为毫秒）
redis.longExpiration=86400
redis.shortExpiration=30
```

### 2.在主配置文件中，添加配置文件位置

```xml
  <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="locations">
            <list>
                <value>classpath:config/redis.properties</value>
            </list>
        </property>
    </bean>
```

## 四、使用spring管理bean

```xml
    <beans:bean id="jedisConfig" class="redis.clients.jedis.JedisPoolConfig">
        <beans:property name="maxTotal" value="${redis.maxActive}"/>
        <beans:property name="maxIdle" value="${redis.maxIdle}"/>
        <beans:property name="maxWaitMillis" value="${redis.maxWait}"/>
        <beans:property name="testOnBorrow" value="${redis.testOnBorrow}"/>
    </beans:bean>
    <!-- redis连接工厂 -->
    <beans:bean id="connectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <beans:property name="hostName" value="${redis.host}"/>
        <beans:property name="port" value="${redis.port}"/>
        <beans:property name="password" value="${redis.password}"/>
        <beans:property name="poolConfig" ref="jedisConfig"/>
    </beans:bean>
    <beans:bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <beans:property name="connectionFactory"   ref="connectionFactory" />
        <beans:property name="defaultSerializer">

            <bean class="org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer"/>
        </beans:property>
        <beans:property name="keySerializer">
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </beans:property>

        <beans:property name="hashKeySerializer">
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </beans:property>

    </beans:bean>



    <bean id="redisCacheManager" class="org.springframework.cache.support.SimpleCacheManager">
        <property name="caches">
            <set>
                <bean class="org.springframework.data.redis.cache.RedisCache">
                    <constructor-arg index="0" name="name" value="shortCache"/>
                    <constructor-arg index="1" name="prefix">
                        <null></null>
                    </constructor-arg>
                    <constructor-arg index="2" name="redisOperations" ref="redisTemplate"/>
                    <constructor-arg index="3" name="expiration" value="${redis.shortExpiration}"/>
                </bean>

                <bean class="org.springframework.data.redis.cache.RedisCache">
                    <constructor-arg index="0" name="name" value="longCache"/>
                    <constructor-arg index="1" name="prefix">
                        <null></null>
                    </constructor-arg>
                    <constructor-arg index="2" name="redisOperations" ref="redisTemplate"/>
                    <constructor-arg index="3" name="expiration" value="${redis.longExpiration}"/>
                </bean>
            </set>
        </property>
    </bean>

    <beans:bean id="workingKeyGenerator" class="org.springframework.cache.interceptor.SimpleKeyGenerator"/>
```

web.xml文件中指定主配置文件，例如指定applicationContext.xml为主配置文件

```xml
  <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
```

## 五、在你想缓存的方法上，加相关注解

　@CacheConfig  配置在类上，cacheNames即定义了本类中所有用到缓存的地方，都去找这个库。只要使用了这个注解，在方法上@Cacheable    @CachePut   @CacheEvict就可以不用写value去找具体库名了。【一般不怎么用】

　@Cacheable  配置在方法或类上，作用：本方法执行后，先去缓存看有没有数据，如果没有，从数据库中查找出来，给缓存中存一份，返回结果，下次本方法执行，在缓存未过期情况下，先在缓存中查找，有的话直接返回，没有的话从数据库查找

　@CachePut   类似于更新操作，即每次不管缓存中有没有结果，都从数据库查找结果，并将结果更新到缓存，并返回结果

　@CacheEvict 用来清除用在本方法或者类上的缓存数据（用在哪里清除用在哪里)
