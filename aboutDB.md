### 记录与数据库相关的知识点

## 一、mysql

- 1、mysql 的连接 `mysql -u root -p`，需要执行该命令的环境中安装 mysql 客户端。

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
