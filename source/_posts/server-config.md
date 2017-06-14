title: ubuntu服务器详细配置
date: 2016-11-28 20:36:03
tags: linux
categories: 开发工具
---
** ubuntu服务器私人定制：** <Excerpt in index | 首页摘要>
把ubuntu服务器打造成自己的个性服务器，装逼必备！！！
<!-- more -->
<The rest of contents | 余下全文>

##　说明
**此教程针对Ubuntu14,其他版本仅作参考**

##　用户密码管理
`sudo passwd root`
1. 添加一个用户组并指定id为1002
`sudo groupadd －g 1002 www`
2. 添加一个用户到www组并指定id为1003
`sudo useradd wyx -g 1002 -u 1003 -m`

3. 修改用户的密码
`sudo passwd wyx`
4. 删除一个用户
`sudo userdel wyx`

5. 为该用户添加sudo权限

```bash
sudo usermod -a -G adm wyx
sudo usermod -a -G sudo wyx
```

6. 查看所有用户和用户组：
```bash
cat /etc/passwd
cat /etc/group
```
## 安装nodejs
1. 安装nvm`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash`
2. 安装node`nvm install v4.4.4`,安装`nvm install v6.9.1`
3. 设置默认的node版本`nvm alias default v4.4.4`
4. 安装npm3  `npm install -g npm@3`
5. 设置淘宝的cnpm源  `npm install -g cnpm --registry=https://registry.npm.taobao.org`
6. 验证安装`node -v,npm -v,cnpm -v`
## 安装node常用包
1. 安装pm2`cnpm install -g pm2`
2. 安装hexo博客`cnpm install -g hexo-cli`
3. 安装同步插件rsync`cnpm install -g rsync`

## 安装docker
1. apt安装

```bash
sudo apt-get update
sudo apt-get install -y docker.io
sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
sudo sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io
```

2. 源码安装最新版本

```bash
sudo apt-get install apt-transport-https
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
sudo apt-get update
sudo apt-get install lxc-docker
```

3. 验证安装版本
` docker -v`

## 安装nginx
`sudo apt-get install nginx`
启动和配置nginx
## 安装redis
`sudo apt-get install redis-server`
启动和配置文件:
## 安装mongodb
1. 安装3.0

```bash
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
echo "deb http://repo.mongodb.org/apt/debian wheezy/mongodb-org/3.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
apt-get update  
apt-get install mongodb-org
```

2. 安装3.2最新版

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb.list
sudo apt-get update
sudo apt-get install mongodb-org
```

3. 制定版本
`apt-get install mongodb-org=3.2.0 mongodb-org-server=3.2.0 mongodb-org-shell=3.2.0 mongodb-org-mongos=3.2.0 mongodb-org-tools=3.2.0`

4. 启动服务

```bash
sudo service mongod start
sudo service mongod stop
```

5. 验证安装
`mongod --version`

配置

## 安装jdk
安装jdk1.7`sudo apt-get install openjdk-7-jdk`
源码安装

```bash
sudo mkdir /usr/lib/jvm
sudo tar zxvf jdk-7u21-linux-i586.tar.gz -C /usr/lib/jvm
cd /usr/lib/jvm
sudo mv jdk1.7.0_21 java

sudo vim ~/.bashrc

export JAVA_HOME=/usr/lib/jvm/java
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  
```
## 安装mysql
实用ubuntu自带的工具下载
`sudo apt-get install mysql-server`

## 环境变量
常见的方法有两种。

1. 在用户主目录下有一个 .bashrc 文件，可以在此文件中加入 PATH 的设置如下： 
`export PATH=”$PATH:/your path1/:/your path2/…..” `

2. 在 /etc/profile中增加
```bash
PATH="$PATH:/home/zhengb66/bin" 
export PATH
``` 

## 开机自启动
1. 方法一，编辑rc.loacl脚本
Ubuntu开机之后会执行/etc/rc.local文件中的脚本，
所以我们可以直接在/etc/rc.local中添加启动脚本。
当然要添加到语句：exit 0 前面才行。代码如下:
`sudo vi /etc/rc.local`
然后在 exit 0 前面添加好脚本代码。

2. 方法二，添加一个Ubuntu的开机启动服务。
如果要添加为开机启动执行的脚本文件，
可先将脚本复制或者软连接到/etc/init.d/目录下，
然后用：update-rc.d xxx defaults NN命令(NN为启动顺序)，
将脚本添加到初始化执行的队列中去。
注意如果脚本需要用到网络，则NN需设置一个比较大的数字，如99。
1) 将你的启动脚本复制到 /etc/init.d目录下
 以下假设你的脚本文件名为 test。
2) 设置脚本文件的权限

代码如下:
`sudo chmod 755 /etc/init.d/test`
3) 执行如下命令将脚本放到启动脚本中去：
代码如下:
`cd /etc/init.d`  `sudo update-rc.d test defaults 95`
 注：其中数字95是脚本启动的顺序号，按照自己的需要相应修改即可。在你有多个启动脚本，而它们之间又有先后启动的依赖关系时你就知道这个数字的具体作用了。该命令的输出信息参考如下：
卸载启动脚本的方法：
代码如下:
`cd /etc/init.d`
`sudo update-rc.d -f test remove`

## 定时任务
在Ubuntu下，cron是被默认安装并启动的。通过查看/etc/crontab
推荐使用crontab -e命令添加自定义的任务（编辑的是/var/spool/cron下对应用户的cron文件，在/var/spool/cron下的crontab文件 不可以直接创建或者直接修改，crontab文件是通过crontab命令得到的）。
`crontab -e`

1. 直接执行命令行
每2分钟打印一个字符串“Hello World”，保存至文件/home/laigw/cron/HelloWorld.txt中，cron 格式如下：
`*/2 * * * * echo “Hello World.” >> /home/HelloWorld.txt`

2. shell 文件
每3分钟调用一次 /home/laigw/cron/test.sh 文件，cron 格式如下：
`*/3 * * * * /home/laigw/cron/test.sh`
## ftp和rsync配置

## 持续集成环境
1. jenkens配置
2. gitlab配置
3. git服务器

> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
