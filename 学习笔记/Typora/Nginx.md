# Nginx

## 1、nginx基本概念

### 1.1、简介

1、Nginx(engine x) 是一个**轻量级的**、高性能的**HTTP**和**反向代理**web服务器，同时也提供了**IMAP/POP3/SMTP服务**。Nginx是由==伊戈尔·赛索耶夫==开发的，第一个公开版本0.1.0发布于2004年10月4日。

2、Nginx(engine x) 因它的**稳定性**、**丰富的功能集**、**示例配置文件**和**低系统资源的消耗**而闻名。2011年6月1日，nginx 1.0.4发布。

3、Nginx其特点是**占有内存少**，**并发能力强**。

### 1.2、nginx主要应用场景以及出现的原因

**应用场景：**

1、静态资源服务

2、反向代理服务

3、API服务

**出现的原因：**

1、互联网的快速增长

- 互联网的快速普及。

- 全球化。

- 物联网。

2、摩尔定律：性能提升

3、低效的Apache：一个进程同一时间只会处理一个连接请求。

### 1.3、nginx优点

1、高并发，高性能

2、可扩展性好

3、高可靠性

4、热部署

5、BSD许可证

### 1.4、nginx的组成

![image-20200928203217256](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928203217256.png)

### 1.4、nginx的命令行

![image-20200928211902016](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928211902016.png)

## 2、nginx安装、常用命令和配置文件

### 2.1、nginx安装(linux)

在windows中性能不能完全发挥。

**下载地址：**http://nginx.org/en/download.html 

解压完之后进入文件执行`./configure`命令。

**出现错误(依赖为安装)：**

````
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
````

**centos：**

````
yum -y install pcre-devel
yum -y install make zlib zlib-level gcc-c++ libtool openssl openssl-devel
````

**ubuntu(安装依赖)：**

````
sudo apt-get install libpcre3 libpcre3-dev	
sudo apt-get install openssl libssl-dev
````

**执行：**

````
make 
make install	
````

执行完之后就安装了pcre的依赖了

````
tan@dark:/usr/local/nginx/sbin$ pcre-config --version
8.39
````

安装其他依赖：

````
sudo apt-get install gcc zlib-devel pcre-devel openssl openssl-devel
````

进入/usr/local/nginx/sbin，用root权限运行即可。

**查看nginx的访问端口：**

````
tan@dark:/usr/local/nginx/conf$ vim nginx.conftan@dark:/usr/local/nginx/conf$ 
````

![image-20200927184252775](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927184252775.png)

![image-20200927182656354](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927182656354.png)

**如果不能访问需要在防火墙中增加端口：**

**centos：**

```
# 查看firewall服务状态
systemctl status firewalld

# 查看firewall的状态
firewall-cmd --state

# 开启firewalld服务
service firewalld start

# 重启firewalld服务
service firewalld restart

# 关闭firewalld服务
service firewalld stop

# 查看防火墙规则
firewall-cmd --list-all 

# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp

#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload

# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```

**ubuntu(一般是关闭或未安装，ubuntu一般可以使用)：**记录防火墙命令

```
# 安装方法
sudo apt-get install ufw

# 启用
sudo ufw enable
sudo ufw default deny

作用：开启了防火墙并随系统启动同时关闭所有外部对本机的访问（本机访问外部正常）。

# 关闭
sudo ufw disable

# 查看防火墙状态
sudo ufw status

# 开启/禁用相应端口或服务举例
sudo ufw allow 80 允许外部访问80端口
sudo ufw delete allow 80 禁止外部访问80 端口
sudo ufw allow from 192.168.1.1 允许此IP访问所有的本机端口
sudo ufw deny smtp 禁止外部访问smtp服务
sudo ufw delete allow smtp 删除上面建立的某条规则
sudo ufw deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 22 要拒绝所有的TCP流量从10.0.0.0/8 到192.168.0.1地址的22端口

可以允许所有RFC1918网络（局域网/无线局域网的）访问这个主机（/8,/16,/12是一种网络分级）：
sudo ufw allow from 10.0.0.0/8
sudo ufw allow from 172.16.0.0/12
sudo ufw allow from 192.168.0.0/16

# 推荐设置
sudo apt-get install ufw
sudo ufw enable
sudo ufw default deny
这样设置已经很安全，如果有特殊需要，可以使用sudo ufw allow开启相应服务。
```

