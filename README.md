# JT-progra
## 京淘项目

15年12月 视频

正在跟视频更新源码中。。。
跑项目目前请用maven来跑，
配置位置：
eclipse-->Run-->Run Configurations...-->Maven Build
中新建一个Maven Build
Main页签：
Base directory 选中jt-manage-web工程
Goals 填写 tomcat7:run
Source页签：
Add 加入全部工程
其他一般都用默认的，jre用自己的

1.京淘架构

学习方法：
思想，业务，表设计，代码

特点：
技术新
技术范围广，6个大点
分布式
高并发、集群、负载均衡、高可用
海量数据
业务复杂
系统安全

软件分析：
nginx 负载均衡 自动分配服务器
tomcat 负载最大大概150-200/秒
weblogic 负载200-500/秒
apache 
动静分离web服务器 常用PHP
可被nginx替代，是apache中间件100倍，
c写的，Linux epoll技术

集群，单点故障，
高可用 zookeeper
分布式，高并发

Maven继承，聚合
mysql主从复制，读写分离



第一天：
1）京淘电商，互联网电子商城，类似京东、淘宝大型电商网站。
主要业务就包括：
用户在商城浏览商品，看中的商品加入购物车，提交订单。
-后台系统
（管理商品、维护商品分类、维护商品信息、
内容管理、用户管理），
-前台系统
（展示商品分类分为三大类、展示商品分页列表、
展示商品详细的信息、商品规格管理），
-购物车
（加载商品到购物车、将用户选择的数据存放到cookie中、
提交订单时合并cookie到数据库中这个用户的购物车中），
-订单系统
-物流系统
-sso单点登录 + redis

2）Maven 继承、聚合
在大型项目中就将项目分割成多个子项目，maven直接支持。
例如：
jt-parent POM 项目，父项目，管理整个京淘项目的所有的jar的依赖
jt-common JAR 项目，工具类的集合，继承 jt-parent
jt-manage POM 项目，聚合项目，在聚合项目中增加moudles标签，
里面加载所有的聚合的子项目，项目是有顺序性
jt-manage-pojo 持久化类 + JPA注解，
实现数据库表和字段对应 PO 的属性，
在类上加 @Table 标识类对应的是哪个数据库的表；
在主键上加 @Id 主键标识；
在主键上加 @GeneratedValue(strategy=GenerationType.IDENTITY)，
自增，前提是设计数据库表必须支持自增（mysql、sqlserver）,
Oracle序列需要单独配置；
在数据库字段和属性不一致的情况下，加 @Column 标识对应关系
jt-manage-mapper 接口文件，
必须继承SysMapper，通用Mapper第三方组件，
封装该组件，扩展CRUD，扩展了一个批量删除根据IDS，
充当DAO层，利用注解可注入到service层，
只封装单表CRUD操作（好处不用映射），特殊方法需自行扩展，
为了好扩展，习惯性写好接口
jt-manage-service 业务层，业务层直接注入mybatis接口
jt-manage-web web项目 后台系统的前台展现，
必须有资源文件，必须有静态文件

Maven 项目管理
maven多个子项目分别部署，开发时非常适合团队开发，
每个子项目高度聚合，可以单独发布到本地仓库，
其他的项目依赖即可。传递依赖。可以轻松管理大型项目。

SVN 版本控制
冲突：
1）提供多个版本的比较，自动显示文件中的差异
2）可以对文件进行加锁，检出的加锁文件不能提交
（核心配置文件常用加锁）

商品分类表设计：
1）主键类型考量
UUID易于合并和导出
速度：int>long>uuid
电商：数据非常 量大，操作非常集中，并发压力大，
采用长整型，(mysql上亿效率明显降低)，
上千万后一般会分表（保持当前表数据量在近百万）
2）对查询条件中常用的字段进行索引（查询），
大量查询时就非常块。
3）表都成为单表。
超大型项目，没有表之间关联，无数据库约束，
将表直接的关系都变成代码维护关系，
POJO的关系要根据当前的业务操作，
还是要建立对象之间的关系，不要太复杂，最多4层，
利用mybatisSQL的映射，
查询多个表的数据，将数据映射到各个层次的对象中。

EasyUI控件
了解基本的用法，山寨，错误学习法
布局：
class属性控制，
jsp解析后，执行js，easyUI渲染render，加工标签，menu菜单/tabs页夹
tree 数据动态：
从数据库来组织数据，
绑定按钮点击事件，弹框窗口，初始化tree，
发送ajax请求，自动带id（父id），第一次为空，后台默认为0
响应固定json字符串（传输中会序列化成文本流），tree自动解析json，
json带有的特殊字段，需要自行在POJO添加get方法。

easyUI组件 + 后台准备数据形成json格式 + 前台ajax提交

springmvc：
1）RESTFul访问形式 
http://localhost:8081/item/cat/list/100 
在 controller @RequestMapping中要写占位符{name}，
在方法的形参上接收的注解 @PathVariable 标识
2）在方法上增加 @ResponseBody，
springmvc会自动将返回的java对象转换为json串。






