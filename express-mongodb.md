### Express 

> 基于 Node.js 平台，快速、开放、极简的 Web （web应用 网站 ） 开发框架

express的使用方法有这么两种：

1. 直接使用express-generator工具生成一个项目模板

2. 调用express函数生成一个app应用并且编写大量中间件来进行自定义的使用


简单使用方式：

1. 可以利用express函数来创建一个app应用

2. app.listen(). 可以启动应用

3. 中间件 （middleware <http-proxy-middleware>）

    express中有一个很神奇的东西就叫中间件,顾名思义意思就是在一个完整的应用流程中去执行一些事情

    * 自定义中间件，根据请求来实现一些自定义的操作

    * 资源管理中间件  express.static('public')
    
    * 路由中间件 处理请求和响应

        可以通过app.get/post/all..方法来配置一个一个的路由，当然我们一般都会将这些路由抽离出去

        抽离出来的路由模块中利用express.Router来创建一个一个的路由中间件 ，然后在入口文件中利用app.use来使用这些中间件

        app.use('/', index) -> 当req.url 为/开头的，就去index路由中间件中进行匹配，匹配不到的话再进入下一个能匹配到的路由中间件进行匹配

        app.use('/api', api)

    *  app.use((req, res, next) => { next(); }) 可以再某一个时刻去执行一个事情（取决于写在哪里），next代表下一步动作


