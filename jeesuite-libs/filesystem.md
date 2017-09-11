### 功能说明

文件服务，提供统一的接口，目前底层提供了七牛、阿里云OSS、fastdfs三个文件服务实现。  
可以通过配置文件自由切换。

### 使用说明

#### 添加依赖

```
<dependency>
    <groupId>com.jeesuite</groupId>
    <artifactId>jeesuite-filesystem</artifactId>
    <version>[最新版本]</version>
</dependency>

<!--七牛需要依赖-->
<dependency>
	<groupId>com.qiniu</groupId>
	<artifactId>qiniu-java-sdk</artifactId>
	<version>7.2.2</version>
</dependency>

<!--fastDFS需要依赖-->
<dependency>
  <groupId>io.netty</groupId>
  <artifactId>netty-all</artifactId>
  <version>4.0.34.Final</version>
</dependency>
<!--阿里云需依赖-->
<dependency>
   <groupId>com.aliyun.oss</groupId>
   <artifactId>aliyun-sdk-oss</artifactId>
   <version>2.8.1</version>
</dependency>
```



