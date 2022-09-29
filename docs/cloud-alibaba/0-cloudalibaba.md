<a name="agFJY"></a>

## 开发环境


- 语言：Java 8（1.8.0_271）
- IDE ：IDEA / Visual Studio Code
- 依赖管理：Maven3.6
- 数据库：MySQL 8.0.22
- 缓存：Redis 5.0
- 反向代理：Nginx1.20.1



## 服务端技术架构



![image.png](https://pub-static.stcsm.sh.gov.cn/help/1628826194223-89b2575d-ead0-4e6d-8e5f-87d1ca99cc51-20220622152850226.png)

- 基础分布式框架：
   - Spring Cloud Alibaba 2.2.5.RELEASE（框架子组件版本）
   - 使用组件：
      - Spring Cloud Gateway 2.2.6.RELEASE                   API网关中心
      - Spring Cloud OpenFeign 2.2.5.RELEASE                服务之间调用方式
      - SkyWalking                                                       链路追踪(可视化)
      - Ribbon									负载均衡
      - Sentiel									限流熔断  



- 基础框架：Spring Boot 2.3.12.RELEASE
- 服务注册、发现、配置中心：Nacos 1.4.1
- 持久层框架：Mybatis 3.5.x 、MyBatis-Plus
- 数据搜索：Elasticsearch 7.x
- 消息队列：RabbitMQ 2.4.x
- 安全框架：Spring Cloud Security + JsonWebToken 3.3 
- 缓存框架：Redis 5.0
- 日志打印：Log4j/Logback
- 其他：FastJson，EasyPoi，XXL-JOB， Lombok等

<br />
```latex
# 工程结构
├── bke-auth -- 统一鉴权中心
├── bke-common -- 常用工具包
├── bke-gateway -- （安全、监控、埋点、限流）
├── bke-ops -- 综合服务模块
├    ├── bke-develop -- 低代码平台
├    ├── bke-flowable -- 工作流
├    ├── bke-flowable_admin -- 工作流管理
├    ├── bke-smartform -- 智能表单
├    ├── bke-smartform_admin -- 智能表单管理
├    ├── bke-report -- 报表、pdf导出
├    ├── bke-xxljob -- 分布式定时任务
├    ├── bke-xxljob-admin -- 分布式定时任务管理
├    ├── bke-message-send -- 消息推送服务
├    ├── bke-setata-order -- 分布式订单服务
├── bke-service -- 综合业务模块
├    ├── bke-user -- 用户管理
├    ├── bke-system -- 系统管理
├    ├── bke-dictionary -- 数据字典服务（数据字典、分类字典、业务常量）
├    ├── bke-message -- 消息服务配置及日志管理
├── bke-tool -- 核心仓库
├    ├── bke-core-boot -- 业务包综合模块
├    ├── bke-core-launch -- 基础启动模块
├    ├── bke-core-log -- 日志封装模块 
├    ├── bke-core-mybatis -- mybatis拓展封装模块 
├    ├── bke-core-oss -- 对象存储封装模块 
├    ├── bke-core-swagger -- swagger拓展封装模块 
├    ├── bke-core-test -- 单元测试封装模块 
├    ├── bke-core-tool -- 核心工具包 
├    └── bke-core-transaction -- 分布式事物封装模块
├    └── ...
└──

```


```latex
# 单服务工程目录
bke-system
├── bke-system-main（项目名称）
│   ├── src
│   │   └── main
│   │       ├── java
│   │       │   └── com.techpower.system   
│   │       │         ├── controller           （控制层）
│   │       │         ├── mapper               （dao层）
│   │       │         ├── service               (业务层）
│   │       │         ├── wrapper               (） 
│   │       └── resources
│   │           ├── application.yml       （配置文件）
│   │           └── jwt.jks
│   ├── Dockerfile
│   ├── ci-setting.xml
│   ├── README.md
│   ├── pom.xml
│   ├── .gitlab-ci.yml
│   ├── .gitignore
│   ├── .dockerignore
│   └── bke-system-main.iml
├── bke-system-main-api（feign）
│   ├── src
│   │   └── main
│   │       ├── java
│   │       │   └── com.techpower.system   
│   │       │         ├── cache                 （缓存）
│   │       │         ├── feign                 （接口层）
│   │       │         ├── pojo                   (实体层）
│   │       │         ├── dto                    (输入）
│   │       │         ├── vo                     (输出） 
│   │       └
└── pom.xml  （POM文件）
```