4. 参数问题

    参数一般有这么几种： 
    
    1. path-params  路径参数  (get/post) 

        eq: http://localhost:3000/api/goodinfo/3 其中3就是我们要得到的参数

        router: router.get('/goodinfo/:id',

        get : req.params

    2. querystring 查询字符串参数 （get/post）

        eq http://localhost:300/api/goodinfo?id=...&age=...

        router: router.get('/goodinfo',
        get : req.query

    3. form-data 表单参数  (post) 取决于请求头中content-type = 'application/x-www-form-urlencoded; charset=UTF-8'

        get : req.body 注意，需要body-parser中间件来处理

    4. request payload (get/ post) 取决于请求头中content-type = 'application/json; charset=UTF-8'

        注意，jq非得把对象转成qs，所以我们可以先转成字符串.. 

        get: req.body  注意，也需要body-parser


    cookie怎么办？ 

#### MongoDB 数据库

这是一个数据库，与MySQL(关系型数据库)的区别就是，它是一个非关系型数据库  NoSql数据库

##### 关系型数据库和非关系型数据库的区别

1.实质。    

非关系型数据库的实质：非关系型数据库产品是传统关系型数据库的功能阉割版本，通过减少用不到或很少用的功能，来大幅度提高产品性能。

2.价格。

目前基本上大部分主流的非关系型数据库都是免费的。而比较有名气的关系型数据库，比如Oracle、DB2、MSSQL是收费的。虽然Mysql免费，但它需要做很多工作才能正式用于生产。

3.功能。    

实际开发中，有很多业务需求，其实并不需要完整的关系型数据库功能，非关系型数据库的功能就足够使用了。这种情况下，使用性能更高、成本更低的非关系型数据库当然是更明智的选择。

mongodb的特点：

1. 性能高、I/O处理快
2. 速度快
3. 稳定不好，占用空间大

#### 1.安装MongoDB

下载对应版本的mongodb来进行安装，安装的后注意需要手动设置数据库的位置。。。详细请搜索

1.在安装的时候选择安装地址，一般情况下安装在c盘（默认安装地址）即可

2.我们在c盘（最好）建立一个data文件夹，在data文件夹下面再建立一个db文件夹

3.在mongodb的bin文件夹下，cmd执行 mongod.exe --dbpath c:\data\db

4.运行mongod.exe来启动mongodb

5.依然在mongodb的bin文件夹下cmd 输入mongo回车，就可以操作mongodb，例如 show databases;

> 小贴士：如果安装不上，因为有一些个dll文件缺少，要么去按照百度教的方法下载驱动精灵修复系统，要么重装系统，要么就用自己的电脑

一般情况下，使用命令行操作数据库就可以，如果不愿意请自行下载各种可视化工具：
[基于node可视化工具](https://www.cnblogs.com/shiweida/p/7692468.html)



mongoose

#### 2.MongoDB概念

MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。

它的特点:高性能、易部署、易使用，存储数据非常方便。

#### 3.专业术语

| SQL术语、概念 | MongoDB术语、概念 | 说明                           |
| ------------- | ----------------- | ------------------------------ |
| database      | database          | 数据库                         |
| table         | collection        | 表\集合                        |
| row           | doucument         | 数据记录行\文档                |
| column        | field             | 数据字段\域                    |
| index         | index             | 索引                           |
| table joins   |                   | 表连接 mongodb不支持           |
| primary key   | primary key       | 主键，mongodb自动将_id作为主键 |

database>colletions>documents

#### 4.mongodb的存储数据类似于js的json格式对象，或者json文件存储数据的方式：

```
[
    {
        "_id":ObjectId("1726iuhas678971726731"),
        "age":25,
        "city":"beijing",
        "email":"asdgakj@qq.com"
    },
    {
        "_id":ObjectId("1726iuhas678971726731"),
        "age":25,
        "city":"beijing"
    }
]
```

#### 5.数据库

一个mongodb中可以建立多个数据库。

MongoDB的默认数据库为"db"，该数据库存储在data目录中。

MongoDB的单个实例可以容纳多个独立的数据库，每一个都有自己的集合和权限，不同的数据库也放置在不同的文件中。

#### 6.简单操作

show databases 查看数据库

db 查看当前数据库

use name 切换某个数据库

#### 7.文档

文档是一个键值(key-value)对(即BSON)。

MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 MongoDB 非常突出的特点。
一个简单的文档例子如下：
[
    {"genres": ["犯罪","剧情" ],"title": "肖申克的救赎"},
    {"title":"阿甘正传",grade:"8.4","genres":"励志"}
]

#### 8.集合

集合就是 MongoDB 文档组，类似于 RDBMS （关系数据库管理系统：Relational Database Management System)中的表格。

集合存在于数据库中，集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。

```
{
    'title':['aaa',"aaa","aaa",["a":{}]]
},
{
    'title':'bbb'
}
```

#### 9.数据类型

String : 这是最常用的数据类型来存储数据。在MongoDB中的字符串必须是有效的UTF-8。

Integer : 这种类型是用来存储一个数值。整数可以是32位或64位，这取决于您的服务器。

Boolean : 此类型用于存储一个布尔值 (true/ false) 。

Double : 这种类型是用来存储浮点值。

Min/ Max keys : 这种类型被用来对BSON元素的最低和最高值比较。

Arrays : 使用此类型的数组或列表或多个值存储到一个键。

Timestamp : 时间戳。这可以方便记录时的文件已被修改或添加。

Object : 此数据类型用于嵌入式的文件。

Null : 这种类型是用来存储一个Null值。

Symbol : 此数据类型用于字符串相同，但它通常是保留给特定符号类型的语言使用。

Date : 此数据类型用于存储当前日期或时间的UNIX时间格式。可以指定自己的日期和时间，日期和年，月，日到创建对象。

Object ID : 此数据类型用于存储文档的ID。

Binary data : 此数据类型用于存储二进制数据。

Code : 此数据类型用于存储到文档中的JavaScript代码。

Regular expression : 此数据类型用于存储正则表达式



#### 10.库的操作

- Help查看命令提示

help

db.help()

db.test.help()

db.test.find().help()

- 创建/切换数据库

use music

- 查询数据库

show dbs 空库将不会显示

db.albums.insertOne({'title':'bey bey'})来插入一条后再看

- 查看当前使用的数据库

db/db.getName()

- 显示当前DB状态

db.stats()

- 查看当前DB版本

db.version()

- 查看当前DB的链接机器地址

db.getMongo()

- 删除数据库

db.dropDatabase()

#### Collection聚集集合操作

创建一个聚集集合

db.createCollection("collName", {size（集合大小）: 20, capped（固定大小，可提高使用效率）: true, max（最大值）: 100});

db.collName.isCapped(); //判断集合是否为定容量

得到指定名称的聚集集合

db.getCollection("account");

得到当前db的所有聚集集合

db.getCollectionNames();

显示当前db所有聚集的状态

db.printCollectionStats();

### 添加、修改与删除集合数据

查看

db.users.find()

添加

db.users.save({name: ‘zhangsan', age: 25, sex: true});

db.users.insertOne({name: ‘zhangsan', age: 25, sex: true});

db.users.insertMany([{name: ‘zhangsan', age: 25, sex: true},{name: ‘zhangsan', age: 25, sex: true}]);

修改



db.users.update({age: 25}(约定条件，全部修改只写{}), {$set: {name: 'changeName',sex:1}}, false, true);

第三个参数为，如果没有这个数据，会不会创建，第四个参数为，如果有很多，是要全改true，还是只改第一条

相当于：update users set name = ‘changeName' where age = 25;

db.users.update({name: 'Lisi'}, {$inc: {age: 50}}, false, true);

相当于：update users set age = age + 50 where name = ‘Lisi';

db.users.update({name: 'Lisi'}, {$inc: {age: 50}, $set: {name: 'hoho'}}, false, true);

相当于：update users set age = age + 50, name = ‘hoho' where name = ‘Lisi';

删除

db.users.remove({age: 32});符合条件全删
db.users.remove({age: 132}，{justone:true});只删一条
db.users.remove({});删除所有document

查询修改删除

```
db.users.findAndModify({
    query: {age: {$gte: 25}}, 
    sort: {age: -1}, 
    update: {$set: {name: 'a2'}, $inc: {age: 2}},
    remove: true
});

db.runCommand({ findandmodify : "users", 
    query: {age: {$gte: 25}}, 
    sort: {age: -1}, 
    update: {$set: {name: 'a2'}, $inc: {age: 2}},
    remove: true
});
```

query 过滤条件 $gte大于

sort如果多个文档符合查询过滤条件，将以该参数指定的排列方式选择出排在首位的对象，该对象将被操作，-1位为降序

remove 若为true，被选中对象将在返回前被删除

update 一个 修改器对象

remove 创建新对象若查询结果为空



#### 聚集集合查询

查询所有记录

db.userInfo.find();

相当于：select* from userInfo;

查询去重后数据

db.userInfo.distinct("name");

相当于：select distict name from userInfo;

查询age = 22的记录

db.userInfo.find({"age": 22});

相当于： select * from userInfo where age = 22;

查询age > 22的记录

db.userInfo.find({age: {$gt: 22}});

相当于：select * from userInfo where age >22;

查询age < 22的记录

db.userInfo.find({age: {$lt: 22}});

相当于：select * from userInfo where age <22;

查询age >= 25的记录

db.userInfo.find({age: {$gte: 25}});

相当于：select * from userInfo where age >= 25;

查询age <= 25的记录

db.userInfo.find({age: {$lte: 25}});

查询age >= 23 并且 age <= 26

db.userInfo.find({age: {$gte: 23, $lte: 26}});

查询name中包含 mongo的数据

db.userInfo.find({name: /^mongo/});

//相当于%%
select * from userInfo where name like ‘%mongo%’;

查询name中以mongo开头的

db.userInfo.find({name: /^mongo/});

相当于select * from userInfo where name like ‘mongo%’;

查询指定列name、age数据

db.userInfo.find({}, {name: 1, age: 1});

相当于：select name, age from userInfo;

查询指定列name、age数据, age > 25

db.userInfo.find({age: {$gt: 25}}, {name: 1, age: 1});

相当于：select name, age from userInfo where age >25;

按照年龄排序排序

升序：db.userInfo.find().sort({age: 1});

降序：db.userInfo.find().sort({age: -1});

查询name = zhangsan, age = 22的数据

db.userInfo.find({name: 'zhangsan', age: 22});

相当于：select * from userInfo where name = ‘zhangsan' and age = 
’22';

查询前5条数据

db.userInfo.find().limit(5);

相当于：select top 5 * from userInfo;



查询10条以后的数据

db.userInfo.find().skip(10);

相当于：select * from userInfo where id not in (
   select top 10 * from userInfo
);

查询在5-10之间的数据

db.userInfo.find().limit(10).skip(5);

or与 查询

db.userInfo.find({$or: [{age: 22}, {age: 25}]});

相当于：select * from userInfo where age = 22 or age = 25;

查询第一条数据

db.userInfo.findOne();

相当于：selecttop 1 * from userInfo;

db.userInfo.find().limit(1);

查询某个结果集的记录条数

db.userInfo.find({age: {$gte: 25}}).count();

相当于：select count(*) from userInfo where age >= 20;

