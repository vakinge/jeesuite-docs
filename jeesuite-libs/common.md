### 功能说明

cache模块实现了

### 使用说明

#### 添加依赖

```
<dependency>
    <groupId>com.jeesuite</groupId>
    <artifactId>jeesuite-cache</artifactId>
    <version>[最新版本]</version>
</dependency>
```

* 加密类：AES,DES,RSA,Base64，SHA1，SimpleCryptUtils\(可逆加密\)，DigestUtils\(MD5、MD5短码\)
* HttpUtils:基于HttpURLConnection实现的http操作工具类，无第三方依赖，适用简单无性能要求场景。

  ```java
        HttpResponseEntity responseEntity = HttpUtils.get("http://192.168.1.89:9082/info");
        //上传文件
        HttpRequestEntity entity = HttpRequestEntity.create()
                                             .addFileParam("file", new FileItem("/Users/jiangwei/Desktop/homepage.txt"))
                                             .basicAuth("admin", "123456");
        responseEntity = HttpUtils.post("http://192.168.1.89:9082/upload", entity);
        System.out.println(responseEntity);

        //post
        responseEntity = HttpUtils.post("http://192.168.1.89:9082/login", 
                      HttpRequestEntity.create()
                                       .addTextParam("name", "vakinge")
                                       .addTextParam("password", "123456"));

        //post json
        responseEntity = HttpUtils.postJson("http://192.168.1.89:9082/delete", "{'id':'1000'}", "utf-8");

        if(responseEntity.isSuccessed()){
            System.out.println(responseEntity.getBody());
         }
  ```

* JSON:JsonUtils

* 序列化：FSTSerializer，JavaSerializer，KryoSerializer，KryoPoolSerializer

* 包扫描：PackageScanner

* BeanCopyUtils：bean之间值复制工具类，比BeanUtils效率高

* DateUtils：常用日期函数，与common-lang包日期函数互补
* FormatValidateUtils：常用格式校验工具
* HashUtils：MurMurHash算法实现的hash算法
* JDBCUtils：jdbc工具
* ReflectUtils：反射工具
* ResourceUtils：属性读取工具
* NodeNameHolder：全局节点名称



