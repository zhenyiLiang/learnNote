## mongoDB学习笔记

1. mongodb副本集

   - mongodb 的副本集至少要有连个节点，其中一个是主节点，负责处理客户端的请求，其余的是从节点，负责复制主节点的数据
   - 主节点记录在其上的所有操作oplog，从节点定期轮询主节点获取这些操作，然后对自己的数据副本执行这些操作，从而保证从节点的数据与主节点一致。

   - 副本集的mongodb 的服务端的replSet 名称必须一直，否者添加副本集时报

     ```
     Our set name did not match that of localhost:27018
     ```

   - 副本集再主节点宕机后接管主节点，成为主节点，服务不会宕机

  2. mongodb 分片

     - 在多台机器上分割数据，使得数据库系统能存储和处理更多的数据

     - 创建数据库分片需要三个组件

       ```
       Shard:
       用于存储实际的数据块，实际生产环境中一个shard server角色可由几台机器组个一个replica set承担，防止主机单点故障
       
       Config Server:
       mongod实例，存储了整个 ClusterMetadata，其中包括 chunk信息。
       
       Query Routers:
       前端路由，客户端由此接入，且让整个集群看上去像单一数据库，前端应用可以透明使用。
       ```

     - 创建shard服务器时要加上--shardsvr

     -  创建config 服务器时，要使用副本集，创建主节点，并加上--configsvr

  3. todo











