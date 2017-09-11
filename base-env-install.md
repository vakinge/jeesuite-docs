## 环境：centos6.5

### 安装JDK

```
1.下载jdk rpm包
2.chmod +x  jdk-xxx.rpm
3.rpm -ivh jdk-xxx.rpm
4.配置环境变量
vi /etc/profile
-----------
export JAVA_HOME=/usr/java/jdk1.8.0_121
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH
-----------
source /etc/profile
```

### 安装mysql5.7

```
# 下载mysql源安装包
wget https://dev.mysql.com/get/mysql57-community-release-el6-11.noarch.rpm
# 安装mysql源
yum localinstall mysql57-community-release-el7-8.noarch.rpm

以上两步可以直接：rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el6-11.noarch.rpm


#安装mysql-server
yum install mysql-community-server
```

验证安装是否成功

```
mysql -V
```

启动（第一次启动比较慢会初始化）

```
service mysqld start 
-- 如果启动失败，提示“MySQL Daemon failed to start”，可以尝试先初始化mysql，输入命令：mysqld --initialize
```

初始化root密码

```bash
#获取安装的临时密码
grep 'temporary password' /var/log/mysqld.log
#登录mysql
mysql -uroot -p
#更新密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '新的密码';
```

配置

```
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
```

### 安装redis

```
--
```



