---
title: Metabase-BI系列05： 错误Unknown system variable 'session_track_schema'
date: 2019-11-29 23:30:35
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---
Metabase连接mysql5.6.16-log版本数据库报错：Unknown system variable 'session_track_schema'
<!--more-->

## 情况一：Metabase启动数据库为mysql5.6.16

启动直接报错，启动失败

Metabase在启动时报错`session_track_schema`，日志文件如下：

```
while i'm executing
export MB_DB_CONNECTION_URI="mysql://..."; java -jar metabase.jar
with mysql server version of 5.6.16
i've got this exception

05-13 18:58:09 INFO metabase.util :: Loading Metabase...
05-13 18:58:09 INFO metabase.util :: Maximum memory available to JVM: 3.4 GB
05-13 18:59:54 INFO util.encryption :: Saved credentials encryption is DISABLED for this Metabase instance. ?
For more information, see https://metabase.com/docs/latest/operations-guide/start.html#encrypting-your-database-connection-details-at-rest
05-13 19:02:25 INFO metabase.driver :: Registered abstract driver :sql ?
05-13 19:07:27 INFO metabase.core :: Starting Metabase in STANDALONE mode
05-13 19:07:30 INFO metabase.server :: Launching Embedded Jetty Webserver with config:
{:port 10080, :host "0.0.0.0"}

05-13 19:07:34 INFO metabase.core :: Starting Metabase version v0.32.7 ([`e309f28`](https://github.com/metabase/metabase/commit/e309f2875ca0ffd87da5fb1666260b0ab257860d) release-0.32.x) ...
05-13 19:07:34 INFO metabase.core :: System timezone is 'Asia/Shanghai' ...
05-13 19:07:34 INFO metabase.plugins :: Loading plugins in /home/lhladmin/metabase/plugins...
05-13 19:07:40 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :hive-like...
05-13 19:07:40 DEBUG plugins.classloader :: Setting current thread context classloader to NEWLY CREATED classloader clojure.lang.DynamicClassLoader@79ca51cb...
05-13 19:07:42 INFO metabase.driver :: Registered abstract driver :sql-jdbc (parents: :sql) ?
Load driver :sql-jdbc took 2 s
05-13 19:07:43 INFO metabase.driver :: Registered abstract driver :hive-like (parents: #{:sql-jdbc}) ?
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :sparksql...
05-13 19:07:43 INFO metabase.driver :: Registered driver :sparksql (parents: #{:hive-like}) ?
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :druid...
05-13 19:07:43 INFO metabase.driver :: Registered driver :druid ?
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :snowflake...
05-13 19:07:43 INFO metabase.driver :: Registered driver :snowflake (parents: #{:sql-jdbc}) ?
05-13 19:07:43 INFO plugins.dependencies :: Metabase cannot initialize plugin Metabase Oracle Driver due to required dependencies. Metabase requires the Oracle JDBC driver in order to connect to Oracle databases, but we can't ship it as part of Metabase due to licensing restrictions. See https://metabase.com/docs/latest/administration-guide/databases/oracle.html for more details.

05-13 19:07:43 INFO plugins.dependencies :: Metabase Oracle Driver dependency {:class oracle.jdbc.OracleDriver} satisfied? false
05-13 19:07:43 INFO plugins.dependencies :: Plugins with unsatisfied deps: ["Metabase Oracle Driver"]
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :google...
05-13 19:07:43 INFO metabase.driver :: Registered abstract driver :google ?
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :presto...
05-13 19:07:43 INFO metabase.driver :: Registered driver :presto (parents: #{:sql}) ?
05-13 19:07:43 INFO plugins.dependencies :: Metabase cannot initialize plugin Metabase Vertica Driver due to required dependencies. Metabase requires the Vertica JDBC driver in order to connect to Vertica databases, but we can't ship it as part of Metabase due to licensing restrictions. See https://metabase.com/docs/latest/administration-guide/databases/vertica.html for more details.

05-13 19:07:43 INFO plugins.dependencies :: Metabase Vertica Driver dependency {:class com.vertica.jdbc.Driver} satisfied? false
05-13 19:07:43 INFO plugins.dependencies :: Plugins with unsatisfied deps: ["Metabase Vertica Driver" "Metabase Oracle Driver"]
05-13 19:07:43 INFO plugins.dependencies :: Plugin 'Metabase BigQuery Driver' depends on plugin 'Metabase Google Drivers Shared Dependencies'05-13 19:07:43 INFO plugins.dependencies :: Metabase BigQuery Driver dependency {:plugin Metabase Google Drivers Shared Dependencies} satisfied? true
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :bigquery...
05-13 19:07:43 INFO metabase.driver :: Registered driver :bigquery (parents: #{:sql :google}) ?
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :sqlite...
05-13 19:07:43 INFO metabase.driver :: Registered driver :sqlite (parents: #{:sql-jdbc}) ?
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :sqlserver...
05-13 19:07:43 INFO metabase.driver :: Registered driver :sqlserver (parents: #{:sql-jdbc}) ?
05-13 19:07:43 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :redshift...
05-13 19:07:43 INFO metabase.driver :: Registered driver :postgres (parents: :sql-jdbc) ?
Load driver :postgres took 1 s
05-13 19:07:44 INFO metabase.driver :: Registered driver :redshift (parents: #{:postgres}) ?
05-13 19:07:44 INFO plugins.dependencies :: Plugin 'Metabase Google Analytics Driver' depends on plugin 'Metabase Google Drivers Shared Dependencies'
05-13 19:07:44 INFO plugins.dependencies :: Metabase Google Analytics Driver dependency {:plugin Metabase Google Drivers Shared Dependencies} satisfied? true
05-13 19:07:44 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :googleanalytics...
05-13 19:07:44 INFO metabase.driver :: Registered driver :googleanalytics (parents: #{:google}) ?
05-13 19:07:44 INFO plugins.lazy-loaded-driver :: Registering lazy loading driver :mongo...
05-13 19:07:44 INFO metabase.driver :: Registered driver :mongo ?
05-13 19:07:46 INFO metabase.driver :: Registered driver :mysql (parents: :sql-jdbc) ?
Load driver :mysql took 2 s
05-13 19:07:48 INFO metabase.driver :: Registered driver :h2 (parents: :sql-jdbc) ?
Load driver :h2 took 1 s
05-13 19:07:49 INFO metabase.core :: Setting up and migrating Metabase DB. Please sit tight, this may take a minute...
05-13 19:07:49 INFO metabase.db :: Verifying mysql Database Connection ...
05-13 19:07:49 INFO metabase.driver :: Initializing driver :sql...
05-13 19:07:49 INFO metabase.driver :: Initializing driver :sql-jdbc...
05-13 19:07:49 INFO metabase.driver :: Initializing driver :mysql...
05-13 19:07:52 ERROR metabase.core :: Metabase Initialization FAILED
java.lang.Exception: java.sql.SQLException: Unknown system variable 'session_track_schema'
at metabase.driver.util$can_connect_with_details_QMARK_.invokeStatic(util.clj:34)
at metabase.driver.util$can_connect_with_details_QMARK_.doInvoke(util.clj:18)
at clojure.lang.RestFn.invoke(RestFn.java:442)
at clojure.lang.Var.invoke(Var.java:393)
at metabase.db$verify_db_connection$fn__16148.invoke(db.clj:403)
at metabase.db$verify_db_connection.invokeStatic(db.clj:401)
at metabase.db$verify_db_connection.invoke(db.clj:394)
at metabase.db$verify_db_connection.invokeStatic(db.clj:397)
at metabase.db$verify_db_connection.invoke(db.clj:394)
at metabase.db$setup_db_BANG_$Missing open brace for subscriptfn__16165.invoke(db.clj:467)
at metabase.util$do_with_us_locale.invokeStatic(util.clj:676)
at metabase.util$do_with_us_locale.invoke(util.clj:662)
at metabase.db$setup_db_BANG_.invokeStatic(db.clj:466)
at metabase.db$setup_db_BANG_.doInvoke(db.clj:460)
at clojure.lang.RestFn.invoke(RestFn.java:421)
at metabase.core$init_BANG_.invokeStatic(core.clj:77)
at metabase.core$init_BANG_.invoke(core.clj:56)
at metabase.core$start_normally.invokeStatic(core.clj:123)
at metabase.core$start_normally.invoke(core.clj:117)
at metabase.core$*main.invokeStatic(core.clj:143)
at metabase.core$\*main.doInvoke(core.clj:138)
at clojure.lang.RestFn.invoke(RestFn.java:397)
at clojure.lang.AFn.applyToHelper(AFn.java:152)
at clojure.lang.RestFn.applyTo(RestFn.java:132)
at metabase.core.main(Unknown Source)
Caused by: java.util.concurrent.ExecutionException: java.sql.SQLException: Unknown system variable 'session_track_schema'
at java.util.concurrent.FutureTask.report(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
at clojure.core$deref_future.invokeStatic(core.clj:2302)
at clojure.core$future_call$Missing open brace for subscriptreify__8439.deref(core.clj:6974)
at clojure.core$deref.invokeStatic(core.clj:2324)
at clojure.core$deref.invoke(core.clj:2306)
at metabase.util$deref_with_timeout.invokeStatic(util.clj:328)
at metabase.util$deref_with_timeout.invoke(util.clj:324)
at metabase.driver.util$can_connect_with_details_QMARK\*.invokeStatic(util.clj:29)
... 24 more
Caused by: java.sql.SQLException: Unknown system variable 'session_track_schema'
at org.mariadb.jdbc.internal.util.exceptions.ExceptionMapper.get(ExceptionMapper.java:255)
at org.mariadb.jdbc.internal.util.exceptions.ExceptionMapper.getException(ExceptionMapper.java:165)
at org.mariadb.jdbc.internal.protocol.AbstractConnectProtocol.connectWithoutProxy(AbstractConnectProtocol.java:1199)
at org.mariadb.jdbc.internal.util.Utils.retrieveProxy(Utils.java:560)
at org.mariadb.jdbc.MariaDbConnection.newConnection(MariaDbConnection.java:174)
at org.mariadb.jdbc.Driver.connect(Driver.java:92)
at java.sql.DriverManager.getConnection(DriverManager.java:664)
at java.sql.DriverManager.getConnection(DriverManager.java:208)
at clojure.java.jdbc$get_driver_connection.invokeStatic(jdbc.clj:271)
at clojure.java.jdbc$get_driver_connection.invoke(jdbc.clj:250)
at clojure.java.jdbc$get_connection.invokeStatic(jdbc.clj:411)
at clojure.java.jdbc$get_connection.invoke(jdbc.clj:274)
at clojure.java.jdbc$db_query_with_resultset_STAR*.invokeStatic(jdbc.clj:1093)
at clojure.java.jdbc$db_query_with_resultset_STAR_.invoke(jdbc.clj:1075)
at clojure.java.jdbc$query.invokeStatic(jdbc.clj:1164)
at clojure.java.jdbc$query.invoke(jdbc.clj:1126)
at clojure.java.jdbc$query.invokeStatic(jdbc.clj:1142)
at clojure.java.jdbc$query.invoke(jdbc.clj:1126)
at metabase.driver.sql_jdbc.connection$can_connect_QMARK_.invokeStatic(connection.clj:123)
at metabase.driver.sql_jdbc.connection$can_connect_QMARK_.invoke(connection.clj:118)
at metabase.driver.sql_jdbc$Missing open brace for subscriptfn__62793.invokeStatic(sql_jdbc.clj:39)
at metabase.driver.sql_jdbc$fn__62793.invoke(sql_jdbc.clj:38)
at clojure.lang.MultiFn.invoke(MultiFn.java:234)
at metabase.driver.util$can_connect_with_details_QMARK_$Missing open brace for subscriptfn__18037.invoke(util.clj:30)
at clojure.core$binding_conveyor_fn$Missing open brace for subscriptfn__5739.invoke(core.clj:2030)
at clojure.lang.AFn.call(AFn.java:18)
at java.util.concurrent.FutureTask.run(FutureTask.java:266)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
at java.lang.Thread.run(Thread.java:748)
Caused by: java.sql.SQLException: Unknown system variable 'session_track_schema'
at org.mariadb.jdbc.internal.protocol.AbstractQueryProtocol.readErrorPacket(AbstractQueryProtocol.java:1587)
at org.mariadb.jdbc.internal.protocol.AbstractQueryProtocol.readPacket(AbstractQueryProtocol.java:1445)
at org.mariadb.jdbc.internal.protocol.AbstractQueryProtocol.getResult(AbstractQueryProtocol.java:1407)
at org.mariadb.jdbc.internal.protocol.AbstractConnectProtocol.additionalData(AbstractConnectProtocol.java:730)
at org.mariadb.jdbc.internal.protocol.AbstractConnectProtocol.connect(AbstractConnectProtocol.java:542)
at org.mariadb.jdbc.internal.protocol.AbstractConnectProtocol.connectWithoutProxy(AbstractConnectProtocol.java:1195)
... 27 more
05-13 19:07:52 INFO metabase.core :: Metabase Shutting Down ...
05-13 19:07:52 INFO metabase.server :: Shutting Down Embedded Jetty Webserver
05-13 19:07:53 INFO metabase.core :: Metabase Shutdown COMPLETE
```

