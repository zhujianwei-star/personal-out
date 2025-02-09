# find 查找文件或目录

find指令将从指定目录向下递归地遍历其各个子目录，将满足的文件显示在终端。

## 基本语法

`find [搜索范围] [选项]`

## 选项说明

| 选项          | 功能                                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| -name<查询方式> | 按照指定的文件名查找模式查找文件                                                                                              |
| -user<用户名>  | 查找属于指定用户名所有文件                                                                                                 |
| -size<文件大小> | 按照指定的文件大小查找文件，单位为： <br />B---块(512字节) <br /> C--字节 <br />W---字(2字节) <br />K---千字节 <br />M---兆字节 <br />G---及字节 |

## 案例实操

（1）按文件名：根据名称查找/目录下的.txt文件

```linux
[root@centos7 ~]# find / -name *.txt

（2）按拥有者：查找/opt目录下，用户名称为-user的文件

[root@centos7 ~]# find /opt -user root  
/opt  
/opt/containerd  
/opt/containerd/lib  
/opt/containerd/bin  
/opt/rh  
[root@centos7 ~]# 

（3）按文件大小：在/home目录下查找大于200m的文件（+n 大于 -n小于 n等于）

[root@centos7 ~]# find /home -size +200M

```
# locate 快速定位文件路径

locate指令利用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位给定的文件。locate指令无需遍历整个文件系统，查询速度较快。为了保证locate查询结果的准确度，管理员必须定期更新locate时刻。

## 基本语法

`locate 搜索文件`

## 经验技巧

由于locate指令基于数据库查询，所以第一次运行前，必须使用updatedb指令创建locate数据库。

## 案例实操

（1）查询文件夹

```linux
[root@centos7 ~]# updatedb  
[root@centos7 ~]# locate .txt
```

# grep 过滤查找以及「|」管道符

管道符「|」，表示将前一个命令的处理结果输出传递给后面的命令处理

## 基本语法

`grep [选项] 查找内容 源文件`

## 选项说明

| 选项       | 功能              |
| -------- | --------------- |
| -n       | 显示匹配行以及行号       |


## 案例实操

（1）查找某文件排在第几行
```linux
[root@centos7 ~]# ls | grep -n archieve  
2:archieve
```