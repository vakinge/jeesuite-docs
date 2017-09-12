## 环境：centos6.5 64位

由于yum直接安装版本版本会比较低，所以采用手动安装的方式。

### 安装JDK {#jdk}

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

### 安装mysql5.7 {#mysql}

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

```ruby
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
wget http://download.redis.io/releases/redis-3.2.10.tar.gz
tar -xzvf redis-3.2.10.tar.gz
cd redis-3.2.10
make
cd src
make install PREFIX=/usr/local/redis
cp ../redis.conf /etc/redis.conf
```

参考配置

```
daemonize yes
#protected-mode no
pidfile /var/run/redis/redis.pid

port 6379
#bind 120.0.0.1
timeout 0
loglevel warning

logfile /datas/logs/redis/redis.log
databases 16


save 900 1
save 300 10
save 60 10000

rdbcompression yes
dbfilename dump.rdb
dir /datas/redis/

requirepass 123456
```

启动

```
/usr/local/redis/bin/redis-server /etc/redis.conf
```

### 安装zookeeper

```
wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz
tar -xzvf zookeeper-3.4.10.tar.gz
cp zookeeper-3.4.10
mv conf/zoo_sample.cfg conf/zookeeper.properties
```

启动

```
./bin/zkServer.sh conf/zookeeper.properties &
```

### 安装kafka

```
wget https://www.apache.org/dyn/closer.cgi?path=/kafka/0.11.0.0/kafka_2.11-0.11.0.0.tgz
tar -xzvf kafka_2.11-0.11.0.0.tgz
cp kafka_2.11-0.11.0.0
# 修改配置
vi config/server.properties
```

关键配置

```
broker.id=1
#advertised.host.name=192.168.1.94
#listeners=PLAINTEXT://192.168.1.94:9092
port=9092
num.network.threads=3
num.io.threads=3
socket.send.buffer.bytes=102400
socket.receive.buffer.bytes=102400
socket.request.max.bytes=104857600
log.dirs=/datas/kafka-logs
num.partitions=3
num.recovery.threads.per.data.dir=1
log.retention.hours=168
log.segment.bytes=1073741824
log.retention.check.interval.ms=300000
auto.create.topics.enable=true
zookeeper.connect=localhost:2181
zookeeper.connection.timeout.ms=6000
```

启动

```
./bin/kafka-server-start.sh ./config/server.properties 1>/dev/null 2>&1 &
```



