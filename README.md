# mybatis-cache-enhanced-1.0.0
mybatis-enhanced-cache 简 介

MyBatis Enhanced Cache, Control your Caches precisely! 该插件主要是为了弥补MyBatis二级缓存控制上的不足，提高二级缓存Cache和数据库数据的同步性和一致性，处理各个Cache之间的关联关系。 该插件可以精确地管理MyBatis的二级缓存，实现对MyBatis二级缓存细粒度的控制。 当执行过对数据库表的更新操作(update、delete、insert)时，可以指定清除由特定的StatementId表示的查询语句产生的缓存。

应 用 场 景：

当前的MyBaits对于缓存的比较粗糙，一般为一个Mapper配置一个Cache缓存，或者多个Mapper共用一个缓存。 而对缓存的维护都是独立的，缓存之间不会相互影响，指定的Mapper中的语句只会影响到该Mapper对应的Cache缓存。 有时候希望当执行某些更新操作时，能够刷新或者清空特定的查询语句产生的缓存，以避免数据不一致的情况。

举例

现有AMapper.xml 中定义了对数据库表ATable的CRUD操作，BMapper定义了对数据库表BTable的CRUD操作； 假设MyBatis的二级缓存开启，并且AMapper中使用了二级缓存，AMapper对应的二级缓存为ACache； 除此之外，AMapper中还定义了一个跟BTable有关的查询语句，类似如下所述：

select * from ATable left join BTable on .... 执行了selectATableWithJoin操作，该查询的缓存会被放置到对应的二级缓存ACache中， 当再次执行相同的查询，则直接从二级缓存ACache中； 如果某个时候，BMapper中执行了对BTable的update操作(update 、delete、insert),BTable 的数据已经更新， 再次执行selectATableWithJoin操作，该查询的结果直接从二级缓存ACache中取，这时候就造成了数据不一致的情况 针对上述的问题，需要解决： 当BMapper执行对BTable的update操作时，指定刷新 ACache中的 selectATableWithJoin语句产生的缓存 该插件正是解决上述的这一问题！

使用方法：

使用此插件非常简单：

在mybatisConfig.xml 文件中定义plugin节点如下： dependencys.xml配置文件，配置StatemntId依赖关系

节点配置更新语句和查询语句的依赖关系，如果id表示的更新语句执行了，会清空由配置的id表示的查询语句生成的缓存。 节点表示当父节点 id表示的更新语句执行后，应该清除此语句所产生的缓存
