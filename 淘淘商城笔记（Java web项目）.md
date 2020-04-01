# 淘淘商城笔记（Java web项目）

## 项目分析

1. 网站用户越来越大，单纯增加服务器不发满足需求，配置tomcat 集群（集群方式）。 通过提高硬件性能，加大网络带宽。
2. 前台并发量较大，后台只有操作人员使用
3. 首页访问量比下订单需求更大， 压力最大的模块为商品浏览的模块。 但是两个模块需要进行交互，webservice。

#### 旧式项目的问题：

1. 模块之间耦合度太大，其中一个升级，其他的都得升级
2. 开发困难，各个团队都需要结合在一起
3. 系统的拓展性差
4. 不能灵活进行分布式部署

###### 解决方法

优点：

将模块拆分成独立的工程，当某模块压力太大，可以对一个项目增加配置

缺点：

系统之间的交互需要额外的工作量

*把系统拆分成多个工程，要完成的工程需要多个工程协作完成，这种方式叫做分布式*

#### 分布式系统架构

###### 优点：

1. 把模块拆分开， 使用接口通讯，降低模块之间的耦合度
2. 将项目拆分成若干个子项目，不同团队负责不同的子项目
3. 增加功能时只需要在呢个价一个子项目，使用其他系统的接口就可以实现
4. 可以灵活地进行分布式部署

###### 缺点：

系统之间交互需要使用远程通讯，接口开发增加工作量



[《淘宝这十年》]: https://pan.baidu.com/s/1vPQwG31YkSz9cq-xN7RnDg

密码:6fpl       

[^为什么探求分布式系统]: 



## 项目使用的技术

- Spring, SpringMVC， Mybatis( Hibernate 将表映射为对象，Mybatis 面向sql语句，更加灵活)
- JSP, JSTL, Jquery, Jquery plugin,  EasyUI， KindEditor(富文本编译器),  Css + DIV
- Solr(搜索)
- httpclient (调用系统服务)
- Mysql
- Nginx(反向代理工具)



###### Maven 的好处

- 依赖管理， jar包， 工程之间的依赖
- 项目构建

Maven 工程类型

- war 包工程（web）

- jar 包工程（Java）
- Pom工程(聚合工程)

###### *为了统一jar 包， 需要定义父工程。 父工程中定义jar包的版本信息。Maven 插件版本信息*

### 创建maven 工程

- poem 工程(父工程)

  - 修改poem 文件

    配置整个公司jar包的版本信息

    dependancyManagement 只定义版本

    dependency 定义jar 包

    druid 连接池（alibaba开发）

- 通用的工具类

  common 工程： 放入所有的通用的工具类，打成jar 包，供所有工程使用

  - 所有的工程都需要继承父工程 

- 后台管理工程
  搭建一个聚合工程（poem）

  此聚合工程依赖common 工程

  此聚合工程下面有4个子工程：

  1. mapper（jar）
  2. pojo（jar）
  3. service(jar)
  4. web(war)

  pom.xml 会发生报错，原因是缺少web.xml。web工程必须要有一个web.xml

  *解决方法，在web 项目中的webapp新建WEB-INF文件夹，并新建web.xml文件*

### 工程测试

使用maven.tomcat 插件

- 新建欢迎页

- 配置插件

  在webapp里面新建jsp 文件 index.jsp. 此时web 工程不完整，需要运行聚合工程，才能使web工程运行

  *要运行工程，需要使用tomcat 插件。tomcat 插件是一个maven 插件，所以需要在pom.xml 文件中添加build 结点，并进行配置插件。*

- 把parent 工程，common工程安装到本地仓库

- 将项目上传到SVN服务上

  [Git VS SVN]: https://www.cnblogs.com/mtl-key/p/6902666.html

  

  SVN的三个文件夹

  1. Branch 在里面编写不会影响主干
  2. tags 标签
  3. trunk 主干

  SVN项目上传

  右键项目--> Team-->share project -->  SVN--> next ....

### 创建数据库

使用mysql 数据库

​	*在互联网行业， 尽可能减少表和表之间的关联，使用储存冗余数据，保证性能，实行单表查询。此外，单表查询还有利于分库分表*

###  逆向工程

mybatis 的逆向工程，根据数据库表生成java代码

###### mybatis 如何进行逆向工程

1. 导入生成逆向工程的代码
2. 修改lib文件夹里面generatorConfig.xml 配置文件
3. 指定PO类生成位置
4. 选择要生成的表

### SSM框架整合

#### Dao 层

使用mybatis 框架。 创建sqMapConfig.xml文件，不用配置数据库连接，直接给Spring 管理。

创建一个applicationContext-dao.xml

1. 配置数据源
2. 需要让spring管理容器管理sqlsessionfactory，单例存在
3. 把mapper 的代理对象放到spring 容器中。使用扫描包的方式加载到mapper 的代理对象中

#### Service 层

1. 事务管理
2. 需要把service 实现类对象放在SPring 容器中管理

####  表现层

1. 配置注解驱动
2. 配置视图解析器
3. 需要扫描controller

#### web.xml 

1. spring 容器的配置
2. springmvc前端控制器的配置
3. post乱码过滤器