### 2.2、命令

**需要先进入`/usr/local/nginx/sbin`。**
**常用命令：**

````
# 查看版本号
tan@dark:/usr/local/nginx/sbin$ ./nginx -v
nginx version: nginx/1.18.0

# 启动nginx（必须使用root权限）
tan@dark:/usr/local/nginx/sbin$ sudo ./nginx 

# 关闭nginx（必须使用root权限）
tan@dark:/usr/local/nginx/sbin$ sudo ./nginx -s stop

# 重启nginx（必须使用root权限）
tan@dark:/usr/local/nginx/sbin$ sudo ./nginx -s reload
````

### 2.3、nginx配置文件

**1、全局块**

从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令，主要包括配置运行nginx服务器的用户(组)、允许生成worker process 数，进程PID存放路径、日志存放路径和类型以及配置文件的引入等。

````
#user  nobody;

# nginx服务器并发处理服务的关键配置
# worker_processes越大，可以支持的并发处理量越多，但是会收到硬件、软件等设备的制约。
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;
````

**2、events块**

**events块主要影响nginx服务器与用户的网络连接**，常见的设置包括是否开启对多

worker_processes下网络连接进行序列化，是否允许同时接收多个网络连接，选取那种事件驱动模型来处理连接请求，每个wordprocess可以同时支持的最大连接数等。

````
events {
	# 表示每个work process支持的最大连接数为1024
	# 这个配置对nginx的性能影响较大，应该灵活配置
    worker_connections  1024;
}
````

**3、http块**

nginx服务器配置中最频繁的部分。

http块也可以包括==http全局块、server块==。

**1、http块：**

http全局配置的指令包括文件引入、MIME-TYPE定义、日志自定义、连接超时时间、单链接请求数上限等。

**2、server块：**

这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。  

每一个http块可以包括多个server块，而每一个Server块就相当于一个虚拟主机。

每一个Server块也分为全局server块，以及可以同时包含多个location块。

- **全局Server块**

最常见的配置是本虚拟主机的监听配置和本虚拟机的名称或IP配置。

````\
listen       80;
server_name  localhost;
````

- **location块**

一个server块可以配置多个location块。

可以对服务器接受请求的字符串，对虚拟主机名称（或IP地址别名）之外的字符串进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也可以在这里进行。

```
# 表示路径中包含/则进行请求的跳转 
location / { 
 	root   html;
	index  index.html index.htm;
}
```

### 2.4、配置ssl

````
server{
        listen   80;
        # 域名地址
        server_name  www.tanxiang.com tanxiangblog.com;
        # 永久重定向到https://www.tanxiangblog.com:443
        return 301 https://www.tanxiangblog.com:443;

        location / {
        # 反向代理 http://101.200.240.22:设置的端口
        proxy_pass http://101.200.240.22:xxxx;
        root   html;
        index  index.html index.htm;
        }
}

server {
        listen 443 ssl;
        server_name  www.tanxiangblog.com;
        charset utf-8;
        # 4228758_www.tanxiangblog.com.pem 放在conf目录下
        ssl_certificate      4228758_www.tanxiangblog.com.pem;
        # 4228758_www.tanxiangblog.com.key 放在conf目录下
        ssl_certificate_key  4228758_www.tanxiangblog.com.key;
       	# 加密
        ssl_ciphers ECDHE-RSA-AES128-GCM-		SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_session_cache    shared:SSL:1m;
        # session超时时间
        ssl_session_timeout  5m;
        ssl_prefer_server_ciphers  on;

        location / {
          # 反向代理 http://101.200.240.22:设置的端口
            proxy_pass https://www.tanxiangblog.com:xxxx;
            # 可以正确地识别实际用户发出的协议是 http 还是 https
            proxy_set_header X-Forwarded-Proto https;
            root   html;
            index  index.html index.htm;
        }
    }

