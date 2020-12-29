# 	Docker（集装箱）

## 一、什么是Docker 

### 1.1、为什么会出现Docker?

上线的项目需要两套环境（开发+线上），可能本地能上线，而线上不能使用。

环境配置是十分麻烦的，每一台的电	脑需要配置时，可能出现不同的问题，导致项目上线的时间延长，而docker就可以解决这个问题，把环境一起上线，就算放到其他电脑也不需要再一次配置环境变量，可以直接（jar+mysql+jdk ...），可以大大降低运维的难度（并不只有docker可以做到）。

Docker的思想是集装箱。

隔离：Docker的核心思想！打包装箱，每一个箱子是隔离的。

Docker通过隔离技术，将服务器压榨到了极致。

### 1.2、Docker

**理念：将应用和环境打包成镜像！**

2010年，在美国几个搞IT的年轻人，成立了`dotCloud`,做pass云计算服务！LXC有关的容器技术，他们将自己的技术（`容器化技术`）命名为Docker。

2013年Docker开源。

2014年4月9日DOcker1.0发布。

Docker火的原因：轻巧。

虚拟机：通过虚拟机虚拟的电脑，十分的大（最少都需要几个G），非常笨重。

Docker（也是一种虚拟机）：不需要装不需要的文件，只有最核心的文件及环境（需要什么就安装什么---> 核心文件 + jdk + mysql 等等 几M或几十M就可以了）。

![image-20200914142438207](/home/tan/.config/Typora/typora-user-images/image-20200914142438207.png)

## 二、Docker能做什么

### 2.1、之前的虚拟机技术

![image-20200914144130221](/home/tan/.config/Typora/typora-user-images/image-20200914144130221.png)

**缺点：**

- 占用资源多。
- 启动时间长。
- 冗余步骤多。

### 2.2、Docker容器化技术

`模拟的并不是一个完整的操作系统。`

![image-20200914144703420](/home/tan/.config/Typora/typora-user-images/image-20200914144703420.png)

**Docker和传统的虚拟机技术的不同：**

- 传统虚拟机，虚拟出一条硬件，运行一整个完整的操作系统，然后在操作系统上安装软件和运行软件。
- 容器内的应用直接运行在宿主机的内容，容器没有自己的内核，也没有虚拟出硬件，所以就很轻便。
- 每一个容器之间相互隔离，每一个容器都有自己的文件系统，互不影响。

> DevOps

- 应用更快速的交付和部署。

**传统：许多的帮助文档，安装程序。**

**Docker：可以一键打包和发布。**

- 更快捷的升级和扩容。

**Docker可以实现整体升级。**

- 更简单的系统运维。

**在容器化技术之后，开发和运维的环境都是一致的，不需要安装环境。**

- 更高效的计算资源利用。

**Docker是内核级别的虚拟化，可以在一个物理机运行许多的容器实例，服务器的性能可以被压榨到极致。**

## 三、安装Docker

````
非root用户加上sudo
````

### 3.1、Docker的基本组成

![image-20200914154641142](/home/tan/.config/Typora/typora-user-images/image-20200914154641142.png)

**名词：**

- 镜像（image）

docker镜像相当于一个模板，可以利用此模板创建容器服务，可以通过镜像创建多个服务，项目就是运行在容器之中的。

- 容器（container）

Docker利用容器技术，独立运行一个或一个组应用，通过镜像来创建的。

有start、stop、remove等命令。

可以理解为一个建立的linux系统。

- 仓库（repository）

存放镜像的地方。

仓库和github一致，分公有仓库和私有仓库之分。

Docker Hub默认是国外的（配置镜像加速器）。

>**安装**

![image-20200914160343969](/home/tan/.config/Typora/typora-user-images/image-20200914160343969.png)

### 3.2、Docker-ubuntu安装

1、删除旧的版本：

````
sudo apt-get remove docker docker-engine docker.io containerd runc
````

使用仓库进行安装：

2、更新及安装需要的环境：

````
sudo apt-get update

sudo apt-get install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common
````

3、增加Docker的GPG的钥匙：

````
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
#阿里云
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
````

````
//使用此命令会出现的信息
sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
````

4、增加仓库

**根据自己的电脑进行选择：**

**阿里云：**

````
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
````

![image-20200914161138318](/home/tan/.config/Typora/typora-user-images/image-20200914161138318.png)

![image-20200914161152384](/home/tan/.config/Typora/typora-user-images/image-20200914161152384.png)

![image-20200914161203947](/home/tan/.config/Typora/typora-user-images/image-20200914161203947.png)

### 3.3、Docker-centos安装

1、删除旧的版本

````
sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine
````

使用仓库安装：

2、更新或安装仓库

````
# 安装需要的包
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# 阿里云镜像
sudo yum-config-manager --add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
````

3、更新缓存

````
sudo yum makecache fast
````

### 3.4、安装Docker引擎

**ubuntu：**

````
 #更新软件包索引
 sudo apt-get update
 #ce社区版 ee企业版 
 sudo apt-get install docker-ce docker-ce-cli containerd.io 
````

````
#删除引擎
sudo apt-get purge docker-ce docker-ce-cli containerd.io
#删除docker文件
sudo rm -rf /var/lib/docker
````

**centos：**

