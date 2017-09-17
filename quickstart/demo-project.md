### 下载项目

```
git clone https://git.oschina.net/vakinge/jeesuite-bestpl.git
```

### 编译项目

```
mvn clean package -DskipTests=true
```

**最终生成部署包为：**

* dubbo服务:bestpl-dubbo/target/bestpl-dubbo-dist.zip
* springboot web：bestpl-springboot/target/bestpl-springboot.jar
* rest API:bestpl-rest/target/bestpl-rest.war

### 项目部署

**说明**：目前没有前后端分离所以rest API模块暂时用不上，无需部署。

* 步骤一：分别将以上构建的部署包拷贝到部署目录，如：/datas/deploy目录下。
* 步骤二：部署dubbo服务
  ![](http://ojmezn0eq.bkt.clouddn.com/duubo-deploy-1.png)启动成功，如下：

```
2017-09-17 11:22:13.752 INFO [main][AppServer.java:31] - 启动app-server....
==============================
|---------服务启动成功----------|
==============================
```

* 步骤三：部署springboot
  ```
  nohup java -jar bestpl-springboot.jar > bestpl-springboot.out 2>&1 &
  ```

启动成功后，访问：[http://127.0.0.1:8081/](http://127.0.0.1:8081/)  查看效果

#### 首页效果
![image](http://ojmezn0eq.bkt.clouddn.com/bestpl_snapshot.png)