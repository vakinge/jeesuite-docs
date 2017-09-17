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
#指定全局组id （默认为：public & private）
public.filesystem.id=public
private.filesystem.id=private

#全局公共空间
public.filesystem.provider=qiniu
public.filesystem.bucketName=test112233
public.filesystem.urlprefix=http://owep828p6.bkt.clouddn.com
public.filesystem.accessKey=iqq3aa-ncqfdGGubCcS-N8EUV-qale2ezndnrtKS
public.filesystem.secretKey=1RmdaMVjrjXkyRVPOmyMa6BzcdG5VDdF-SH_HUTe
public.filesystem.private=false

#全局私有空间
private.filesystem.provider=qiniu
private.filesystem.bucketName=testa1b2c3
private.filesystem.urlprefix=http://ovjjqjpmp.bkt.clouddn.com
private.filesystem.accessKey=iqq3aa-ncqfdGGubCcS-N8EUV-qale2ezndnrtKS
private.filesystem.secretKey=1RmdaMVjrjXkyRVPOmyMa6BzcdG5VDdF-SH_HUTe
private.filesystem.private=true

#自定义空间
report.filesystem.provider=qiniu
report.filesystem.bucketName=report123
report.filesystem.urlprefix=http://rtjjqrrpmp.bkt.clouddn.com
report.filesystem.accessKey=iqq3aa-ncqfdGGubCcS-N8EUV-qale2ezndnrtKS
report.filesystem.secretKey=1RmdaMVjrjXkyRVPOmyMa6BzcdG5VDdF-SH_HUTe
report.filesystem.private=true

# 使用fastdfs
xxxx1.filesystem.provider=fastDFS
xxxx1.filesystem.servers=120.24.185.19:22122
xxxx1.filesystem.urlprefix=http://120.24.185.19:81
xxxx1.filesystem.connectTimeout=3000
xxxx1.filesystem.maxThreads=100

#使用阿里云
xxxx2.filesystem.provider=aliyun
xxxx2.filesystem.bucketName=aaabb
xxxx2.filesystem.endpoint=http://guangzhou.aliyun.com
xxxx2.filesystem.accessKey=111111
xxxx2.filesystem.secretKey=22222
```

使用

```java
public void test() {
   //上传到全局公有
   String url = FileSystemClient.getPublicClient().upload(new File("/Users/jiangwei/readme.txt"));
   System.out.println(url);

   //上传到全局私有空间
   url = FileSystemClient.getPrivateClient().upload("readme2.txt", new File("/Users/jiangwei/readme.txt"));
   //生成私有下载链接
   String downloadUrl = FileSystemClient.getPrivateClient().getDownloadUrl(url);

   //上传到自定义空间
   url = FileSystemClient.getClient("report").upload(new File("/Users/jiangwei/report.xls"));
}
```