````
sudo yum install docker-ce docker-ce-cli containerd.io
````

````
# 删除引擎
sudo yum remove docker-ce docker-ce-cli containerd.io
# 删除docker文件
sudo rm -rf /var/lib/docker
````

**当前用户加入到docke用户组（不加入，非root用户是无法Server的信息的）：**

````
通过将用户添加到docker用户组可以将sudo去掉，命令如下

sudo groupadd docker #添加docker用户组

sudo gpasswd -a $USER docker #将登陆用户加入到docker用户组中

newgrp docker #更新用户组
````

**查看是否安装成功(可能需要先启动，有权限仍无法看到Server，需要开启Docker-Server服务)：**

````
docker version
````

​	![image-20200914165613136](/home/tan/.config/Typora/typora-user-images/image-20200914165613136.png)

### 3.5、Docker运行与停止

**启动Docker**

````
sudo systemctl start docker
````

**停止Docker**

````
sudo systemctl stop docker
````

**启动Docker-Server：**

````
sudo service docker start
````

**关闭Docker-Server：**

````
sudo service docker stop
````

### 3.6、测试

````
docker run hello-world
````

**安装成功：**

![image-20200914165450637](/home/tan/.config/Typora/typora-user-images/image-20200914165450637.png)	

> 查看下载的hello-world镜像

**表示镜像被成功拉取下来，并运行：**

![image-20200914165735515](/home/tan/.config/Typora/typora-user-images/image-20200914165735515.png)

### 3.7、配置镜像加速器(阿里云)

**1、登录阿里云**

**2、在产品与服务的弹性计算之中找到容器镜像服务，没开通就开通。**

**3、进入容器镜像服务，找到镜像加速器。**

**ubuntu：**

![image-20200914171623356](/home/tan/.config/Typora/typora-user-images/image-20200914171623356.png)

**centos：**

![image-20200918134439456](/home/tan/.config/Typora/typora-user-images/image-20200918134439456.png)

**4、配置镜像加速器**

**ubuntu:**

````
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://yei1lmnz.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
````

**centos：**

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://yei1lmnz.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

配置完！

## 四、 Docker 运行流程及原理

### 4.1、运行流程

![image-20200914172942673](/home/tan/.config/Typora/typora-user-images/image-20200914172942673.png)

### 4.2、底层原理

**Docker工作：**

Docker是一个Client - Server结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端进行访问。

Docker-client发出指令，Dcoker-Server接收进行执行。

![image-20200914181710460](/home/tan/.config/Typora/typora-user-images/image-20200914181710460.png)

**Docker为什么会比虚拟机快：**

1、Docker有虚拟机更少的抽象层。

2、docker利用的是宿主机的内存，vm利用的事GuestOS。

![image-20200914182415622](/home/tan/.config/Typora/typora-user-images/image-20200914182415622.png)

在新建一个容器时，docker不需要像虚拟机一样重新加载一个操作系统内核，可以避免引导，虚拟机加载的Guest OS，所以虚拟机是分钟级别的，而docker直接利用宿主机的操作系统，直接省略了这个复杂的过程，故docker是秒级的。

![image-20200914183326194](/home/tan/.config/Typora/typora-user-images/image-20200914183326194.png)

## 五、Docker常用命令

![image-20200918185710342](/home/tan/.config/Typora/typora-user-images/image-20200918185710342.png)

### 5.1、帮助命令

````
#显示docker信息
docker version
#显示更加详细的信息
docker info
#帮助命令(万能)
dokcer -help
````

**帮助文档：https://docs.docker.com/reference/**

### 5.2、镜像命令

#### 5.2.1、查看所有的镜像

````
docker images
#可选项
-a, --all         #展示所有的镜像
-q, --quiet       #只展示镜像的ID
````

#### 5.2.2、查询镜像

````
docker search
#可选项
docker search --filter #过滤不需要的数据 
- 例：docker search mysql --filter=stars=5000 查询mysql且stars数大于5000
````

#### 5.2.3、下载镜像

````
docker pull 
#可选项
docker pull image(镜像名):tag(标签，版本号)

tan@dark:/etc/docker$ docker pull mysql
Using default tag: latest #使用默认的标签，下载最后一个版本的mysql
latest: Pulling from library/mysql #下载地址
d121f8d1c412: Pull complete #联合文件系统(下载相同文件的其他版本时，存在相同文件时不需要再一次下载)
f3cebc0b4691: Pull complete 
1862755a0b37: Pull complete 
489b44f3dbb4: Pull complete 
690874f836db: Pull complete 
baa8be383ffb: Pull complete 
55356608b4ac: Pull complete 
dd35ceccb6eb: Pull complete 
429b35712b19: Pull complete 
162d8291095c: Pull complete 
5e500ef7181b: Pull complete 
af7528e958b6: Pull complete 
Digest: sha256:e1bfe11693ed2052cb3b4e5fa356c65381129e87e38551c6cd6ec532ebe0e808 #签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest #下载真实地址
````

#### 5.2.4、删除镜像

```
docker rmi 
- docker rmi -f [(名称或ID)] #删除所有的文件，根据指定的信息（-f 强制删除），可以有多个
- docker rmi -f $(docker images -aq) #递归删除所有的镜像
```

### 5.3、容器命令

需要有镜像才能创建容器。

