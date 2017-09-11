### 功能说明

文件服务，提供统一的接口，目前底层提供了七牛、阿里云OSS、fastdfs三个文件服务实现。

* 可以通过配置文件自由切换
* 丰富多样的上传类型：文件、文件路径、byte\[\]、inputStream等
* 自动文件类型识别

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

#### 使用

##### 自己通过构造方法构造实例

```java
public void test() {
    //七牛
    FSProvider provider = new QiniuProvider("http://ovjjqjpmp.bkt.clouddn.com", "test", "accessKey", "secretKey", true);
    String url = provider.upload(new UploadObject(new File("/Users/jiangwei/Desktop/homepage.txt")));
    provider.downloadAndSaveAs(url, "/tmp");
    String downloadUrl = provider.getDownloadUrl(url);
    //阿里云
    provider = new AliyunossProvider("http://oss-cn-ghuangzhou.aliyuncs.com", "test", "accessKey", "secretKey");
    url = provider.upload(new UploadObject(new File("/Users/jiangwei/Desktop/homepage.txt")));
    provider.downloadAndSaveAs(url, "/tmp");
    downloadUrl = provider.getDownloadUrl(url);
    //fastDFS
    provider = new FdfsProvider("http://192.168.1.100:81", "fastdfstest", new String[] {
        "192.168.1.100:22122"
    },
    3000, 100);
    url = provider.upload(new UploadObject(new File("/Users/jiangwei/Desktop/homepage.txt")));
    provider.downloadAndSaveAs(url, "/tmp");
    downloadUrl = provider.getDownloadUrl(url);
}
```

##### 通过工厂方法

配置文件

```
fs.groupNames=test,fastdfstest
# 服务提供者可选值（fastDFS，qiniu，aliyun）
fs.test.provider=qiniu
fs.test.accessKey=iqq3aa-ncqfdGGubCcS-N8EUV-qale2ezndnrtKS
fs.test.secretKey=1RmdaMVjrjXkyRVPOmyMa6BzcdG5VDdF-SH_HUTe
fs.test.urlprefix=http://7xq7jj.com1.z0.glb.clouddn.com
fs.test.private=true
# fastdfstest 配置
fs.fastdfstest.provider=fastDFS
fs.fastdfstest.servers=192.168.1.101:22122,192.168.1.102:22122
fs.fastdfstest.connectTimeout=3000
fs.fastdfstest.maxThreads=100
```

使用

```java
FSProvider provider = FSClientFactory.build("qiniu", "test");
String url = provider.upload(new UploadObject(new File("/Users/jiangwei/Desktop/homepage.txt")));
System.out.println(provider.getDownloadUrl(url));
```



