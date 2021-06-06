### 记录与数据库相关的知识点

## 一、mysql

- 1、mysql 的连接 `mysql -u root -p`，需要执行该命令的环境中安装 mysql 客户端。

- 2、利用docker安装mysql
```javascript
  sudo docker run -p 3306:3306 --name mysql \
  -v /usr/local/docker/mysql/conf:/etc/mysql \
  -v /usr/local/docker/mysql/logs:/var/log/mysql \
  -v /usr/local/docker/mysql/data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -d mysql:5.7
```
- 3、修改字段的数据类型`alter table tableName MODIFY fieldName VARCHAR(10);`
- 4、按照时间查询：日期相隔一天`SELECT * FROM forest_fire_points_details where TO_DAYS(NOW()) - TO_DAYS(monitorTime) = 1 order by monitorTime desc;`；时间相差24小时`SELECT * FROM forest_fire_points_details where monitorTime >=(NOW() - interval 24 hour) order by monitorTime desc`
- 5、修改时区
```javascript
    > set global time_zone = '+8:00'; ##修改mysql全局时区为北京时间，即我们所在的东8区
    > set time_zone = '+8:00'; ##修改当前会话时区
    > flush privileges; #立即生效
```
- 6、导出数据库`mysqldump -u 用户名 -p 数据库名 > 导出的文件名`
- 7、导出数据表`mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名`
---

## 二、mongodb

- 1、mongodb 连接远程服务器：`mongo 10.249.7.251:27017/bhqd -u bh -p`
- 2、mongodb 分页查询，有两种方法：
  > - 1)**通过 limit 以及 skip 俩个方法**，如`db.users.find().skip(10).limit(10)`该语句为查询 users 表中的数据，略过前十个数据，获取不大于十个数据，该方法在小数据量可以 有很好表现，当数据量增大时，查询速度会很慢，因为他会略过很多条数据。
  > - 2)**通过`_id`索引实现**:`_id`字段是默认索引的，如果要获取下一页，需要查找`_id`大于上一页的最后一条`_id`，然后 limit，这样利用索引查询速度快，但是不能跳页查询。
- 3、**分组并统计**将数据按照`img_name`字段进行分组，并且将`Class`字段累积放在`mark_class`数组中
  ```javascript
  db.marks.aggregate([
    { $group: { _id: "$img_name", mark_class: { $addToSet: "$Class" } } },
  ]);
  ```
- 4、复杂条件查询，查询 state 为 2，然后按照 img_name 分组，然后统计分组的个数。$match用来过滤数据；$group 用来进行分组，\$sum 为统计分组中的数据个数。
  ```javascript
  db.marks.aggregate([
    { $match: { state: 2 } },
    { $group: { _id: "$img_name", mark_num: { $sum: 1 } } },
  ]);
  ```
- 5、**联表查询**：mongodb 两个表 join，from 为 join 的表名称，localField 为本表关联字段，foreignField 为 join 表的字段，as 为将关联的表变为字段。\$match 为进一步过滤,\$project 为限制返回字段。
  ```javascript
  db.orders.aggregate([
    {
      $lookup: {
        from: "inventory",
        localField: "item",
        foreignField: "sku",
        as: "inventory_docs",
      },
    },
    { $match: { "Class.Level1": "test" } },
    { $project: { geom: 0, "imgData.mark_class": 0 } },
  ]);
  ```
- 6、**创建 mongodb 用户**:创建 mongodb 容器`docker run -itd --name bhMongo1 -p 27017:27017 -v $PWD/db:/data/db mongo --auth`其中`--auth`为需要认证登录才能操作数据库。需要认证登录时需要在 admin 数据库中`use admin`创建一个管理员用户`db.createUser({user:'root',pwd:'root',roles:[{role:'userAdminAnyDatabase', db:"admin"}]})`,然后在新的数据库中`use bhqd`创建一个用户`db.createUser({user:'root',pwd:'root',roles:['readWrite']})`，该用户只可读写该数据库。然后重新登录数据库，认证一下新的用户名，即可操作数据库。mongodb 数据库文件放在 nas 中存储，可以另起一个 mongodb 容器挂载该数据目录，里面数据用户名仍然存在。
- 7、**mongodb 条件查询**：
  `table_connect.find({"img_name": img_name, "$or": [{"editor_": user_name}, {"state_": 2}, {"state_": 4}]})`
- 8、**新增字段**：`db.test.update({},{$set:{content:""}},{multi:1})`
- 9、**删除字段**：`db.test.update({},{$unset:{uname:""}},false,true)`
- 10、`javascript 大于：db.collection.find({ "field" : { $gt: value } } ); 小于：db.collection.find({ "field" : { $lt: value } } ); 大于等于：db.collection.find({ "field" : { $gte: value } } ); 小于等于：db.collection.find({ "field" : { $lte: value } } ); 查询指定字段：db.collection.find( {}, { id: 1, title: 1 } )`
- 11、**更新文档**： `db.col.update({'title':'xxx'},{$set:{'title':'MongoDB'}},{multi:true})`
- 12、**替换文档**：如果\_id 存在替换之前的文档，如果不存在则插入

```javascript
b.col.save({
  _id: ObjectId("56064f89ade2f21f36b03136"),
  title: "MongoDB",
  description: "MongoDB 是一个 Nosql 数据库",
  by: "Runoob",
  url: "http://www.runoob.com",
  tags: ["mongodb", "NoSQL"],
  likes: 110,
});
```

- 13、**创建表**：`db.createCollection('collectionName'); `
- 14、**创建索引**：`db.chongqing.createIndex({"content.title":1, "content.judgementType":1, "content.caseType":1},{background:true})`
- 15、**查看索引**： `db.chongqing.getIndexes()`

---

## 三、postgresql

- 1、postgresql 连接数据库`psql -h localhost -U postgres xxx(数据库)`，localhost 为数据库服务的 ip，postgres 为数据库的用户名。
- 2、postgresql 创建表，mysql 相类似：
  ```javascript
    create table tree_user (
      id serial primary key not null,
      name varchar(30) not null,
      passwd varchar(30) not null,
      authority int not null
    );
  ```
- 3、postgresql 查看所有的数据库 `select pg_database.datname, pg_database_size(pg_database.datname) AS size from pg_database;`
- 4、postgresql 切换数据库`\c tree_manage`
- 5、postgresql 显示当前数据库下所有的表格`\d`
- 6、进入数据库，首先切换用户：`su postgres`, 然后进入数据库的 cli：`psql`
- 7、postgis 导入 shp 文件到 postgresql 需要在 postgres 用户下：`shp2pgsql -s SRID SHAPEFILE.shp SCHEMA.TABLE | psql -h HOST -d tree_manage -U postgres`，其中-s 为导入的 shp 文件坐标，TABLE 生成的表格；-d 之后的为数据库
