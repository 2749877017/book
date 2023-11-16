# oracle数据泵导入导出

## 一、登录oracle

打开DOS命令行界面，使用system用户登录oracle，格式：**sqlplus 用户名/密码@实例名**（或者使用plsql、sqlyog等工具登录）。

```shell
sqlplus system/000000@orcl 
```

## 二、 创建逻辑目录

创建备份逻辑目录，此目录不是真实的目录（单引号里面的内容是备份的目录，可以先查看一下所有的目录：select * from dba_directories;）

```sql
create or replace directory data as 'D:\app\shuhao\oradata\orcl';
```

## 三、 授予SYS.UTL_FILE包的执行权限

```sql
GRANT EXECUTE ON SYS.UTL_FILE TO <用户名>;
```

## 四、 授予读写权限

```sql
grant read, write on directory <目录名> to <用户名>;
```

## 五、导出数据库

exit退出oracle界面，在dom中进行导出

```shell
expdp jeecg_test/000000@orcl directory=data dumpfile=JEECG_20180226.DMP logfile=jeecg.log schemas=jeecg_test
```

## 六、导入数据库

使用system登录oracle

```shell
sqlplus system/orcl@orcl 
```

创建逻辑目录

```sql
create or replace directory data as 'D:\app\shuhao\oradata\orcl';
```

给目标用户授权

```sql
grant read,write on directory <目录名> to <用户名>;
```

创建真实目录，存放备份文件

DOS命令行执行下列命令

```shell
impdp jeecg_test/000000@orcl directory=data dumpfile=JEECG_20180226.DMP logfile=jeecg.log remap_schema =JEECG_TEST:JEECG_TEST
```

**注：remap_schema=JEECG_TEST:JEECG_TEST表示把左边的JEECG_TEST用户的数据，导入到右边的JEECG_TEST用户里面**
