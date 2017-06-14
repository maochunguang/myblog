title: ubuntu16开发环境配置
date: 2016-04-26 18:48:11
tags: linux
categories: 开发工具
---
** ubuntu开发环境配置：** <Excerpt in index | 首页摘要>
    ubuntu16下node,java开发环境配置
 <!-- more -->
<The rest of contents | 余下全文>

## 安装系统软件
1. 更新系统和软件
   ```
   sudo apt-get update
   sudo apt-get upgade
   ```
2. 谷歌浏览器，火狐浏览器，atom编辑器，sublime编辑器，webstome,idea,eclipse
3. 安装搜狗输入法（官网），安装fcitx配置搜狗输入法

## 安装jdk
1. 下载jdk并新建一个文件夹
    ```
    sudo mkdir /usr/lib/jvm
    ```
2. 解压文件
    ```
    sudo tar zxvf jdk-7u71-linux-x64.tar.gz -C /usr/lib/jvm/jdk1.7
    ```
3. 设置环境变量,设置~/.zshrc文件,或者编辑/etc/profile（全局）文件
    ```
    export JAVA_HOME=/usr/lib/jvm/jdk1.7
    export JRE_HOME=${JAVA_HOME}/jre  
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
    export PATH=${JAVA_HOME}/bin:$PATH
    ```
4. 检查是否安装成功
    打开shell,
    ```
    java --version
    ```

## 安装nodejs
1. nodejs版本迭代较快，有时候需要检查在不同版本下的兼容性问题，用nvm来控制版本
2. 安装nvm,source的时候根据自己的shell版本，~/.bashrc, ~/.profile, 或者 ~/.zshrc
    ```
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
    source ~/.profile
    ```
3. 安装不同版本的nodejs
　　```
    nvm ls-remote
    nvm install v0.12.9
    nvm install 5.0
    nvm use 0.12.9
    nvm alias default 0.12.9
    ```

## 安装mongodb
1. 配置公钥
```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
```
2. 更新软件列表
```bash
sudo apt-get update
sudo apt-get install -y mongodb-org
```
3. 完成上面的安装步骤配置mongodb的数据库的位置
```bash
sudo mongod --dbpath /data/db
```
4. 启动mongod
```bash
sudo service mongod start
sudo service mongod stop
sudo service mongod restart
```

## 安装redis
1. 下载软件
```bash
wget http://download.redis.io/releases/redis-2.8.11.tar.gz
```
2. 解压安装
```bash
tar xvfz redis-2.8.11.tar.gz
cd redis-2.8.11 && sudo make && sudo make install
```
3. 配置使用
    1. 下载配置文件和init启动脚本
    ```bash
    wget https://github.com/ijonas/dotfiles/raw/master/etc/init.d/redis-server
    wget https://github.com/ijonas/dotfiles/raw/master/etc/redis.conf
    sudo mv redis-server /etc/init.d/redis-server
    sudo chmod +x /etc/init.d/redis-server
    sudo mv redis.conf /etc/redis.conf
    ```
    2. 初始化用户和日志路径
    ```bash
    sudo useradd redis
    sudo mkdir -p /var/lib/redis
    sudo mkdir -p /var/log/redis
    sudo chown redis.redis /var/lib/redis
    sudo chown redis.redis /var/log/redis
    ```
    3. 设置开机自动启动，关机自动关闭
    ```bash
    sudo update-rc.d redis-server defaults
    ```

## 环境变量配置
1. 认识环境变量相关的文件
- /etc/profile —— 此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从/etc/profile.d目录的配置文件中搜集shell的设置；
- /etc/environment —— 在登录时操作系统使用的第二个文件,系统在读取你自己的profile前,设置环境文件的环境变量；
- /etc/bashrc —— 为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取；
- ~/.profile —— 每个用户都可使用该文件输入专用于自己使用的shell信息，当用户登录时，该文件仅仅执行一次！默认情况下,它设置一些环境变量,执行用户的.bashrc文件；
- ~/.bashrc —— 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该文件被读取；
2. 配置环境变量
- 在Ubuntu14.04的~/.bashrc中添加的环境变量,在文件添加
``` bash
export PATH=$PATH:/home/qtcreator-2.6.1/bin
```
- 修改profile文件,vim编辑/etc/profile
```bash
sudo vim /etc/profile
source /etc/profile
```

## 安装开发工具
1. zsh命令行工具
2. mysql客户端workbench，mongo客户端工具robomongo
3. 安装git,svn版本控制工具
```bash
sudo apt-get install git
sudo apt-get install subversion
```
