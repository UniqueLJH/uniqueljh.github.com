title: Mongodb Sharded Collection Balancing 相关
date: 2016-03-29 20:10:32
tags: mongodb Sharded Collection balancing
---
今天在服务器上使用mongo分片集群处理数据时,发现数据在服务器的shard上面分布不均.
服务器配置:
    mongodb版本3.2
    在shard有3个,分别部署在3个节点.
    
进入mongo, 查看数据库状态如下图
```
use admin
sh.status()
```
![enter description here][1]

  [1]: /images/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-03-29%20%E4%B8%8B%E5%8D%889.09.19.png "屏幕快照 2016-03-29 下午9.09.19.png"



仔细看了一下官方文档中对于sharded cluster, 与cluster balancer的一些细节

mongodb默认会自动平衡各个shard, 通过数据块迁移(Chunk Migration)使得shard保持均衡.触发条件就是当一个shard会比其他shard的chunk数量多很多时就会将自己的chunk向其他迁移.
具体的迁移安全线(Migration Thresholds)

| Number of Chunks | Migration Threshold |
| ---------------- | ------------------- |
| Fewer than 20    | 2                   |  
| 20-79            | 4                   |
| 80 and greater   | 8                   |

然后对于blancer功能的一些管理
```

use config
db.locks.find( { _id : "balancer" } ).pretty()
#查看锁状态, balancer在进行中的时候会向mongo.config.locks提交一个锁状态


{   "_id" : "balancer",
"process" : "mongos0.example.net:1292810611:1804289383",
  "state" : 2,
     "ts" : ObjectId("4d0f872630c42d1978be8a2e"),
   "when" : "Mon Dec 20 2010 11:41:10 GMT-0500 (EST)",
    "who" : "mongos0.example.net:1292810611:1804289383:Balancer:846930886",
    "why" : "doing balance round" }
```

创建定时balancer任务
```
db.settings.update(
   { _id: "balancer" },
   { $set: { activeWindow : { start : "<start-time>", stop : "<stop-time>" } } },
   { upsert: true }
)
# HH:MM
# HH  00-23
# MM  00-59
```
删除定时任务
```
db.settings.update(
   { _id: "balancer" },
   { $set: { activeWindow : { start : "<start-time>", stop : "<stop-time>" } } },
   { upsert: true }
)
```
关闭balancer
```
db.settings.update(
   { _id: "balancer" },
   { $set: { activeWindow : { start : "<start-time>", stop : "<stop-time>" } } },
   { upsert: true }
)
```
打开balancer
```
db.settings.update( { _id: "balancer" }, { $set : { stopped: false } } , { upsert: true } )
``