#### 5.3.1、新建容器启动

```` 
docker run [可选参数] 镜像名
# 参数
--name="name"    指定容器，用来区分容器
-d               后台方式运行
-it              使用交互方式运行，进入容器查看内容
-p               指定容器的端口 
 ---------------------------
 -p  ip:主机端口:容器端口
 -p  主机端口:容器端口（最常用）
 -p  容器端口
 容器端口
 ---------------------------
-P               使用随机的端口
````

#### 5.3.2、启动并进入容器

```
docker run -it centos /bin/bash(控制台样式  bash[root@eb0122a02be4 /]   sh[sh-4.4#] ) 
```

#### 5.3.3、退出容器

````
exit # 退出容器，并停止
Ctrl + P + Q # 退出而不停止容器
````

#### 5.3.4、列出所有运行的容器

````
docker ps # 列出所正在运行的容器
docker ps -a # 列出所有当前正在运行的容器+历史运行过的容器
docker ps -a -n=[个数] # 列出最近运行的容器，指定个数
docker ps -aq # 列出所有的容器（无论是否运行）的名字
````

#### 5.3.5、删除容器

````
docker rm 容器ID # 删除一个容器
docker rm 容器ID 容器ID 容器ID # 删除多个容器
docker rm $(docker ps -aq) # 递归删除所有的容器            （处于停止状态）
docker rm [-f：强制] $(docker ps -aq) # 递归删除所有的容器  （所有状态）
docker ps -aq|xargs docker rm # 删除所有的容器            （处于停止状态）
docker ps -aq|xargs docker rm -f # 删除所有的容器         （所有状态）
````

#### 5.3.6、启动和停止容器

````
docker start 容器ID    # 启动容器
docker stop 容器ID     #停止容器
docker restart 容器ID  #重启容器
docker kill 容器ID     #杀死容器（强制停止容器）
````

### 5.4、其他命令

#### 5.4.1、后台启动容器

`````
docker run -d 镜像名
# 问题：docker ps,发现镜像停止了。
# 坑：容器后台启动，必须要有一个前台进程，docker发现没有应用，就会自己停止。
`````

#### 5.4.2、查看日志

````
docker logs -tf [容器名/ID] # 查看所有的日志 （t:显示日志 f:显示时间戳）
docker logs -tf [ --tail [条数] ] [容器名/ID] # 查看指定的最新日志
````

#### 5.4.3、查看容器进程信息

```
docker top [容器ID]
tan@dark:~$ docker top 351936dcb98c
UID       PID         PPID          C          STIME         TTY         TIME          CMD
root      7150        7132          0          13:08         pts/0     00:00:00  /bin/bash
```

#### 5.4.4、查看容器的元数据

````
docker inspect [容器ID]
````

#### 5.4.5、进入当前运行的容器

**通常容器是以后台方式运行的，需要进入容器修改配置时。**

方式一：

````
docker exec -it(以交互方式进入) [容器ID] (/bin/bash or /bin/sh) 
````

方式二：

````
docker attach 容器ID 
````

**两种方式的区别：**

1、方式一进入容器开启一个新的终端，可以在里面操作。

2、方式二进入容器正在执行的终端，不会启动新的进程。

#### 5.4.6、拷贝

**容器数据拷贝到主机**

````
docker cp 容器ID:容器内文件的位置(例:/home/java)  主机位置(例:/home)
````

### 5.5、小结

![image-20200915134027729](/home/tan/.config/Typora/typora-user-images/image-20200915134027729.png)

### 5.6、练习

#### 5.6.1、nginx

````
docker search nginx # 查询nginx
docker pull nginx # 下载nginx
docker run -d nginx # 运行nginx	
````

#### 5.6.2、tomcat

````
docker search tomcat # 查询tomcat
docker run -d tomcat # 下载并运行tomcat	
# 测试时可以使用的方式（只运行一次，停止就删除镜像）
docker run -it --rm tomcat

#tomcat是阉割版的，tomcat的webapps没有数据，linux的命令也少了，原因是因为阿里云镜像，默认是最小的镜像，剔除不必要的数据。
````

#### 5.6.3、Elasticsearch

````
# 控制了最小和最大的占用内存（无法使用）
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64 -Xmx512m" elasticsearch:7.6.2
````

### 5.7、端口暴露机制

![image-20200915173038257](/home/tan/.config/Typora/typora-user-images/image-20200915173038257.png)

### 5.8、查看占用内存

````
docker stats
````

### 5.9、可视化

**portainer（基本不用）**

docker的图形化界面管理工具，提供一个后台面板。

````
docker run -d -p 8888:9000 \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
--name prtainer-local \
docker.io/portainer/portainer
````

访问测试：http://ip:8888/

## 六、Docker镜像

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含某个软件所需的所有内容，包括代码、库、环境变量和配置文件。

**应用可以直接用Docker打包成镜像，直接运行。**

### 6.1、Docker镜像加载原理

> 联合文件系统（UnionFS）

联合文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。联合文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

**我的理解：Dcoker联合文件系统，在下载文件时，会先下载它所需要的底层文件，在一路叠加上来，当有另一个镜像需要用到上一次下载的底层文件时，会直接去引用，没有引用的文件，会跟着镜像的删除而删除。**

> 特性

