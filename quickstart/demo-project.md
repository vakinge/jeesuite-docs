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

   - 步骤一：分别将以上构建的部署包拷贝到部署目录，如：/datas/apps目录下。



