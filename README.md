# postgresql

<br/>

```shell
[root@zhangyz src]# wget https://ftp.postgresql.org/pub/source/v10.3/postgresql-10.3.tar.gz
[root@zhangyz src]# tar -xf postgresql-10.3.tar.gz 
[root@zhangyz src]# cd postgresql-10.3
[root@zhangyz postgresql-10.3]# ./configure --prefix=/usr/local/postgresql
[root@zhangyz postgresql-10.3]# make && make install
```

#### 添加用户和用户组

```shell
[root@zhangyz ~]# useradd postgres
[root@zhangyz ~]# groupadd postgres
```

#### 生成数据库文件目录

```shell
[root@zhangyz ~]# mkdir /usr/local/postgresql/data
```

#### 更改用户文件夹访问权限

```shell
[root@zhangyz ~]# chown postgres /usr/local/postgresql/data/
[root@zhangyz ~]# chgrp postgres /usr/local/postgresql/data/
```