一次同时加载多个文件系统，从外面只能看到一个文件系统，联合加载回把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

> 原理

docker 的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

​		bootfs(boot file system) 主要包含bootloader和kernel，bootloader  主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就存在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

　　roorfs （root file system），在bootfs之上。包含的就是典型Linux系统中的 /dev ，/proc，/bin ，/etx 等标准的目录和文件。rootfs就是各种不同的操作系统发行版。比如Ubuntu，Centos等等。

![image-20200915212355161](/home/tan/.config/Typora/typora-user-images/image-20200915212355161.png)

　　对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host（宿主机）的kernel，自己只需要提供rootfs就行了，由此可见对于不同的Linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。

**所以虚拟机是分钟级别的（存在bootfs，需要引导加载内核），容器是秒级的（不存在bootfs，直接使用host的bootfs）。**

### 6.2、镜像分层下载

为什么Docker在下载镜像时，会采用这种分层下载？

**资源共享**,当有多个镜像都从相同的Bash镜像构建而来时，宿主只需要在磁盘保存一份bash镜像，在内存中加载镜像，这样下载新的镜像时，无需再一次下载，直接引用即可，镜像的每一层都可以被共享。

**理解：**

Docker镜像是一层一层的镜像展示的，每下载一个镜像都会叠加在原有镜像之上。

![image-20200916092513611](/home/tan/.config/Typora/typora-user-images/image-20200916092513611.png)

**在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的总和：**

![image-20200916093404933](/home/tan/.config/Typora/typora-user-images/image-20200916093404933.png)

**镜像更新：**

![image-20200916093614107](/home/tan/.config/Typora/typora-user-images/image-20200916093614107.png)

**下载镜像时，根据镜像内的文件会分为6层下载，每一层资源都可以共享：**

![image-20200916093701694](/home/tan/.config/Typora/typora-user-images/image-20200916093701694.png)

第一层可能为系统，因为已经存在，所以直接引用其他已经下载好的环境。

![image-20200916093827904](/home/tan/.config/Typora/typora-user-images/image-20200916093827904.png)

> 特点

Docker镜像默认是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部！

这一层被我们称为容器层，容器之下的都叫镜像层。

我们都是在操作容器层，镜像层无法修改。

![image-20200916095032506](/home/tan/.config/Typora/typora-user-images/image-20200916095032506.png)

### 6.3、Commit镜像

```` 
docker commit 提交容器成为一个新的镜像

# 与git类似
docker commit -m="描述信息（describe）" -a="作者" 容器id 目标镜像名:[tag]
````

### 6.4、修改镜像名

````
docker tag 镜像名或镜像ID 新的镜像名[:tag]
````

### 6.5、查看历史变迁

````
docker history 镜像名或镜像ID[:tag]
````

## 七、容器数据卷

**问题：**

1、现在数据存储在容器内，创建的数据也存储在容器内，当容器删除后，数据直接没了，此时产生了一个需求**数据持久化**。

2、容器没了mysql内的数据也就不复存在了，删库跑路也变得简单了，此时又产生了一个需求**数据本地化**。

**解决：**

**容器数据卷：**可以将容器内数据保存的目录挂载到本地，实现数据保存在本地，实现数据同步。

![image-20200916131940210](/home/tan/.config/Typora/typora-user-images/image-20200916131940210.png)

**总结：容器内数据的持久化和同步操作。**

````
docker run -d -v 主机路径:容器内路径 容器名

#docker inspect 容器ID 

"Mounts": 
[
    {
        "Type": "bind",
        "Source": "/home/test", # 主机地址
        "Destination": "/home", # 容器内地址
        "Mode": "",
        "RW": true,
        "Propagation": "rprivate"
    }
],

````

数据卷绑定是双向的，只要绑定上无论容器是否启动数据都会进行双向绑定。

**可以将一些配置文件挂载到服务器，在服务器上修改，容器内就会同步发生变化。**

### 7.1、mysql数据同步

````
docker -d -p 主机IP:容器IP -v 本地路径:容器内路径 -v 本地路径:容器内路径 \
-e MYSQL_ROOT_PASSWORD=mysql登录密码(自己设置) --name 设置容器名 mysql镜像名:版本[tag] 
````

### 7.2、具名挂载和匿名挂载

具名挂载和匿名挂载区别在于挂载时有没有给卷一个名字。

````
指定路径挂载：docker run -d p 3300:3306 -v 容器外路径:容器内路径 nginx
匿名挂载：docker run -d p 3300:3306 -v 容器内路径 nginx
匿名挂载：docker run -d p 3300:3306 -v (卷名不以 / 开头)卷名:容器内路径 nginx
````

所有挂载的卷都保存在**/var/lib/docker/volumes/卷名/_data**之中,可以在里面找到数据。

**匿名挂载(挂载的卷没有指定的名字，不推荐)**

````
tan@dark:~$ docker volume ls
DRIVER              VOLUME NAME
local               0b564012c50b9318a18732bbcadda70c30c0e7539f6ad7fd4ba53fc6112c93c6
local               4a1e43a01f434412b9da2db896d66111a7e37915f0d9b5641e0be8eaa2ff3e47
local               205df3c3f7ce5ebaeb2a812a5a42f82dcca0b561ef0bf12057aee3ca71ff33e4
````

