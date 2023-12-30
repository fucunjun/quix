# 数据存储与同步方案

- 数据中心方案（数据存储在独立的数据中心服务datacenter(拆分为多个服务, 以减轻单个LUA VM垃圾回收的压力 )中，数据加载同步请参阅entity、entitybase、dbmgr.lua dbsync.lua dcmgr.lua相关文件
- 数据直接存存储在各个模块中（通过dbproxy来加载与同步），新的方案将原本存储在数据中心的数据挪到了各个模块中，由各个模块自行负责数据的加载与备份

第二种方案与第一种方案相比，缺点在于，要取其他玩家数据，要从其他玩家的agent身上获取，第一种方案只需要从数据中心获取即可，其他玩家不在线，可以从数据中心直接加载并缓存起来

而第二种方案如果其他玩家不在线，则需要拉起agent，再通过发消息的方式往每个agent发消息获取数据，会稍微麻烦，这就是actor模式缺点，在隔离数据的同时，使得数据访问变得麻烦

解决方案，可以考虑一个进程只有一个agent，管理该进程所有玩家数据
