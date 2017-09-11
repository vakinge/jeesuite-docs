### 功能说明

* spring依赖包管理，静态bean工厂，一些工具类。

### 使用说明

#### 添加依赖

```
<dependency>
    <groupId>com.jeesuite</groupId>
    <artifactId>jeesuite-spring</artifactId>
    <version>[最新版本]</version>
</dependency>
```

静态bean工厂

```
//获取bean实例
JedisProvider provider = InstanceFactory.getInstance(JedisProvider.class);
//阻塞只到context初始化
InstanceFactory.waitUtilInitialized();
```

helper类

* EnvironmentHelper
* SpringAopHelper



