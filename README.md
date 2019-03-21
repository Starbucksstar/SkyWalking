# SkyWalking
分布式链路追踪系统(APM)

### 1.背景
> 随着微服务架构的流行，基于dubbo和spring cloud等框架选用开始成为大小企业构建后端微服务的利剑。但是并不是微服务拆分的越小越好或者横向扩展越多越好，微服务内部集群难免会存在某台机器故障或者连接数据库异常等，导致该机器所在服务成为整个架构的性能瓶颈，所以需要对微服务工程无代码侵入、性能良好、便于集成的框架，可以帮助理解系统行为、用于分析性能问题的工具，以便发生故障的时候，能够快速定位和解决问题，这就是APM(application performance management)，常用工具：Cat、Zipkin、Pinpoint、SkyWalking。这里我们选用SkyWalking（国产）。	随着微服务架构的流行，基于dubbo和spring cloud等框架选用开始成为大小企业构建后端微服务的利剑。但是并不是微服务拆分的越小越好或者横向扩展越多越好，微服务内部集群难免会存在某台机器故障或者连接数据库异常等，导致该机器所在服务成为整个架构的性能瓶颈，所以需要对微服务工程无代码侵入、性能良好、便于集成的框架，可以帮助理解系统行为、用于分析性能问题的工具，以便发生故障的时候，能够快速定位和解决问题，这就是APM(application performance management)，常用工具：Cat、Zipkin、Pinpoint、SkyWalking。这里我们选用SkyWalking（国产）。
### 2.SkyWalking
	1.架构图：
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/architecture.png)

	2.功能：分布式追踪、性能指标分析、应用和服务依赖分析功能
	
	3.过程简介：
		agent模块：将微服务中接口、服务、数据库、MQ等进行追踪，将追踪结果通过http或gRPC发送给collector。
		collector模块：通过分析和聚合将结果存储到H2/ElasticSearch/Mysql等中。
		SkyWalking UI模块：可视化展示，UI通过GraphQL + HTTP方式去查找数据。
		
	4.尝鲜图：
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/apm.jpg)

### 3.动手尝鲜（Windows系统）
	1.下载SkyWalking：(6.0.0版本真香)
	地址：http://www.apache.org/dyn/closer.cgi/incubator/skywalking/6.0.0-GA/apache-skywalking-apm-incubating-6.0.0-GA.zip
	
	2.下载安装Elasticsearch(自行百度)：默认监听本地ip，tcp端口9200，transport端口：9300.可以安装es-head插件快速查看es数据，如下图：
	
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/es.jpg)
	
	3.下载安装Mysql
	4.完成2,3步骤后，分别启动之
	

	5.开始安装和配置SkyWalking：
	  1.解压步骤1的zip包：如下图
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/TIM%E6%88%AA%E5%9B%BE20190321155244.jpg)

	  2.进入config/application.yml,修改storage，改用elasticsearch存储收集结果，配置好clusterNodes。
	  
	  3.进入bin目录，startUp.bat一键启动SkyWalking(启动前es必须启动)，如下图：
	  
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/skywalking-service.jpg)  

	  4.新建两个springboot工程，新建A工程--sky接口，B工程--agent接口。接口功能：sky接口调用agent接口，agent接口查询mysql数据库。
	  
	  5.使用maven打包两个工程
	  
	  6.使用命令 java -javaagent:skywalking安装目录\agent\skywalking-agent.jar -jar A.jar启动两个工程，如下图：
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/service2.jpg)

	  7.然后进localhost:8080查看Skywalking UI管理页面，如下图所示
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/dashboard.jpg)
	  
	  8.通过postman调用sky接口，在ui后台查看链路调用情况，如下图。
![](https://github.com/Starbucksstar/SkyWalking/blob/master/img/trace.jpg)
	  
	