````

**错误：**

```
# 表示ssl模块没有加入
nginx:[emerg]unknown directive ssl
```

**解决：**

```
# 进入nginx文件的位置（解压文件不是编译文件）
[root@754211a5c99d local]# cd nginx-1.18.0/
[root@754211a5c99d nginx-1.18.0]# ./configure --with-http_ssl_module 
# 使用make即可，重新编译
[root@754211a5c99d nginx-1.18.0]# make
# 不需要在进行 make install （这个命令是进行安装，使用所有文件会被覆盖）
```

```
# 表示在 nginx 1.15 不要再使用在 ssl:on，应该改成 listen 443 ssl;
nginx: [warn] the "ssl" directive is deprecated, use the "listen ... ssl"
```

## 3、nginx配置实例 1-反向代理

**1、正向代理：**

在客户端（浏览器）配置代理服务器，进行互联网的访问。

![image-20200927151815283](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927151815283.png)

**2、反向代理：**

由于客户端对于代理是没有感觉的，因为客户端不需要任何配置就可以访问，所以我们只需要将请求发送到代理服务器，由代理服务器去目标服务器获取数据后，再返回给客户端，此时代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器的IP地址，隐藏了真实服务器的IP地址。

![image-20200927152303252](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927152303252.png)

### 3.1、实现

**1、实现效果**

- 打开浏览器，在浏览器输入地址www.123.com，跳转到tomcat的启动项目主页面中。

**2、准备工作**

- 开启tomcat。
- 查询tomcat的端口是否开放（为开放则需要开放）。

![image-20200927221408584](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927221408584.png)

````
listen       80; # 监听端口
server_name  localhost;

location / {
	root   html;
	# 反向代理：映射IP地址 http://127.0.0.1:8081
	proxy_pass  http://127.0.0.1:8081;
	index  index.html index.htm;
}
````

![image-20200928094120925](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928094120925.png)

**1、实现效果**

- 根据访问的路径跳转到不同端口的服务中。

nginx监听的端口为8100,

访问http://127.0.0.1:8100/user/    跳转到http://127.0.0.1:8081

访问http://127.0.0.1:8100/admin/ 跳转到http://127.0.0.1:8082

**2、准备工作**

- 开启两个tomcat。
- 查询tomcat的端口是否开放（为开放则需要开放）。
- 在tomcat内建立对应的文件夹（名称为要跳转的路径，如要跳转到user，名称则为user）

```
server {
	listen       8100;
	server_name  localhost;

	location ~ /user/ {
		proxy_pass  http://127.0.0.1:8081;
	}

	location ~ /admin/ {
		proxy_pass  http://127.0.0.1:8082;
	}
}
```

![image-20200928110121868](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928110121868.png)

![image-20200928110138886](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928110138886.png)

### 3.2、location指令

1、=：用于不含正则表达式的url前，要求请求字符串与url严格匹配，如果匹配成功，就停止继续向下搜索并立即处理该请求。

2、~：用户表示url包含正则表达式，并区分大小写。

3、~*：用户表示url包含正则表达式，不区分大小写。

4、^~：用于不含正则表达式的url前，要求nginx服务器找到标识url和请求字符串匹配度最高的location后，立即使用此location处理请求，而不在使用location块中的正则url和请求字符串做匹配。

## 4、nginx配置实例 2-负载均衡

单个服务器解决不了，我们可以增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为分发到多个服务器上，将负载分发**(平均分发)**到不同的服务器，也就是我们所说的负载均衡。

![image-20200927162107652](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927162107652.png)

