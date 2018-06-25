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
mysqldump -u用户名 -p密码 -d 数据库名 表名 > 导出文件名

# 导出数据库所有表结构，加-d
mysqldump -u用户名 -p密码 -d 数据库名 > 导出文件名

# 导出表结构和数据
mysqldump -u用户名 -p密码 数据库名 表名 > 导出文件名

# 导出数据库所有表和数据
mysqldump -u用户名 -p密码 数据库名 > 导出文件名

# 如果要导出多个表，则在数据库表名后填写表名列表
mysqldump -u用户名 -p密码 数据库名 表名1 表名2 表名3 > 导出文件名
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



### 数据库用户授权

```shell
GRANT ALL PRIVILEGES ON `video_buddy_config`.* TO 'watchnow'@'%'
```

