
# SQL分类

1. DDL：操作数据库和表，包括增删改查数据库和表
2. DML：增删改表中的数据
3. DQL：查询表中的数据
4. DCL：管理用户，授权

# DBA

database administrator：数据库管理员

# DCL

## 概念

管理用户，授权

## 管理用户

1. 添加用户：
   语法：create user '用户名'@'主机' identified by '密码';
2. 删除用户：
   语法：drop user '用户名'@'主机名';
3. 修改用户密码：
   语法：1.update user set password = password('新密码') where user = '用户名';
   2.set password for '用户名'@'主机名' = password('新密码');
   mysql中忘记了root用户的密码？
   1.停止mysql服务
   2.使用无验证方式启动mysql服务：mysqld --skip-grant-tables
   3.打开新的cmd窗口，直接输入mysql命令，敲回车。就可以登录成功
   4.usr mysql;
   5.update user password = password('新密码') where user = 'root';
   6.关闭两个窗口
   7.打开任务管理器，手动结束mysqld.exe进程
   8.启动mysql服务
   9.使用新密码登录
4. 查询用户（MySQL）：
   切换到mysql数据库：
   use mysql;
   查询user表：
   select * from user;
   通配符：%表示在任意主机都可以使用用户登录数据库

## 权限管理

1. 权限查询
   语法：
   show grants for '用户名'@'主机名';
2. 授予权限
   语法：
   grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
   grant all on \[数据库名称，、* 表示所有数据库 \].\[ 表名称，、* 表示数据库下所有表 \] to '用户名'@'主机名'; -- 授予所有权限;
   `新增超级权限并允许远程访问：GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;   FLUSH   PRIVILEGES;`
1. 撤销权限
   语法：
   revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';