## 情况二：添加数据源mysql5.6.16

如果数据源是这个版本也会同样的问题：

![5.6.16](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/5.6.16/metabase-mysql5.6.png)



这个问题是数据库版本导致的，你看一下详细查看Metabase github的[issue：9483]( https://github.com/metabase/metabase/issues/9483 )的讨论

关于这个问题Metabase上的issue特别的多，而且容易看晕，最后终于在找到了一个解决办法，亲测可用

最终解决方案，你可以看一下（见[issue：9954]( https://github.com/metabase/metabase/issues/9954 )）：

## 分析原因-session-track-schema

至于MariaDB是MySQL 的一个 branch，还是MariaDB 是 MySQL 的一个fork先不讨论，不过它俩似乎一直就存在兼容性的问题，Metabase MySQL/MariaDB driver是mariadb-java-client这个包。

Metabase不支持Alibaba RDS MySQL 5.6.16-log，因为在0.32.0版本中更改为MariaDB Connector/J，因为旧MySQL不支持session-track-schema。

后来发现就是这个5.6.16-log这个小版本有问题，目前其他的版本未发现存在问题。

## 解决办法-mariadb-java-client包

首先确定Metabase使用的MySQL/MariaDB driver版本，到project下面

```
[org.mariadb.jdbc/mariadb-java-client "2.3.0"]           ; MySQL/MariaDB driver
```



需要下载源码包：mariadb-java-client-2.3.0-sources.jar（Metabase当前版本用的这个版本）

```
AbstractConnectProtocol.java-542行注释掉

if (options.usePipelineAuth && !options.createDatabaseIfNotExist) {
  try {
    sendPipelineAdditionalData();
    readPipelineAdditionalData(serverData);
  } catch (SQLException sqle) {
    if ("08".equals(sqle.getSQLState())) {
      throw sqle;
    }
    //in case pipeline is not supported
    //(proxy flush socket after reading first packet)
    //解决mysql5.6.16-log版本，session_track_schema问题
    //additionalData(serverData);
    }
  } else {
    additionalData(serverData);
  }
```

编译完替换就重新启动即可，也可以直接下载我编译完的 [AbstractConnectProtocol.class](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/AbstractConnectProtocol.class)