````
http{
 	include       mime.types;
    default_type  application/octet-stream;

	# 配置负载均衡
    upstream myserver{ 
        server http://127.0.0.1:8081;
        server http://127.0.0.1:8082;
    }
    server {
        listen       80;
        server_name  localhost;

        location / {
           # 配置负载均衡
           proxy_pass http://myserver;
           root   html;
           index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
````

![image-20200928130527548](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928130527548.png)

![image-20200928130541239](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928130541239.png)

**分配策略：**

**1、轮询（默认）**

每个请求按时间顺序逐一分配到不同的后端服务器，如果后台服务器down掉，能自动剔除。

**2、weight（权重）**

weight代表权重，默认为1,权重越高被分配的客户端越多（服务器性能不均时用）。

````
upstream myserver{ 
	server http://127.0.0.1:8081 weight=5;
	# 权重高，分配的客户端更多
	server http://127.0.0.1:8082 weight=7;
}
````

**3、ip_hash(第一次访问其中一个服务器，后面此IP会一直被分配到此服务器)**

每个请求按访问ip的hash结果分配，每个访客固定一个后端服务器，可以解session的问题。

````
upstream myserver{ 
	ip_hash
	server http://127.0.0.1:8081;
	server http://127.0.0.1:8082;
}
````

**4、fair（第三方）**

按后端服务器的响应时间来分配请求，响应时间段的优先分配。

**每个服务器都会发送请求，选取响应时间最短的。**

````
upstream myserver{ 
	server http://127.0.0.1:8081;
	server http://127.0.0.1:8082;
	fair
}
````

## 5、nginx配置实例 3-动静分离

![image-20200927163403481](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927163403481.png)

为了加快网站的解析速度，可以将动态页面和静态页面进行分离由不同的服务器来解析，加快解析速度。降低原来单个服务器的压力。

![image-20200927164210188](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200927164210188.png)



将动态请求和静态请求分开，而不是简单的将动态页面和静态页面分开，可以理解为使用nginx处理静态页面，tomcat处理动态页面。
![image-20200928130404501](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928130404501.png)

**两种实现方式：**

- 纯粹的把静态文件独立成单独的域名，放在独立的服务器上（主流方案）。
- 动态和静态文件混合发布，通过nginx分开。

**缓存(不适合经常变动的资源文件)：**

location可以通过指定不同后缀名实现不同的请求，通过expires参数进行设置，可以让浏览器缓存过期时间，减少与服务器之间的请求与流量。expires会给一个资源设定一个过期时间，每次请求时会比对服务器该文件最后的更新时间有没有变化，没有则使用缓存，返回状态码304，发生了变化则去服务器进行请求，返回状态码200。

**配置：**

````
server {
        listen       80;
        server_name  自己的服务器，本地使用localhost;

        location / {
            root   html;
            index  index.html index.htm;
        }

        location ~ /files/ {
        	# 文件所在的路径
            root   /image/;
            index  index.html index.htm;
            # 可以让文件以列的形式展示
            autoindex on;
        }

        location ~ /html/ {
            root   /image/;
            index  index.html index.htm;
        }

        location ~ /imgFiles/ {
            root   /image/;
            index  index.html index.htm;
        }
}
````

![image-20200928134535721](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928134535721.png)

![image-20200928134557297](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928134557297.png)

## 6、nginx配置高可用集群

![image-20200928135338515](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928135338515.png)

**高可用：**服务器宕机之后仍然可以实现请求。

**前提：**

- 需要两台nginx服务器。

- 需要keepalived。
- 需要虚拟ip。

![image-20200928140721293](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928140721293.png)

````
# 创建对应的nginx容器
FROM centos

MAINTAINER tanxiang<1571922819@qq.com>

ADD nginx-1.18.0.tar.gz /usr/

RUN yum -y install vim
RUN yum -y install make
# 安装keepalived
RUN yum -y install keepalived
RUN yum -y install pcre-devel
RUN yum -y install gcc gcc-c++ kernel-devel
RUN yum install -y pcre pcre-devel
RUN yum install -y zlib zlib-devel
RUN yum install -y openssl openssl-devel

RUN cd /usr/nginx-1.18.0/ && ./configure && make && make install

ENV LANG C.UTF-8
ENV TPATH /usr/local/
WORKDIR $TPATH

EXPOSE 80

CMD /bin/bash
````

**主服务器的配置：**

````
global_defs {
   notification_email {
     acassen@firewall.loc
     failover@firewall.loc
     sysadmin@firewall.loc
   }
   notification_email_from Alexandre.Cassen@firewall.loc
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   router_id LVS_DEVEL
   vrrp_skip_check_adv_addr
   vrrp_strict
   vrrp_garp_interval 0
   vrrp_gna_interval 0
}

vrrp_script chk_http_port {
	#脚本的放置地址
	script "/usr/local/src/nginx_check.sh" 
	# 脚本检测时间
	interval 2 				
	weight 2
}

vrrp_instance VI_1 {
    state MASTER 			# 备用机改为BACKUP
    interface eth0			# 网卡
    virtual_router_id 51 	#主、备机的virtual_route_id必须相同
    priority 100 			# 优先级，主机值较大、备份机较小
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {		# vrrp 虚拟地址
        192.168.200.16
        192.168.200.17
        192.168.200.18
    }
}
````

**脚本：**

````
#!/bin/bash
A=`ps -C nginx -no-header |wc -l`
if [ $A -eq 0 ];then
	/usr/local/nginx/sbin/nginx
	sleep 2
	if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
		killall keepalived
	fi
fi 
````

**启动keepalived：**

````
systemctl start keepalived.service
````

**如果使用docker容器不能启动：**

````
重新创建一个容器，命令为(启动后可以开启服务)：
docker run -itd   --privileged --name 容器名 镜像名 /usr/sbin/init
docker exec -it 容器名 /bin/bash
````

## 7、nginx原理

**1、master和worker进程**

![image-20200928191943077](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928191943077.png)

![image-20200928192025177](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928192025177.png)

**2、worker工作机制：**

客户端发送请求，master进行接受后，下发给worker，worker之间进行争抢，争抢到的worker去对请求进行处理（因为nginx不支持Java，所以worker还需要进行请求转发、反向代理等，worker使用的是**争抢机制**）。

![image-20200928192411460](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928192411460.png)

**3、一个master和多个worker的好处：**

(1)、可以使用nginx -s reload 热部署。

(2)、每个worker都是独立的进程，不需要加锁，省去了锁的开销。

(3)、一个进程退出之后，其他的进程还在工作，服务不会中断，master进程则很快启动新的worker进程。

(4)、worker进程异常退出，程序出现bug，只是会导致当前的worker上的所有请求失败，不会影响到所有的请求，降低的风险。

**4、设置多少个worker：**

nginx和redis类似，都采用了io多路复用机制，每一个worker都是独立的进程，但每个进程里只有一个主线程，通过异步阻塞的方式来处理请求，每个worker的线程可以把一个cpu的性能发挥到机制，**==所以worker数和服务器的cpu数相等最为适宜==**。设少了会浪费cpu，设多了会造成cpu频繁切换上下文带来的损耗。

**5、连接数worker_connection：**

**发送一个请求会占用2个（请求静态文件系统）或4个（Nginx不支持Java，所以可能还需要连接tomcat）连接数。**

![image-20200928194942593](/home/tan/桌面/Typora文件/Typora图片/Nginx/image-20200928194942593.png)

**一个nginx能建立的最大连接数应该是（worker_processes  * worker_connection）。**

**最大并发数：**

worker_connection：worker最大连接数。

worker_processes：worker数。

- 普通的静态访问最大并发数是：**（worker_connection * worker_processes）/2**。
- 如果是HTTP作为反向代理来说，最大的并发数量应该是**（worker_connection * worker_processes）/4**。