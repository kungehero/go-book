### mysql体系架构
   - [ ] server层
        - [ ] 连接层
        ```
        主要通过tcp等通信协议进行通信
        ```
        - [ ] sql层
        ```
        主要进行身份验证，语句解析，语句优化等
        ```
   - [ ] 存储引擎层
        - [ ] 内存
        ```
        innodb buffer pool:缓存池，innodb将数据文件按页读取到缓冲区，使用lru算法存储数据，数据文件修改时，先修改缓冲池的数据，然后按照一定的规律刷新到磁盘。
        redo log pool:重做日志缓存区，将重做日志信息放入这个缓冲区，然后按一定频率将其刷新到重做日志文件。
        change buffer pool：插入缓存区，主要目的是将对二级索引的数据操作缓存下来，以此减少二级索引的随机IO，并达到操作合并的效果。
        ```
        - [ ] 进程
        ```
        各个日志和缓存区的读写进程
        ```
        - [ ] 数据文件
        ```
        以页(8kb)存储数据和索引
        ```
   - [ ] 聚集索引和非聚集索引
        - [ ] 聚集索引 (以主键作为B+树索引的键值而构建的B+树索引)
        - [ ] 非聚集索引 (以主键以外的列作为B+树索引的键值而构建的B+树索引)
        ```
        非聚集索引的叶子节点不存储表中的数据，而是存储该列对应的主键，想要查找数据我们还需要根据主键再去聚集索引中进行查找，
        这个再根据聚集索引查找数据的过程，我们称为回表。
        ```
        - [ ] 主键(唯一的标识，不重复表中的某一条记录)
 ### binlog和redo log
   - [ ] 记录时间
     ```
     bin-log:sql语句提交成功之后记录日志
     redo-log:事务发起时就开始记录日志
     ```
   - [ ] 使用方式
     ```
     bin-log: 一直产生新的binlog文件
     redo-log:循环使用
     ```
   - [ ] 作用不同
     ```
     bin-log:数据恢复，主从复制
     redo-log:保证断电或crash之后能够恢复数据
     ```
   - [ ] 保证一致性
     ```
     两阶段提交的过程：
     Prepare：先写redolog buffer，然后做一个准备标记，再将log buffer 中的数据刷新到磁盘
     Commit：将事务产生的binlog写入文件，刷入磁盘。 再redolog中做一个事务提交标记，并把binlog写成功的标记也写入redolog的文件。
     只要binlog写成功，再次重启redolog就能恢复数据
     ```
### 性能调优
   - [ ] show plist
   ```
   TODO: 具体看那些字段
   ```
   - [ ] explain 
   ```
   TODO[red]:
   ```
### 索引
###  锁
### log日志
   - [ ] undo log
   ```
   保证事务原子性操作的日志，记录sql操作语句相反的语句，用于恢复操作进行数据回滚！
   ```
   - [ ] truncate和delete
   ```
   truncate 不能回滚 delete 可以回滚
   truncate 清空表的自增it属性 delete 不清空id自增属性
   ```
### 事务的隔离级别
   - [ ] 读未提交
   - [ ] 读已提交
   - [ ] 可重复读
   - [ ] 串行
   - [ ] 脏读
   - [ ] 不可重复读
   - [ ] 幻读
### 死锁
   - [ ] 什么是死锁
   - [ ] 死锁发生的条件
   - [ ] 如何避免和解决
### innoDB特性
   - [ ] 插入缓存
   - [ ] 双写缓存
   - [ ] 异步i/o
   - [ ] 刷新邻近页
   - [ ] 自适应哈希索引
 ### 慢查询日志
   - [X] long_query_time(设置超时时间)
   - [X] log_queries_not_using_indexes(没有使用索引的语句)
   - [X] mysqldumpslow工具进行查询
### bin-log 二进制文件有几种格式
   - [ ] statement
   - [ ] row
