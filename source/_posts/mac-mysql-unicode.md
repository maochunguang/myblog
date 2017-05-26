title: mac下mysql5.6字符集设置
date: 2016-05-28 23:10:37
tags: mysql
categories: 数据库
---
** mac下mysql5.6字符集设置：** <Excerpt in index | 首页摘要>
    在mac下设置mysql5.6字符集时踩过的坑，百分百保证有效
<!-- more -->
<The rest of contents | 余下全文>
## 为什么要设置字符集
1. 设置字符集主要是解决乱码问题，由于中文和英文编码不同导致，中文出现乱码，所以一般都设置为utf8格式
2. 不同的字符集和编码占用的字节不同，选择适合的编码会提高数据库性能

## mac下设置
- 在/etc/my.cnf文件进行设置，如果没有此文件可以从/usr/local/mysql/support-files/拷贝，命令如下
```
cd /usr/local/mysql/support-files
sudo cp my.cnf /etc/my.cnf
```
查看文件的读写权限，如果为644（rw- r-- r--）则改为(664) (rw- rw- r--)
如果改为(666)(rw- rw- rw-)则修改以后配置文件不会生效
```
sudo chmod 664 /etc/my.cnf
```

- my.cnf设置如下：
```
[client]
default-character-set=utf8
[mysqld]
collation-server = utf8_unicode_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
[mysql]
default-character-set=utf8
```

## 查看设置是否成功
在命令行输入mysql，如果提示没有命令的话，在bash或者zsh的文件里修改，我用的是zsh，设置~/.zshrc,
```
export MYSQL="/usr/local/mysql/bin/"
export PATH="/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:$MYSQL"
```
在命令行输入mysql,进入mysql命令行后，输入`status;`或者`show variables like '%char%';`
```
| character_set_client     | utf8                                                    |
| character_set_connection | utf8                                                    |
| character_set_database   | utf8                                                    |
| character_set_filesystem | binary                                                  |
| character_set_results    | utf8                                                    |
| character_set_server     | utf8                                                    |
| character_set_system     | utf8                                                    |
| character_sets_dir       | /usr/local/mysql-5.6.30-osx10.11-x86_64/share/charsets/
```
> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
