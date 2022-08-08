# 将jar转为maven

创建文件夹，在该文件夹下使用cmd窗口

```shell
mvn install:install-file -Dfile=commons-logging-1.1.1.jar -DgroupId=pay -DartifactId=commons-logging-1.1.1 -Dversion=1.0.0 -Dpackaging=jar
```

其中的参数配置以IDEA中的`<dependency>`为主
