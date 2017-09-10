common2也是工具类，较common模块重量级一些，需要依赖第三方环境如：redis、zookeeper等，故与common分开。目前实现了：全局ID生成器、分布式锁\(redis版、zookeeper版\)、excel工具。

### 全局Id生成器

目前只实现基于Snowflake算法生成器，后续新增基于数据库sequence的。

```java
SnowflakeGenerator generator = new SnowflakeGenerator("127.0.0.1:2181");
long id = generator.nextId();
System.out.println(id);
```

### 分布式锁

```java
//redis
public void run() {
    RedisDistributeLock lock = new RedisDistributeLock("test");
    //boolean getLock = lock.tryLock(5, TimeUnit.SECONDS);
    lock.lock();
    System.out.println("LockWorker[" + id + "] get lock,doing");
    try {
        Thread.sleep(RandomUtils.nextLong(1000, 10000));
    } catch(Exception e) {}
    lock.unlock();
    latch.countDown();
    System.out.println("LockWorker[" + id + "] release lock,done");
}

//zookeeper
@Override public void run() {
    ZkDistributeLock lock = new ZkDistributeLock("127.0.0.1:2181", "test");
    lock.lock();
    System.out.println("LockWorker[" + id + "] get lock,doing");
    try {
        Thread.sleep(2000);
    } catch(Exception e) {}
    lock.unlock();
    latch.countDown();
    System.out.println("LockWorker[" + id + "] release lock,done");
}
```

### excel操作

提供poi用户模式操作封装以及大数据量性能模式读取方式。

```java
//数据模型
public class PersonSalaryInfo {

	@TitleCell(name="*姓名",column = 1,notNull = true)
	private String name;
	@TitleCell(name="部门",column = 2)
	private String department;
	@TitleCell(name="身份证号",column = 3 )
	private String idCard;
	@TitleCell(name="基本工资",column = 4,row = 2,parentName = "应发工资",type = Float.class)
	private float baseSalary;//基本工资
	@TitleCell(name="岗位工资",column = 5,row = 2,parentName = "应发工资",type = Float.class)
	private float postSalary;//岗位工资
	@TitleCell(name="绩效工资",column = 6,row = 2,parentName = "应发工资",type = Float.class)
	private float performSalary;//绩效工资
	@TitleCell(name="福利津贴",column = 7,row = 2,parentName = "应发工资",type = Float.class)
	private float subsidies;//福利津贴
	@TitleCell(name="扣除金额",column = 8,row = 2,parentName = "应发工资",type = Float.class)
	private float deductSalary; //扣除金额
	@TitleCell(name="*总计",column = 9,row = 2,parentName = "应发工资",notNull = true,type = Float.class)
	private float total;
	@TitleCell(name="*社保基数",column = 10,notNull = true,type = Float.class)
	private float insuranceBase;//社保基数
	@TitleCell(name="*公积金基数",column = 11,notNull = true,type = Float.class)
	private float housefundBase;
}

//
public static void main(String[] args) throws InvalidFormatException,IOException {
    //普通方式读取
    String excelFilePath = "/Users/jiangwei/Desktop/工资.xlsx";
    ExcelReader excelReader = new ExcelReader(excelFilePath);
    excelReader.parse(PersonSalaryInfo.class);
    excelReader.close();
    //大文件读取防止内存溢出
    List < PersonSalaryInfo > list = new ExcelPerfModeReader(excelFilePath).read(PersonSalaryInfo.class);

    //write
    ExcelWriter writer = new ExcelWriter("/Users/jiangwei/Desktop/工资bak.xlsx");
    writer.write(list, PersonSalaryInfo.class);
    writer.close();
}

```