**具名挂载(常用)**

````
n@dark:~$ docker volume ls
DRIVER              VOLUME NAME
local               0b564012c50b9318a18732bbcadda70c30c0e7539f6ad7fd4ba53fc6112c93c6
local               3f7ec27ea32ce759998cbe3d66b1d90728d9f4de0c001d69210da6f6d04b9d31
local               4a1e43a01f434412b9da2db896d66111a7e37915f0d9b5641e0be8eaa2ff3e47
local               ming  (具名挂载)
````

#### 7.2.1、数据卷容器

数据共享，多用于mysql，redis之中。

![image-20200917113145998](/home/tan/.config/Typora/typora-user-images/image-20200917113145998.png)

````
# 父容器启动
docker run -d --name docker01 镜像名 或 镜像ID
# 子容器
docker run -d --name docker02 --volumes-from docker01 镜像名（指定了版本需带上） 或 镜像ID
# 多子容器(挂载的卷数据都是互通的)
docker run -d --name docker03 --volumes-from docker01 镜像名（指定了版本需带上） 或 镜像ID
docker run -d --name docker04 --volumes-from docker02 镜像名（指定了版本需带上） 或 镜像ID
````

父容器删除，子容器的数据依然存在，共享卷是一种拷贝机制。

![image-20200917113624223](/home/tan/.config/Typora/typora-user-images/image-20200917113624223.png)

容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。

数据一旦持久化到了本地，这个时候本地的数据是不会删除的。

#### 7.2.2、拓展

````
docker run -d -P -v (卷名/容器外路径:)容器内路径:ro  mysql
docker run -d -P -v (卷名/容器外路径:)容器内路径:rw  mysql (默认)

# ro -- rw 改变挂载文件的读写权限

ro：readOnly      #只读
rw:readWrite      #读写
````

一但进行了设置，容器对我们挂载出来的内容就存在权限了，设置了 ro 就说明这个路径能通过宿主机进行改变，容器内部无法改变。

## 八、DockerFile

**使用DockerFile就是创建自己的镜像，先使用别人的，在通过学习，让自己的项目可以变成一个镜像，使用自己的。**

**DockerImages：通过DockerFile构建。**

**Docker容器：通过DockerFile构建的镜像运行起来提供服务器。**

### 8.1、方式

Dockerfile是用来构建docker镜像的构建文件，是一个命令脚本。

镜像是一层一层的，脚本的命令也是一行一行的，每一个命令都是一层。

````
# Dockerfile（用的非常多）
# 文件中的内容 指令（大写）参数
FROM centos
VOLUME ["Volume01","Volume02"]
CMD /bin/bash
````

````
# 执行命令
docker build -f Dockerfile路径  -t 镜像名称:tag(版本) .
````

如果构建镜像的时候没有挂载卷，就需要在创建容器时挂载。

**构建步骤：**

1、编写一个DockerFile脚本。

2、docker build脚本构建一个镜像。

3、docker run 运行一个镜像。

4、docker push 上传一个镜像。（阿里云镜像、Docker Hub）

### 8.2、构建过程

**前提：**

1、每个保留关键字（指令）都必须是大写。

2、执行是从上到下顺序执行的。

3、#表示注解。

4、每一个指令都会提交一个新的镜像，并提交。

**生成镜像并运行的过程：**

1、每往镜像中加入一个镜像就是增加了一层。

2、运行成容器时，在上面又增加了一层。

![image-20200917122325883](/home/tan/.config/Typora/typora-user-images/image-20200917122325883.png)

### 8.3、Dockerfile指令

````
# 创建镜像
docker build -f 指令文件地址 -t 镜像名[:tag] .

# 如果指令文件为Dockerfile，会自己寻找
docker build -t 镜像名[:tag] .
````

**学会指令我们就可以自己做了，使用自己的。**

````
FROM			# 基础镜像，来自哪里。
MAINTAINER		# 作者信息。
RUN				# 镜像构建时要运行的命令。
ADD				# 为镜像添加软件。
WORKDIR			# 镜像的工作目录。
WOLUME			# 挂载的目录。
EXPOSE			# 暴露端口。
CMD				# 指定容器启动时运行的命令，会替换原有命令。
ENTRYPOINT		# 指定容器启动时运行的命令，可以在进行追加。
ONBUILD			# 当构建一个被继承Dockerfile 这个时候就会运行ONBUILD的指令。触发指令
COPY			# 类似ADD，将文件copy到镜像中
ENV				# 构建时设置环境变量
````



![image-20200918110254566](/home/tan/.config/Typora/typora-user-images/image-20200918110254566.png)

#### 8.3.1、创建centos镜像

````
FROM centos
MAINTAINER tanxiang<1571922819@qq.com>
RUN yum -y install vim
RUN yum -y install net-tools
ENV MYPATH /usr/local
WORKDIR $MYPATH
EXPOSE 80
CMD /bin/bash
````

#### 8.3.2、CMD和ENTRYPOINT

**CMD（在运行时追加命令会替换掉之前的命令导致报错）：**

![image-20200918113900045](/home/tan/.config/Typora/typora-user-images/image-20200918113900045.png)

````
tan@dark:~/下载$ cat centosCmd.txt 
FROM centos
MAINTAINER tanxiang<1571922819@qq.com>
CMD ["ls","-a"]

