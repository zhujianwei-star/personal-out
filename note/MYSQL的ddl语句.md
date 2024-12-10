
# 概念

操作数据库和表的语言

## 数据库：CRUD

### C：Create：创建

创建数据库：create database 数据库名称;
创建数据库，判断不存在，再创建：create database if not exists 数据库名称;
创建数据库，并指定字符集：create database 数据库名称 character set 字符集名;
创建数据库，判断是否存在，并指定字符集名：create database if not exists 数据库名称 character set 字符集名;

### R：Retrieve：查询

查询所有数据库的名称：show databases;
查询某个数据库的字符集/查询某个数据库的创建语句：show create database 数据库名称;

### U：Update：修改

修改数据库的字符集：alter database 数据库名称 character set 字符集名;
                
### D：Delete：删除

删除数据库：drop database 数据库名称;
判断数据存在，存在再删除：drop database if exists 数据库名称;

### 使用数据库：

查询正在使用的数据库名称：select database();
使用数据库：use 数据库名称;

## 表：CRUD

### C：Create：创建

####  语法：

```sql
create table 表名(
	列名1 数据类型1,
	列名2 数据类型2,
	……
	列名n 数据类型n
);
```

> 注意：最后一列，不需要加逗号

#### 数据库类型（常用）：

1. int：整数类型 | age int
2. double：小数类型 | score double(4,2)：括号中的数字前面一个数字表示这个数据总共有多少位，后面 一位数字表示小数点后面有多少位
3. date：日期，只包含年月日，yyyy-MM-dd
4. datetime：日期，包含年月日时分秒，yyyy-MM-dd HH:mm:ss
5. timestamp：时间错类型，包含年月日时分秒，yyyy-MM-dd HH:mm:ss；与datetime类型不同的是，如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值。
6. varchar：字符串 | name varchar(32)：括号中的数字表示最大字符为32个字符；而在“zhangsan”中有8个字符，但是“张三”只有两个字符

创建表：
create table student(
id int,
name varchar(32),
sroce double(4,1),
birthday date,
insert_time timestamp
);

复制一张表：
create table 表名 like 原表名;

新增索引：
CREATE INDEX idx_update_action_org_status USING BTREE ON sourcedata_record (update_time,action_code,org_code,status);

##### 索引设计事项

注意：建表时表必须要有主键，表及所有字段必须要comment，主键索引前缀: pk_ ， 唯一索引前缀: uk_，普通索引前缀:idx_。

### R：Retrieve：查询

查询某个数据库中所有表的名称：show tables;
查询某个表的字符集/查询某个表的创建语句：show create table 表名称;
查询表结构：desc 表名;

### U：Update：修改

修改表名：alter table 表名 rename to 新表名;
修改表的字符集：alter table 表名 character set 新的字符集;
增加一列：alter table 表名 add 列名 数据类型;
修改列的名称、类型：
1. alter table 表名 change 列名 新列名 新数据类型;
2. alter table 表名 modify 列名 新数据类型;
```sql
示例：

修改表中当前名称的列所有信息，此处可以改字段名称

ALTER TABLE dmp.data_asset_tableinfo CHANGE DAT_System DAT_Source bigint(255)  NOT NULL COMMENT '数据源';

// 不可以修改字段名称

ALTER TABLE dmp.data_asset_tableinfo MODIFY COLUMN DAT_Source bigint(255) NOT  NULL COMMENT '数据源';

ALTER TABLE dmp.data_asset_tableinfo ADD DTA_System varchar(255) NULL COMMENT  '隶属数据库系统';
```

设置某个字段的默认值：
1. alter table 表名 alter column 字段名 drop default; // 需要先删除其默认值
2. alter table 表名 alter column 字段名 set default 默认值; // 重新设置默认值
                        删除列：atler table 表名 drop 列名;
                        
```sql
-- 设置已有列自动增长：
alter table tabel_name modify 列名 数据类型 auto_increment;

注意事项：
修改后从下一条记录开始自动增长。如果想让原来的自动增长就得复制现有表的结构(无id），添加id并加上AUTO_INCREMENT，然后通过循环，添加n条空记录，然后对应先前表的id，依次插入数据。

mysql自动增长开始值设置总结：
1、创建表，设置表主键id自动增长，默认自动增长的起始值为1开始。
2、当表数据不为空的时候，重新去修改自动增长id开始值，mysql会主动去核对你设置的起始值是否是当前数据库已有id的最大值+1; 若是则修改成功，若不是则修改不成功 （默认还是id最大值+1）
3、要设置自动增长为1开始，需要清空表数据才行。alter table table_name AUTO_INCREMENT=1
4、若每次直接在数据库里面插入数据，则会自动的去修改当前表的自动增长起始值（设置自动增长起始值为当前插入成功的数据的id）
```

### D：Delete：删除

drop 表名;
drop if exists 表名;