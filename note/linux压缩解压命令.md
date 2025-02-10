# gzip/gunzip压缩

## 基本语法

`gzip 文件` 压缩文件，只能将文件压缩为*.gz文件

`gunzip 文件` 解压缩文件命令

## 经验技巧

（1）只能压缩文件不能压缩目录

（2）不保留原来的文件

（3）同时压缩多个文件会产生多个压缩包

## 案例实操

（1）gzip压缩

```linux
[root@centos7 app]# touch test.java  
[root@centos7 app]# ls  
mysql  test.java  
[root@centos7 app]# gzip test.java  
[root@centos7 app]# ls  
mysql  test.java.gz  
[root@centos7 app]# 
```
（2）gunzip解压缩文件

```linux
[root@centos7 app]# ls  
mysql  test.java.gz  
[root@centos7 app]# gunzip test.java.gz   
[root@centos7 app]# ls  
mysql  test.java  
[root@centos7 app]# 
```

# zip/unzip压缩

## 基本语法

`zip [选项] xxx.zip` 将要压缩的内容 压缩文件和目录命令

`unzip [选项] xxx.zip`

## 选项说明

|zip选项|功能|
|---|---|
|-r|压缩目录|

|unzip选项|功能|
|---|---|
|-d <目录>|指定解压后文件的存放目录|

## 经验技巧

zip压缩命令在windows/linux都通用，可以压缩目录且保留源文件。

## 案例实操

（1）压缩test.java与test.txt，压缩后名称为mypackage.zip

```linux
[root@centos7 app]# touch test.txt  
[root@centos7 app]# ls  
mysql  test.java  test.txt  
[root@centos7 app]# zip mypachage.zip test.java test.txt  
  adding: test.java (stored 0%)  
  adding: test.txt (stored 0%)  
[root@centos7 app]# ls  
mypachage.zip  mysql  test.java  test.txt  
[root@centos7 app]# 
```

（2）解压mypackage.zip

```linux
[root@centos7 app]# unzip mypachage.zip   
Archive:  mypachage.zip  
replace test.java? [y]es, [n]o, [A]ll, [N]one, [r]ename: r  
new name: test1  
 extracting: test1                     
replace test.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: r  
new name: test2  
 extracting: test2                     
[root@centos7 app]# ls  
mypachage.zip  mysql  test1  test2  test.java  test.txt  
[root@centos7 app]# 
```

（3）解压mypackage.zip到指定目录-d

```linux
[root@centos7 app]# unzip mypachage.zip -d /opt  
Archive:  mypachage.zip  
 extracting: /opt/test.java            
 extracting: /opt/test.txt             
[root@centos7 app]# ls /opt/  
containerd  rh  test.java  test.txt  
[root@centos7 app]# 
```

# tar 打包

## 基本语法

`tar [选项] xxx.tar.gz` 将要打包进去的内容 打包目录，压缩后的文件格式为.tar.gz

## 选项说明

|选项|功能|
|---|---|
|-c|产生.tar打包文件|
|-v|显示详细信息|
|-f|指定压缩后的文件名|
|-z|打包时压缩，解包时解压.gz文件|
|-x|解包.tar文件|
|-C|解压到指定目录|

## 案例实操

（1）压缩多个文件

```linux
[root@centos7 app]# ls  
mysql  test.java  test.txt  
[root@centos7 app]# tar -zcvf mypackage.tar.gz test.java test.txt  
test.java  
test.txt  
[root@centos7 app]# ls  
mypackage.tar.gz  mysql  test.java  test.txt  
[root@centos7 app]# 
```

（2）压缩目录

```linux
[root@centos7 app]# ls  
mypackage  mysql  test.java  test.txt  
[root@centos7 app]# tar -zcvf mypachage.tar.gz mypackage/  
mypackage/  
[root@centos7 app]# ls  
mypachage.tar.gz  mypackage  mysql  test.java  test.txt  
[root@centos7 app]# 
```

（3）解压到当前目录

```linux
[root@centos7 app]# ls  
mypachage.tar.gz  mysql  test.java  test.txt  
[root@centos7 app]# tar -zxvf mypachage.tar.gz   
mypackage/  
[root@centos7 app]# ls  
mypachage.tar.gz  mypackage  mysql  test.java  test.txt  
[root@centos7 app]# 
```

（4）解压到指定目录

```linux
[root@centos7 app]# ls /opt/  
containerd  rh  
[root@centos7 app]# tar -zxvf mypachage.tar.gz -C /opt  
mypackage/  
[root@centos7 app]# ls /opt/  
containerd  mypackage  rh  
[root@centos7 app]#
```