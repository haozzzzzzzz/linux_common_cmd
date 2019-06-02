# mysql命令

## 查看结构

```shell
desc table_name;
```



## 查看表DDL

```shell
show create table table_name;
```



## 查看索引

```shell
show index from table_name;
show keys from table_name;
```



## 导出数据

```shell
# 导出表结构，加-d
mysqldump -h 数据库链接 -u用户名 -p密码 -d 数据库名 表名 > 导出文件名

# 导出数据库所有表结构，加-d
mysqldump -h 数据库链接 -u用户名 -p密码 -d 数据库名 > 导出文件名

# 导出表结构和数据
mysqldump -h 数据库链接 -u用户名 -p密码 数据库名 表名 > 导出文件名

# 导出表接口和部分数据
mysqldump -h 数据库链接 -u用户名 -p密码 数据库名 表名 --where="sql条件" > 导出文件名

# 导出数据库所有表和数据
mysqldump -h 数据库链接 -u用户名 -p密码 数据库名 > 导出文件名

# 如果要导出多个表，则在数据库表名后填写表名列表
mysqldump -h 数据库链接 -u用户名 -p密码 数据库名 表名1 表名2 表名3 > 导出文件名
```



## 查询导出

https://www.cnblogs.com/coderland/p/5902971.html

```mysql
SELECT ... FROM TABLE_A
INTO OUTFILE "/path/to/file"
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n';
```






## 导入数据

```shell
# 进入mysql命令行
source 脚本目录
```



## 表重命名

```shell
ALERT TABLE table_name RENAME TO new_table_name;
```



### 查看连接数、状态、最大并发数

```shell
# 显示状态
show status

# 查看连接数
show status like 'Threads%'

# 当前正在执行的mysql链接
show processlist

# 查看配置
show variables

# 查看最大连接数
show variables like '%max_connections%'
```



```shell
# 设置最大连接数
set GLOBAL max_connections=连接数;
flush privilegesl;

# 或者修改/etc/my.cnf重的max_connections
```



### 创建数据库

```mysql
CREATE DATABASE <database_name> DEFAULT CHARACTER SET <charset_name> COLLATE <collation_name>

CREATE DATABASE <database_name> DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
```



### 创建用户

```mysql
create user 'test'@'localhost' identified by '1234' -- 如果允许远程用户则将localhost改成%
```

- https://blog.csdn.net/u011120248/article/details/79983250

### 数据库用户授权

```shell
# 查看授权
SHOW GRANTS FOR <用户名>

# 授权。所有权限
GRANT ALL PRIVILEGES ON `video_buddy_config`.* TO 'watchnow'@'%'

# 只读权限
GRANT SELECT ON `video_buddy_config`.* TO 'watchnow'@'%'
```

- https://blog.csdn.net/anzhen0429/article/details/78296814



### 查询表是否被锁

https://blog.csdn.net/szxiaohe/article/details/79247672

https://blog.csdn.net/yucaifu1989/article/details/79400446



查看死锁sql语句，分析索引情况。

```mysql
show engine innodb status\G
```



查看死锁占用时间长的sql语句。

```mysql
show status like '%lock%'
```





1. 查看表是否被锁。

   ```mysql
   show OPEN TABLES where In_use>0;
   ```

   这个语句记录当前锁表状态。

2. 查看进程

   ```mysql
   show processlist
   show full processlist
   ```

3. 查询事务

   ```mysql
   # 查看正在锁的事物
   SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;
   
   # 查看等待锁的事务
   SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;
   ```

4. 查看死锁状态

   ```sh
   show full processlist;
   show engine innodb status\G;
   USE INFORMATION_SCHEMA
   select * from information_schema.INNODB_TRX;
   
   mysql> kill <thread_id>
   ```

5. s

   