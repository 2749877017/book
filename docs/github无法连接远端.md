# github无法连接远端

1. 如果未生成ssh key，使用：   

```git
ssh-keygen -t rsa -C "github账号"
```

生成新的rsa密钥即可。

如果是ssh key 不匹配，此时需要先将本地生成的 id_rsa以及id_rsa.pub这两个文件【一般在用户名下的.ssh文件夹下】删除掉，然后再使用上述指令生成新的rsa密钥。

输入完后一路回车直到结束

2.将新ssh key添加到ssh-agent中

```git
ssh-add ~/.ssh/id_rsa
```

中途可能出现报错，使用以下代码

```git
eval "$(ssh-agent -s)"
```

3.找到生成的ssh文件将密钥复制到github中
