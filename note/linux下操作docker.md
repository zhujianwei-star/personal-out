# Docker安装

## 根据官方帮助文档安装docker

根据帮助文档安装Linux下的docker及其中遇到的问题

```shell
以下操作，当权限不够时，需要使用sudo增加权限
# 1.卸载旧的版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
        
        
# 2.更新yum源
yum clean all

yum makecache

yum -y update

如果出现了"Could not resolve host: mirrorlist.centos.org; 未知的错误"的报错，首先需要查看网络是否畅通，可以ping一下百度确认，如果网络没有问题，则需要换源：
使用： wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo 或者在没有安装wget的情况下使用 curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo


# 3.需要的安装包
yum install -y yum-utils
其中：如果出现了Invalid configuration value: failovermethod=priority in /etc/yum.repos.d/CentOS-epel.repo; Configuration: OptionBinding with id "failovermethod" does not exist错误，需要使用vim将 /etc/yum.repos.d/CentOS-epel.repo文件中的failovermethod=priority注释掉即可


# 4.切换为阿里云镜像
docker官方镜像
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

切换为阿里云镜像
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
    
# 5.安装docker引擎		docker-ce 社区版  docker-ee企业版，官方推荐社区版
yum install docker-ce docker-ce-cli containerd.io


# 6.启动docker
systemctl start docker


# 7.使用docker version查看是否启动成功
docker version


# 8.跑hello world方法
docker run hello-world
打印的结果中包含Hello from Docker!即代表成功。

# 9.查看docker镜像
docker images


# 10.卸载docker
	# 10.1 卸载docker引擎
	yum remove docker-ce docker-ce-cli containerd.io
	# 10.2 删除docker容器文件以及镜像文件
	rm -rf /var/lib/docker
	rm -rf /var/lib/containerd
```

> sudo指令
> 功能： 以root的身分执行命令
> 语法： sudo 其他指令
> 用户： 被root加入『/etc/sudoers』文件中的用户
> 1．root的密码除了root本人知道外，不需被其他需要用到root权限用户知道，因为使用sudo时，要求输入的密码是『该位用户自己的密码』。
> 2．把所有可执行sudo指令的用户都规范在『/etc/sudoers』这个文件中，root可以很容易地掌控整个系统。
>
> 执行sudo su -成root的用户，和root用户的区别：
> 普通用户使用sudo 来执行只有root才能执行权限的命令，跟用root用户执行是不一样的，因为这时候他用的还是普通用户的环境变量。
> 用su -成root的用户还是有些环境变量是和root登陆是不一样的。另外，它们的uid也是不一样，只有euid是相同的。
> PS： 修改root密码
> 请先用该建立的第一个用户登入，使用 sudo passwd root 即可修改 root 密码. ps: 若要输入密码，该密码就是执行 sudo 该用户的密码。

## 阿里云镜像加速

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://tqa7htwq.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 底层原理

### Docker是怎么工作的

Docker是一个Client - Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问
DockerServer接收到Docker-Client的指令，就会执行这个命令。

### Docker为什么比vm快

1. Docker有着更少的抽象层
2. Docker利用的是宿主机的内核，vm 需要的是Guest OS
   所以说，新建一个容器的时候，Docker不需要像虚拟机一个重新加载一个操作系统的内核，避免引导，虚拟机是加载Guest OS，是分钟级别的。而Docker是利用宿主机的操作系统吗，省略了这个复杂的过程，是秒级！

# Docker常用命令

## 帮助命令

```shell
docker verion				# 显示当前docker的版本信息
docker images				# 显示当前docker的系统信息，包括镜像和容器数量
docker [命令] --help		   # 帮助命令
```

