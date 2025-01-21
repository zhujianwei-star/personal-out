# useradd 添加新用户

## 基本语法

`useradd 用户名` 添加新用户

`useradd -g 组名 用户名` 添加新用户到某个组

# passwd 设置密码

## 基本语法

`passwd 用户名` 设置用户密码

# id 查看用户是否存在

## 基本语法

`id 用户名`

# cat /etc/passwd 查看创建了哪些用户

# su 切换用户

`su: switch user` 切换用户

## 基本语法

`su 用户名称` 切换用户，只能获得用户的执行权限，不能获得环境变量

`su - 用户名称` 切换用户并获得该用户得执行权限以及环境变量

# userdel 删除用户

## 基本语法

`userdel 用户名` 删除用户但保存用户主目录

`userdel -r 用户名` 用户和用户主目录都删除

## 选项说明

|选项|功能|
|---|---|
|-r|删除用户的同时，删除用户相关的所有文件|

# who 查看登录用户信息

## 基本语法

`whoami` 显示自身用户名称

`who am i` 显示登录用户的用户名以及登录时间

# sudo 设置普通用户具有root用户权限

## 操作流程

1. 添加用户，并对其设置密码
```
[root@centos7 ~]# useradd zzz
[root@centos7 ~]# passwd zzz
更改用户 zzz 的密码 。           
新的 密码：
重新输入新的 密码：
passwd：所有的身份验证令牌已经成功更新。
[root@centos7 ~]#    
```

2. 修改配置文件
```linux
1. [root@centos7 ~]# vim /etc/sudoers

2.修改 /etc/sudoers 文件，找到下面一行(100 行 不同的系统可能不一样)，在 root 下面添加一行，如下所示： 
100 root    ALL=(ALL)       ALL
101 zzz     ALL=(ALL)       ALL
	或者配置成采用 sudo 命令时，不需要输入密码
100 root    ALL=(ALL)       ALL
101 zzz     ALL=(ALL)       NOPASSWD:ALL

3. 修改完毕，现在可以用 atguigu 帐号登录，然后用命令 sudo ，即可获得 root 权限进行操作。

4. 在/opt下创建文件夹，如果没有配置不输入密码，则是这样的操作。
[zzz@centos7 opt]$ sudo mkdir test

我们信任您已经从系统管理员那里了解了日常注意事项。
总结起来无外乎这三点：

    #1) 尊重别人的隐私。
    #2) 输入前要先考虑(后果和风险)。
    #3) 权力越大，责任越大。

[sudo] zzz 的密码：
[zzz@centos7 opt]$ 
```

# usermod 修改用户

## 基本语法

`usermod -g 用户组 用户名`

## 选项说明

|选项|功能|
|---|---|
|-g|修改用户的登录组，给定的组必须存在，默认组的id是1。|