tan@dark:~/下载$ docker build -f centosCmd.txt -t centoscmd .
Sending build context to Docker daemon    511MB
Step 1/3 : FROM centos
 ---> 0d120b6ccaa8
Step 2/3 : MAINTAINER tanxiang<1571922819@qq.com>
 ---> Using cache
 ---> 9fbe997d2694
Step 3/3 : CMD ["ls","-a"]
 ---> Running in 229f1535fe50
Removing intermediate container 229f1535fe50
 ---> 66883ec29317
Successfully built 66883ec29317
Successfully tagged centoscmd:latest

tan@dark:~/下载$ docker run centoscmd -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\": executable file not found in $PATH": unknown.
ERRO[0000] error waiting for container: context canceled 
````

**ENTRYPOINT（在运行时追加命令会追加在原有命令之后）：**

![image-20200918114013208](/home/tan/.config/Typora/typora-user-images/image-20200918114013208.png)

````
tan@dark:~/下载$ cat centosEntry.txt 
FROM centos
MAINTAINER tanxiang<1571922819@qq.com>
ENTRYPOINT ["ls","-a"]

tan@dark:~/下载$ docker build -f centosEntry.txt -t centosentry .
Sending build context to Docker daemon    511MB
Step 1/3 : FROM centos
 ---> 0d120b6ccaa8
Step 2/3 : MAINTAINER tanxiang<1571922819@qq.com>
 ---> Using cache
 ---> 9fbe997d2694
Step 3/3 : ENTRYPOINT ["ls","-a"]
 ---> Running in fecc3ff6415b
Removing intermediate container fecc3ff6415b
 ---> eea939094272
Successfully built eea939094272
Successfully tagged centosentry:latest

tan@dark:~/下载$ docker run centosentry -l
total 56
drwxr-xr-x   1 root root 4096 Sep 18 03:35 .
drwxr-xr-x   1 root root 4096 Sep 18 03:35 ..
-rwxr-xr-x   1 root root    0 Sep 18 03:35 .dockerenv
lrwxrwxrwx   1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x   5 root root  340 Sep 18 03:35 dev
drwxr-xr-x   1 root root 4096 Sep 18 03:35 etc
drwxr-xr-x   2 root root 4096 May 11  2019 home
lrwxrwxrwx   1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May 11  2019 lib64 -> usr/lib64
drwx------   2 root root 4096 Aug  9 21:40 lost+found
drwxr-xr-x   2 root root 4096 May 11  2019 media
drwxr-xr-x   2 root root 4096 May 11  2019 mnt
drwxr-xr-x   2 root root 4096 May 11  2019 opt
dr-xr-xr-x 360 root root    0 Sep 18 03:35 proc
dr-xr-x---   2 root root 4096 Aug  9 21:40 root
drwxr-xr-x  11 root root 4096 Aug  9 21:40 run
lrwxrwxrwx   1 root root    8 May 11  2019 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 May 11  2019 srv
dr-xr-xr-x  13 root root    0 Sep 18 03:35 sys
drwxrwxrwt   7 root root 4096 Aug  9 21:40 tmp
drwxr-xr-x  12 root root 4096 Aug  9 21:40 usr
drwxr-xr-x  20 root root 4096 Aug  9 21:40 var
````

#### 8.3.3、制作tomcat镜像

````
FROM centos

MAINTAINER tanxiang<1571922819@qq.com>

RUN yum -y install vim

EXPOSE 8080

ADD apache-tomcat-9.0.37.tar.gz /usr/local/
ADD jdk-8u261-linux-x64.tar.gz /usr/local/

ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV LANG C.UTF-8 	#设置容器编码
ENV JAVA_HOME /usr/local/jdk1.8.0_261 #设置Java地址
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.37 设置tomcat地址
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.37
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$CATALINA_HOME/lib

# 容器启动，就启动tomcat，并将输出信息打印到指令位置
CMD /usr/local/apache-tomcat-9.0.37/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.37/logs/catalina.out
````

**运行：**

````
docker run -d -p 8100:8080 -v /software/tomcat/vloume/apache-tomcat-9.0.37:/usr/local/apache-tomcat-9.0.37/webapps -v /software/tomcat/vloume/apache-tomcat-9.0.37/logs/:/usr/local/apache-tomcat-9.0.37/logs/ tomcat
````

## 九、镜像发布

### 9.1、Docker Hub

**步骤：**

1、注册Docker Hub账号。

2、docker 登录。

````
# 方式一
docker login -u 用户名 -p 密码 
# 方式二
docker login -u 用户 
````

3、修改镜像名称，符合上传的格式

````
docker tag 原镜像名或镜像ID 新镜像名( docker用户名/自定义名称 )[:tag]
````

4、上传至docker hub

````
docker push 新镜像名:tag
````



### 9.2、阿里云

**步骤：**

1、注册阿里云。

2、进入阿里云容器镜像服务。

3、创建一个命名空间，设置为私有。

4、创建一个镜像仓库,选择本地仓库。

5、进入仓库。

**阿里云镜像操作：**

**1. 登录阿里云Docker Registry**

```
    sudo docker login --username=阿里云账号名 registry.cn-beijing(镜像仓库地址).aliyuncs.com
```

