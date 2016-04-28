---
layout: post
---
docker swarm 简单使用
### docker swarm 简单使用

检查节点Docker配置

打开Docker配置文件（示例是centos 7）

	vim /etc/sysconfig/docker


添加-H tcp://0.0.0.0:2375到OPTIONS

	OPTIONS='-g /cutome-path/docker -H tcp://0.0.0.0:2375'



CentOS6.6 需要另外添加-H unix:///var/run/docker.sock

	OPTIONS='-g /mnt/docker -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock'


分别给A、B节点安装swarm

	$ docker pull swarm


生成集群token（一次）

	$ docker run --rm swarm create
	6856663cdefdec325839a4b7e1de38e8


其中6856663cdefdec325839a4b7e1de38e8就是我们将要创建集群的token
添加节点A、B到集群

	$ docker run -d swarm join --addr=192.168.20.1:2375 token://6856663cdefdec325839a4b7e1de38e8
	
	$ docker run -d swarm join --addr=192.168.20.2:2375 token://6856663cdefdec325839a4b7e1de38e8


列出集群A、B节点

	$ docker run --rm swarm list token://6856663cdefdec325839a4b7e1de38e8
	
	192.168.20.1:2375
	192.168.20.2:2375


集群管理：
在任何一台主机A、B或者C（C：192.168.20.3）上开启管理程序。例如在C主机开启：

	$ docker run -d -p 8888:2375 swarm manage token://6856663cdefdec325839a4b7e1de38e8


现在你就可以在主机C上管理集群A、B：

	$ docker -H 192.168.20.3:8888 info
	$ docker -H 192.168.20.3:8888 ps
	$ docker -H 192.168.20.3:8888 logs ...


在集群上运行容器

	$ docker -H 192.168.20.3:8888 run -d --name web1 nginx
	$ docker -H 192.168.20.3:8888 run -d --name web2 nginx
	$ docker -H 192.168.20.3:8888 run -d --name web3 nginx
	$ docker -H 192.168.20.3:8888 run -d --name web4 nginx
	$ docker -H 192.168.20.3:8888 run -d --name web5 nginx


查看集群A、B内的容器

	$ docker -H 192.168.20.3:8888 ps -a