### 下载项目

```
git clone https://git.oschina.net/vakinge/jeesuite-config.git
```

### 编译项目

```
mvn clean package -DskipTests=true
```

**最终生成部署包为：**jeesuite-config-server/target/jeesuite-config-server.jar

### 创建数据库表

    CREATE DATABASE IF NOT EXISTS `configcenter` DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

导入db.sql

### 修改配置

可以在编译前直接修改\`jeesuite-config/jeesuite-config-server/src/main/resources/application.properties\`文件，也可以加载外部配置文件，推荐外部加载配置文件。

**配置说明**

```
spring.application.name=jeesuite-config-server
server.port=19992

mybatis.type-aliases-package=com.jeesuite.admin.dao.entity
mybatis.mapper-locations=classpath:mapper/*Mapper.xml

#以下配置，根据需要修改
#数据库配置
db.group.size=1
db.shard.size=1000
db.driverClass=com.mysql.jdbc.Driver
db.initialSize=2
db.minIdle=1
db.maxActive=10
db.maxWait=60000
db.timeBetweenEvictionRunsMillis=60000
db.minEvictableIdleTimeMillis=300000
db.testOnBorrow=false
db.testOnReturn=false
master.db.url=jdbc:mysql://127.0.0.1:3306/configcenter?useUnicode=true&amp;characterEncoding=UTF-8
master.db.username=root
master.db.password=root
master.db.initialSize=2
master.db.minIdle=2
master.db.maxActive=20

#是否允许通过外网拉取配置
api.extranet.enabled=true
#是否启用安全ip
safe.ipfilter.enabled=false
#敏感操作安全码(相当于二次密码)
sensitive.operation.authcode=12345678

#如果需要rsa加密，配置公钥
cc.encrypt.keyStore.location=/Users/jiangwei/configcenter.jks
cc.encrypt.keyStore.password=123456
cc.encrypt.keyStore.type=JCEKS
#如果需要通过zookeeper通知配置更新，则需要配置
cc.sync.zkServers=127.0.0.1:2181
```

### 部署

拷贝**jeesuite-config-server.jar** 与 _**application.properties**_在同一目录，springboot会优先加载同一目录下名为_application.properties的配置文件。_

```
nohup java -jar jeesuite-config-server.jar > config-server.out 2>&1 &
```

启动成功,打开[http://127.0.0.1:19992](http://127.0.0.1:19992) ，初始账号密码：admin /admin123