用于登录的用户名为阿里云账号全名，密码为开通服务时设置的密码。

您可以在访问凭证页面修改凭证密码。

**2. 从Registry中拉取镜像**

```
sudo docker pull registry.cn-beijing(镜像仓库地址).aliyuncs.com/tanx/blog:[镜像版本号]
```

**3. 将镜像推送到Registry**

```
# 登录阿里云
sudo docker login --username=阿里云账号名 registry.cn-beijing.aliyuncs.com

# 修改镜像的名称
sudo docker tag [ImageId] registry.cn-beijing.aliyuncs.com/tanx/blog:[镜像版本号]

# 推送到阿里云镜像仓库
sudo docker push registry.cn-beijing.aliyuncs.com/tanx/blog:[镜像版本号]
```

请根据实际镜像信息替换示例中的[ImageId]和[镜像版本号]参数。

**4. 选择合适的镜像仓库地址**

从ECS推送镜像时，可以选择使用镜像仓库内网地址。推送速度将得到提升并且将不会损耗您的公网流量。

如果您使用的机器位于VPC网络，请使用 registry-vpc.cn-beijing.aliyuncs.com 作为Registry的域名登录。

**5. 示例**

使用"docker tag"命令重命名镜像，并将它通过专有网络地址推送至Registry。

```
sudo docker images
REPOSITORY                            TAG                IMAGE ID      CREATED  VIRTUAL          SIZE
registry.aliyuncs.com/acs/agent    0.7-dfb6816         37bb9c63c8b2      7 days ago            37.89 MB

# 修改镜像名称
sudo docker tag 37bb9c63c8b2 registry-vpc.cn-beijing.aliyuncs.com/acs/agent:0.7-dfb6816
```

使用 "docker push" 命令将该镜像推送至远程。

```
sudo docker push registry-vpc.cn-beijing.aliyuncs.com/acs/agent:0.7-dfb6816
```



## 十、Docker网络

### 10.1、理解Docker0

#### 10.1.1、docker0网络原理

![image-20200918185848361](/home/tan/图片/QQ截图20200918190757.png)

**问题：docker是如何处理容器网络访问的：**

![](/home/tan/图片/QQ截图20200918191237.png)

````
# 查看容器内部网络：
ip addr
````



> 原理

每使用docker启动一个容器时，docker就会给容器分配一个IP地址，docker0使用桥接模式是容器可以联网，使用的技术为evth-pair技术。

**当我们使用docker启动一个容器后，docker给其分配了eth0@if22的地址：**

![image-20200918210245316](/home/tan/图片/QQ截图20200918210946.png)

**当从容器内查看IP地址时：**

![](/home/tan/图片/QQ截图20200918211125.png)

**发现：**

1、docker每启动一个容器时，都会分配一个IP地址。

2、linux可以ping通容器的IP地址。

```` 
原因：docker0分配的IP地址和linux处于同一个网段，可以直接找到（相当于在局域网）。

就相当于手机和电脑连接wifi，之后路由器会给分配一个IP地址，但是两个IP非常接近，只有最后不一致，手机和电脑就处于同一个网段，是可以相互找到的，如果IP地址不一致，是找不到的。

例：172.20.0.2 和 172.20.0.3处于同一个网段，172.20.0.1则是路由器。
````



![image-20200918194955687](/home/tan/.config/Typora/typora-user-images/image-20200918194955687.png)

3、网卡的出现都是一对一对的，（21 对 22，23 对 24）

```
成对出现的网卡就是使用了evth-pair技术。
evth-pair: 就是一对的虚拟设备接口，它们都是成对的出现，一端连接着协议，一端彼此互相连接。
evth-pair相当于一座桥梁，连接着docker0,使得容器可以通过docker0进行通信。
```

**容器间通信：使用evth-pair连接着路由器，通过路由器进行转发来实现通信。**

默认走docker0，但可以自己配置（--net），没有设置容器默认公用docker0。

![image-20200918185229980](/home/tan/.config/Typora/typora-user-images/image-20200918185229980.png)

**思考：linux可以ping容器，那么容器之间可以ping通吗?**

![](/home/tan/图片/QQ截图20200918195606.png)

**通过evth-pair进行连接之后容器之间发现是可以相互ping通的。**

![image-20200918214211515](/home/tan/.config/Typora/typora-user-images/image-20200918214211515.png)

#### 10.1.2、小结

==Dokcer使用的是Linux的桥接，宿主机中是Docker容器的网桥Docker0，容器使用evth-pair进行网络通信。==

![image-20200918222659718](/home/tan/.config/Typora/typora-user-images/image-20200918222659718.png)

Docker中所有的网络接口都是虚拟的，虚拟的网络转发率高。

只要容器被删除，对应的网桥就消失了。

### 10.2、容器互联

> 问题：在部署微服务时，项目需要连接mysql时，需要mysql的IP地址，但由于容器的IP地址是随机分配的，所以mysql宕机重启之后，容器IP地址就会发生变化，我们可不可以直接不用IP地址连接，而是用它的服务名来访问容器？

````
# 直接使用服务名进行连接，发现是ping不通的
tan@dark:~$ docker exec -it tomcat01 ping tomcat02
ping: tomcat02: Name or service not known