帮助文档地址：[https://docs.docker.com/reference/](https://docs.docker.com/reference/)

## 镜像命令

### docker images 查看所有本地主机上的镜像

```shell
[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   2 months ago   13.3kB

# 解释
REPOSITORY			# 镜像的仓库源
TAG					# 镜像的标签
IMAGE ID			# 镜像的id
CREATED				# 镜像的创建时间
SIZE				# 镜像的大小

# 可选项
  -a, --all             Show all images (default hides intermediate images) # 列出所有镜像
      --digests         Show digests # 显示摘要
  -f, --filter filter   Filter output based on conditions provided # 过滤
      --format string   Pretty-print images using a Go template # 格式化
      --no-trunc        Don't truncate output # 不截断输出
  -q, --quiet           Only show image IDs # 只显示镜像的id
```

### docker search 搜索镜像

```shell
docker search [options] 镜像名字 ：只会查询出与这个名字相关的镜像信息，但是没有办法查询出具体的版本号
各个选项说明：
	name：镜像名称
	description：镜像说明
	stars：点赞数量
	official：是否是官方的
	automated：是否自动构建的
options说明：
	--limit [n] : 只列出n个镜像，默认25个
	
https://hub.docker.com：可以查询到镜像的所有信息，比如某个镜像有哪些版本，版本号是多少

[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11840     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4512      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   887                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   565       [OK]       
phpmyadmin                        phpMyAdmin - A web interface for MySQL and M…   398       [OK]       
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   92                   


# 可选项
--filter=STARS=3000		# 搜索出来的镜像就是STARS大于3000的

[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker search mysql -f STARS=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   11840     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4512      [OK]  
```

### docker pull 下载镜像

```shell
# 下载镜像	docker pull 镜像名[:tag]
[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker pull mysql
Using default tag: latest # 如果不写tag，默认就是lastest，即最新的版本
latest: Pulling from library/mysql
72a69066d2fe: Pull complete  # 分层下载，docker image的核心，联合文件系统
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709 # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 真实地址

# 即 docker pull mysql等价于docker pull docker.io/library/mysql:latest

# 指定版本下载
[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
72a69066d2fe: Already exists # 已存在的就不会继续下载，而是会使用已有的
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete # 不存在的就会下载
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7

# 查看镜像
[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
mysql         5.7       c20987f18b13   7 hours ago    448MB
mysql         latest    3218b38490ce   7 hours ago    516MB
hello-world   latest    feb5d9fea6a5   2 months ago   13.3kB
```

### docker system df 查看空间

```shell
docker system df : 查看镜像/容器/数据卷所占的空间
	相似于linux中的命令 df -h
```

### docker rmi 删除镜像！

```shell
[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker rmi -f 容器id					 # 删除指定的容器
[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker rmi -f 容器id 容器id				# 删除多个指定的容器
[root@iZbp1hmr9w2y2q578fwhxgZ ~]# docker rmi -f $(docker images -aq)	  # 删除全部的容器
```

## 容器命令

**说明：我们有了镜像才可以创建容器，linux，下载一个centos镜像来测试学习**

``` shell
docker pull centos
```

### 新建容器并启动

```linux
docker run [可选参数] iamge

# 常用可选参数说明
--name="name"						# 容器名字，比如使用tomcat01,tomcat02等来区分容器
-d									# 后台方式运行
-it									# 使用交互方式运行，进入容器查看内容
-p									# 指定容器端口，如8080:8080
	-p	ip:主机端口:容器端口：将此容器端口映射为对应的ip和主机端口
	-p	主机端口:容器端口：将此容器端口映射为对应的主机端口（常用）
	-p	容器端口
	容器端口
-P									# 大写P，随机指定端口
-d									# 后台方式运行

# 测试，启动并进入容器
[root@iZbp1hmr9w2y2q578fwhxgZ /]# docker run -it centos /bin/bash	# 此目录相当于进入linux的控制台，使用shell是即使不加这个目录，默认也会进入控制台
[root@8460fa9b29a1 /]# ls	# 查看容器内的centos，基础版本，很多命令都是不完善的！
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@8460fa9b29a1 /]# 

#从容器中退回主机
[root@8460fa9b29a1 /]# exit		# 退回主机并停止此容器
exit
[root@iZbp1hmr9w2y2q578fwhxgZ /]# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  patch  proc  root  run  sbin  srv  sys  tmp  usr  var  www
```

### 启动交互式容器(前台命令行)

```linux
使用镜像centos:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。
docker run -it centos /bin/bash 

参数说明：
-i: 交互式操作。
-t: 终端。
centos : centos 镜像。
/bin/bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash。
要退出终端，直接输入 exit 或者按键盘上的  ctrl + p + q：退出后，容器会继续运行，可以使用命令继续进入

两种方式的区别：exit：退出后，容器会立即停止，不会继续运行
			 ctrl + p + q：退出后，容器会继续运行，可以使用命令继续进入
```

### 列出所有容器

```linux
# docker ps [可选参数] 命令
					# 留空的时候只会列出当前运行的容器
-a					# 列出所有容器，包括当前运行的和历史的容器，不加只会列出当前运行的容器
-n=?				# 后面跟数字，可以列出当前运行的最近几个容器
-q					# 只列出容器id
```

### 退出容器

```linux
exit					# 直接容器通知并退出
使用快捷键ctrl + p + q	 # 容器不停止退出
```

### 删除容器

```linux
docker rm 容器id							# 删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)			 # 删除所有容器
docker ps -aq|xargs docker rm			 # 删除所有的容器 
```

### 启动和停止容器的操作

```linux
docker start 容器id						# 启动容器
docker restart 容器id						# 重启容器
docker stop 容器id						# 停止当前正在运行的容器
docker kill 容器id						# 停止当前正在运行的容器
```

### 启用容器数据卷

```linux
启用容器数据卷之前需要增加--privileged=true，因为centos7中文件夹的权限更为严格，如果不加，会导致文件夹中的权限不够。

-v 容器目录:主机目录[:rw/ro(不写默认为rw，rw是都可以读写，ro是容器只读，主机可以读写)]

--volumes-from 父类

例如：启动mysql容器
docker run -d -p 3306:3306 --privileged=true -v /app/mysql/data:/var/lib/mysql -v /app/mysql/log:/var/log/mysql -v /app/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root --name mysql mysql:8.0.33
```

### 提交容器为镜像

```linux
docker commit -m="提交的信息" -a="作者" 容器id 要创建的目标镜像名:标签名
例：docker commit -m="centos add yum,vim,wget..." -a="zzz" asfasdgasd yvwcentos7:1.0
```

## 日志命令

### 容器日志

```linux
docker inspect --format '{{.LogPath}}' 容器简短id/容器名称 		可以查询容器的日志位置
docker inspect --format '{{.Id}}' 容器简短id/容器名称		可以查询容器的全id

然后通过cat命令查看上述命令找到的日志地址

cat /var/lib/docker/containers/97069f94437b86b50341f8253d85f426884315c3d027f7b7fa975751c7d8e18e/97069f94437b86b50341f8253d85f426884315c3d027f7b7fa9757

或者使用docker logs 容器简短id查询
```

## docker-compose常用命令

```linux
Compose常用命令
docker-compose -h                           # 查看帮助
docker-compose up                           # 启动所有docker-compose服务
docker-compose up -d                        # 启动所有docker-compose服务并后台运行
docker-compose down                         # 停止并删除容器、网络、卷、镜像。
docker-compose exec  yml里面的服务id                 # 进入容器实例内部  docker-compose exec docker-compose.yml文件中写的服务id /bin/bash
docker-compose ps                      # 展示当前docker-compose编排过的运行的所有容器
docker-compose top                     # 展示当前docker-compose编排过的容器进程
 
docker-compose logs  yml里面的服务id     # 查看容器输出日志
dokcer-compose config     # 检查配置
dokcer-compose config -q  # 检查配置，有问题才有输出
docker-compose restart   # 重启服务
docker-compose start     # 启动服务
docker-compose stop      # 停止服务
```
