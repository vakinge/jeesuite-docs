### 功能说明

* 自动resonse封装（xml、json）
* i18n
* request、response日志记录
* 全局异常 处理
* 校验框架

### 使用说明

现rest通过springboot发布，该模块不做更新，只做bug修复。

#### 添加依赖

```
<dependency>
    <groupId>com.jeesuite</groupId>
    <artifactId>jeesuite-rest</artifactId>
    <version>[最新版本]</version>
</dependency>
```

```java
import com.jeesuite.bestpl.exception.DemoBaseException;
import com.jeesuite.rest.BaseApplicaionConfig;
import com.jeesuite.rest.excetion.ExcetionWrapper;
import com.jeesuite.rest.response.WrapperResponseEntity;


public class ApplicationConfig extends BaseApplicaionConfig {

	public ApplicationConfig() {
		super();
	}

	@Override
	public ExcetionWrapper createExcetionWrapper() {
		return new bestplExcetionWrapper();
	}

	@Override
	public String packages() {
		return "com.jeesuite.bestpl.rest";
	}
	
	public static class bestplExcetionWrapper implements ExcetionWrapper{
		@Override
		public WrapperResponseEntity toResponse(Exception e) {
			if(e instanceof DemoBaseException){
				DemoBaseException ex = (DemoBaseException) e;
				return new WrapperResponseEntity(ex.getCode() + "", ex.getMessage(), true);
			}
			// 
			return null;
		}
		
	}
	
}
```



