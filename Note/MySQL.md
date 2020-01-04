[TOC]

# MySQL
[教程](https://www.runoob.com/mysql/mysql-tutorial.html)
## 连接

```
mysql -u root -p
```
## 数据库
### 查看所有

```
show databases;
```
### 创建

```
CREATE DATABASE <数据库名>;
```

使用 mysqladmin 创建数据库，用户需要有创建、删除数据库的权限
```
mysqladmin -u root -p create <数据库名>
```

### 删除

```
drop database <数据库名>;
```

mysqladmin命令
```
mysqladmin -u root -p drop <数据库名>
```

### 选择

```
use <数据库名>
```

## 表
### 查看所有的表

```
select table_name from information_schema.tables where table_schema='database_name' and table_type='base table';
```

```
show tables;
```

### 创建

```
CREATE TABLE table_name (column_name column_type);
```

例子
```
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
**_注意_** ` 是键盘左上角的，跟~一个键

### 删除

```
DROP TABLE table_name ;
```

### 插入

```
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );
```

### 查询

```
SELECT column_name,column_name
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
```
### 查看列

```
desc 表名;
```
### 修改

```
ALTER TABLE：添加，修改，删除表的列，约束等表的定义。
```

* 修改表名字

```
alter table 表名 rename to 新表名;
```
* 增加列

```
alter table 表名 add column 列名 varchar(30);
```
* 删除列

```
alter table 表名 drop column 列名;
```
* 修改列名

```
alter table 表名 change 列名 新列名 int; 
```
* 修改列属性

```
alter table 表名 modify 列名 varchar(22); 
```
* 添加主键

```
alter table 表名 add constraint 主键（形如：PK_表名） primary key 表名(主键字段); 
例：

```
* 添加外键

```
alter table 从表 add constraint 外键（形如：FK_从表_主表） foreign key 从表(外键字段) references 主表(主键字段);
```
* 删除主键

```
alter table 表名 drop primary key;
```
* 删除外键

```
alter table 表名 drop foreign key 外键（区分大小写）;
```