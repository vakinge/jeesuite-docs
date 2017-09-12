### 下载项目

```
git clone https://git.oschina.net/vakinge/jeesuite-config.git
```

### 编译项目

```
mvn clean package -DskipTests=true
```

**最终生成部署包为：**jeesuite-config-server/target/jeesuite-config-server.jar

### 修改配置

可以在编译前直接修改\`jeesuite-config/jeesuite-config-server/src/main/resources/application.properties\`文件，也可以加载外部配置文件，推荐外部加载配置文件。

**配置说明**

```
spring.application.name=jeesuite-config-server
server.port=7777

mybatis.type-aliases-package=com.jeesuite.admin.dao.entity
mybatis.mapper-locations=classpath:mapper/*Mapper.xml

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
master.db.url=jdbc:mysql://192.168.1.94:3306/jeesuite_config?useUnicode=true&amp;characterEncoding=UTF-8
master.db.username=root
master.db.password=root
master.db.initialSize=2
master.db.minIdle=2
master.db.maxActive=20

#是否允许通过外网拉取配置
api.extranet.enabled=true
#是否启用安全ip
safe.ipfilter.enabled=false
#设置安全ip是安全码(相当于二次密码)
safe.ipfilter.authcode=12345678

#如果需要rsa加密，配置公钥
cc.encrypt.keyStore.location=/Users/jiangwei/configcenter.jks
cc.encrypt.keyStore.password=123456
cc.encrypt.keyStore.type=JCEKS
#如果需要通过zookeeper通知配置更新，则需要配置
cc.sync.zkServers=127.0.0.1:2181
```

### 部署

```
nohup java -jar jeesuite-config-server.jar > demo.out 2>&1 &
```



