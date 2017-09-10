**common模块提供常用工具类。**

* 加密类：AES,DES,RSA,Base64，SHA1，SimpleCryptUtils\(可逆加密\)，DigestUtils\(MD5、MD5短码\)
* HttpUtils:基于HttpURLConnection实现的http操作工具类，无第三方依赖，适用简单无性能要求场景。
   ```
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
        responseEntity = HttpUtils.postJson("http://192.168.1.89:9082/delete", "{'id':'1000'}", DEFAULT_CHARSET);

        if(responseEntity.isSuccessed()){
            System.out.println(responseEntity.getBody());
         }
  ```

* JsonUtils:基于jackson包封装。
  ```

  ```



