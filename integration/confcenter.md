## 介绍

配置中心提供应用管理，配置统一管理，应用监控等。

## 使用说明

### 添加依赖

```
<dependency>
  <groupId>com.jeesuite</groupId>
  <artifactId>jeesuite-config-client</artifactId>
 <version>1.1.5</version>
</dependency>
```

### 添加配置

普通项目添加在`confcenter.properties`,springboot项目添加在`application.properties`

```
#是否启用配置中心，默认：true
jeesuite.configcenter.enabled=false
#应用名，springboot默认：${spring.application.name}
jeesuite.configcenter.appName=passport
jeesuite.configcenter.base.url=http://configserver:7777
#当前环境
jeesuite.configcenter.profile=dev
#同步方式，默认:http
jeesuite.configcenter.sync-type=zookeeper
# 同步间隔，同步方式为：http时生效
jeesuite.configcenter.sync-interval-seconds=30
# 如果需要对配置RSA加密，则配置以下内容，证书需要提前生成
jeesuite.configcenter.encrypt-keyStore-location=classpath:private.jks
jeesuite.configcenter.encrypt-keyStore-password=123456
jeesuite.configcenter.encrypt-keyStore-type=JCEKS
jeesuite.configcenter.encrypt-keyStore-alias=demo
jeesuite.configcenter.encrypt-keyStore-keyPassword=123456
```

### 通过JVM参数外部设置配置

```
-Djeesuite.configcenter.profile=dev
-Djeesuite.configcenter.encrypt-keyStore-location=/Users/jiangwei/private.jks
-Djeesuite.configcenter.encrypt-keyStore-password=123456
-Djeesuite.configcenter.encrypt-keyStore-alias=demo
-Djeesuite.configcenter.encrypt-keyStore-keyPassword=123456
```

### docker外部设置配置

```
-e jeesuite.configcenter.profile="dev"
-e jeesuite.configcenter.encrypt-keyStore-location="/Users/jiangwei/private.jks"
-e jeesuite.configcenter.encrypt-keyStore-password="123456"
-e jeesuite.configcenter.encrypt-keyStore-alias="demo"
-e jeesuite.configcenter.encrypt-keyStore-keyPassword="123456"
```

### 配置中心新增配置

1. 登录[http://${configserver}:7777\(开发环境\)](http://${configserver}:7777)，默认管理员账号密码：_**admin/admin123**_。
2. 新增应用
3. 新增对应应用及环境配置

```
http://${configserver}:7777/api/fetch_all_configs?appName=${jeesuite.configcenter.appName}&env=dev&version=0.0.0
```

### 普通spring项目配置xml

1. 去掉原加载配置相关配置
2. 新增配置

```xml
<bean class="com.jeesuite.confcenter.spring.CCPropertyPlaceholderConfigurer">
    <property name="remoteEnabled" value="true" />
    <!-- 本地配置文件，无本地配置可不配置 -->
    <property name="locations">
      <list>
        <value>classpath*:application.properties</value>
      </list>
    </property>
</bean>
```

### 配置优先级

1. 应用本地配置 &gt; 远程应用配置 &gt; 远程全局配置
2. 配置中心配置`jeesuite.configcenter.remote-config-first=true`可以启用远程配置覆盖本地配置

### 配置实时生效

配置变更后会实时下发到应用，可以通过以下方式实时读取最新配置  
1. 在代码中使用`ResourceUtils`实时读取  
2. 依赖注入`Environment`，在代码中实时读取  
3. 实现`ConfigChangeHanlder`接口，自定义刷新逻辑

---

```java
@Controller  
@RequestMapping(value = "/sms")
public class AuthCommonController implements ConfigChangeHanlder{

    @Value("${sms.send.open}")
    private boolean open = false;


    @Override
    public void onConfigChanged(Map<String, Object> changedConfigs) {
        if(changedConfigs.containsKey("sms.send.open")){
            open = Boolean.parseBoolean(changedConfigs.get("sms.send.open").toString());
        }
    }
}
```



