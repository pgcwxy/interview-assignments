# 短域名服务
## 功能介绍
- 根据长域名生成并保存短域名
- 根据短域名搜索长域名
- 全球最多支持部署94个无状态的节点
- 每个节点支持每秒940个请求（有限速器）
- 短域名最大长度不超过8个字符
- 短域名支持保存1000年
- 长短域名对应关系保存于内存中，同时会持久化到本地日志中，系统重启支持自动恢复
- 自动检测JVM空间剩余情况，当内存不足时，自动停止生成和保存新的短域名(可以重启并扩大JVM内存空间)
- 详细的swagger文档 ，截图参考：[github地址](https://github.com/pgcwxy/interview-assignments/tree/master/java/screenshot)
- Jacoco单元测试平均覆盖率大于90%，截图参考：[github地址](https://github.com/pgcwxy/interview-assignments/tree/master/java/screenshot)

## 设计思路
### 根据长域名生成短域名的核心方法
```$xslt

根据需求短域名长度要求为8个字符，本程序共选择了94个字符，请参考：GenAndSaveSURLService.ARRAY
本方法采用类似雪花算法的方式，将8位字符分隔成3部分，分别是：
1. 1到6位，共6位，作为时间戳，共支持表达 94^6=6.8e11 个数字，本程序采用0.1秒为一个数字的方式进行换算
2. 第7位，共1位，作为短域名生成器的ID，共支持表达 94 个数字
3. 第8位，共1位，作为一个自增序列，共支持表达 94 个数字

各个部分单独分析：
1. 时间戳部分
支持从1970年1月1日之后的1000年
2. 短域名生成器
共支持94个无状态的服务分布式部署，不会生成重复的数字
3. 自增序列
程序内有令牌桶限速逻辑，单个时间周期（时间戳部分增长一个数字）内，自增序列只能自增一个循环，之后便会阻塞

QPS：
单个短域名生成服务0.1秒内可以生成94个短域名，1秒内可以生成940个短域名
共支持部署94个短域名生成服务，也就是1秒内最多可以生成940*94=88,360个短域名

```

-----------------------------
下面内容是原始需求

# Java Assignment

## 这是什么？

为了节省大家的时间，我们使用作业分配来对Java候选人进行资格预审。这使我们在面试中保持客观，专注于候选人解决​​复杂问题并捍卫他们选择技术或方法的能力。我们还评估候选人如何处理来自同事、管理层或运营团队的压力，时间压力，批评和审查。

***要考虑参加面试，您需要完成下面的“作业”部分。***

### Assignment

#### 实现短域名服务（细节可以百度/谷歌）

撰写两个 API 接口:
- 短域名存储接口：接受长域名信息，返回短域名信息
- 短域名读取接口：接受短域名信息，返回长域名信息。

限制：
- 短域名长度最大为 8 个字符
- 采用SpringBoot，集成Swagger API文档；
- JUnit编写单元测试, 使用Jacoco生成测试报告(测试报告提交截图即刻)；
- 映射数据存储在JVM内存即可，防止内存溢出；

**递交作业内容** 
- 源代码(使用gitignore过滤掉非必要的提交文件，如class文件)
- Jacoco单元测试覆盖率截图
- 设计思路以及所有做的假设(TXT即可)


## Job Description

### 岗位指责

1. 负责公司内部自用产品开发，能够独立的按产品需求进行技术方案设计和编码实现，确保安全、可扩展性、质量和性能;
2. 在负责的业务上有独立的见解和思考，对业务产品具有独立沟通、完善业务需求和识别方案风险的能力;
3. 具有持续优化、追求卓越的激情和能力，能持续关注和学习相关领域的知识，并能使用到工作当中;
4. 具备和第三方供应商进行沟通，对设计方案进行审核的能力;

### 要求

1. 5年软件研发/解决方案设计工作经验(金融领域经验加分)；
2. Java基础扎实，熟悉高级特性和类库、多线程编程以及常见框架(SpringBoot等)；
3. 具备基本系统架构能力，熟悉缓存、高可用等主流技术；
5. 持续保持技术激情，善于快速学习，注重代码质量，有良好的软件工程知识和编码规范意识；