# 使用--link，发现tomcat03是能ping通tomcat02的
tan@dark:~$ docker run -d -P --name tomcat03 --link tomcat02 tomcat
d6425243d96fec068b917d958fd73d22f8d1e41fe35f04ef0d886a0a74c77688
tan@dark:~$ docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.089 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.086 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.084 ms

# 那反向ping呢？
tan@dark:~$ docker exec -it tomcat02 ping tomcat03
ping: tomcat03: Name or service not known
````

````
tan@dark:~$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
5fba85d41414        bridge              bridge              local
0c284ca22028        host                host                local
28a50c570ec5        none                null                local
tan@dark:~$ docker network inspect 5fba85d41414
````

![image-20200919100356459](/home/tan/.config/Typora/typora-user-images/image-20200919100356459.png)

**link本质：在hosts中写死了 172.17.0.3	tomcat02 044bcbd2e51c**

````
tan@dark:~$ docker exec -it tomcat03 cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.3	tomcat02 044bcbd2e51c
172.17.0.4	d6425243d96f
````

现在不推荐使用link，我们需要自定义网络，不使用官方的Docker0，docker0不支持容器名连接访问。

### 10.3、自定义网络

> 查看所有网络

````
tan@dark:~$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
5fba85d41414        bridge              bridge              local
0c284ca22028        host                host                local
28a50c570ec5        none                null                local
````

> 网络模式

````
bridge 		桥接模式（默认，创建容器默认使用）
none  		不配置网络
host  		和宿主机共享网络
container	容器网络连通（用的少，局限性大）
````

> 自定义网络

docker0特点：

1、默认使用。

2、域名不能访问。

3、可以使用link进行容器间通信。

````
docker run -d -P tomcat 等于 docker run -d -P --net bridge tomcat
````

**我们可以使用自定义网络来解决docker0不支持的问题：**

````
tan@dark:~$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
5fba85d41414        bridge              bridge              local
0c284ca22028        host                host                local
28a50c570ec5        none                null                local

tan@dark:~$ docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynetwork
5fce2ab9d729853c3e8db5912b4dce94e0355d0c6d1251cbadf81d20bd10b9e3

tan@dark:~$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
5fba85d41414        bridge              bridge              local
0c284ca22028        host                host                local
5fce2ab9d729        mynetwork           bridge              local
28a50c570ec5        none                null                local

# driver  使用模式（默认桥接）
# subnet  子网掩码
# gateway 网关
````

查看自己创建的网卡：

![image-20200919105059958](/home/tan/.config/Typora/typora-user-images/image-20200919105059958.png)

**使用自定义网络**

````
# 使用自定义网络创建容器
tan@dark:~$ docker run -d -P --name tomcat-net-01 --net mynetwork tomcat
c88a02689eb319cef6f21fd22d323d828305664ab3bf6d7c037c09182d6dfe2d
tan@dark:~$ docker run -d -P --name tomcat-net-02 --net mynetwork tomcat
4560d6a3bfbbd4c2ca9528c47262d9377a4e442361b3f2d5c8b033d74afecd81

# 不使用link，直接使用名称进行ping连接
tan@dark:~$ docker exec -it tomcat-net-01 ping tomcat-net-02
PING tomcat-net-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-net-02.mynetwork (192.168.0.3): icmp_seq=1 ttl=64 time=0.091 ms
64 bytes from tomcat-net-02.mynetwork (192.168.0.3): icmp_seq=2 ttl=64 time=0.096 ms
````

通过使用发现，自定义是可以直接使用名称来进行ping连通的，docker官方帮我们维护好了对应的关系，推荐平时使用。

### 10.4、网络连通

![image-20200919113125921](/home/tan/.config/Typora/typora-user-images/image-20200919113125921.png)

**使用容器与网络进行连接**

![](/home/tan/图片/QQ截图20200919111820.png)

````
# 在没有连通之前是ping不通的
tan@dark:~$ docker exec -it tomcat01 ping tomcat-net-01
ping: tomcat-net-01: Name or service not known
# 使用connect将容器与网络连通之后
tan@dark:~$ docker network connect mynetwork tomcat01
# 再一次进行ping操作，成功的连通了
tan@dark:~$ docker exec -it tomcat01 ping tomcat-net-01
PING tomcat-net-01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-net-01.mynetwork (192.168.0.2): icmp_seq=1 ttl=64 time=0.088 ms
64 bytes from tomcat-net-01.mynetwork (192.168.0.2): icmp_seq=2 ttl=64 time=0.087 ms
````

在自定义网络信息中可以看到tomcat01,成功的加入了。

````
方式：一个容器，两个IP，相当于公网和私网
````

![QQ截图20200919112306](/home/tan/图片/QQ截图20200919112306.png)

**结论：**要实现夸网络操作其他容器，需要使用`docker network connect 网卡名 容器名`

## 十一、将项目打包成镜像

**步骤：**

1、架构springboot项目。

2、打包应用。

3、编写dockerfile。

4、构建镜像。

5、发布运行。

**Dockerfile**

````
FROM java:8

MAINTAINER tanxiang<1571922819@qq.com>

CMD ["mkdir","/blog"]

COPY blogs-0.0.1-SNAPSHOT.jar /blog/blogs.jar

EXPOSE 443

ENTRYPOINT ["java","-jar","/blogs.jar"]
````

