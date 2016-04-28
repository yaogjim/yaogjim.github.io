---
layout: post
---
Docker中Ubuntu镜像添加openssh-server服务，有需要的朋友可以参考下。

### Docker中Ubuntu镜像添加openssh-server服务


##### 1，首先，需要从docker官网获得centos或Ubuntu镜像

##### 2，当本地已有Ubuntu镜像后（大概200M左右大小），
使用如下命令

	docker run -t -i ubuntu /bin/bash
即可启动一个容器，并放入Ubuntu镜像

##### 3，更新源, apt-get update

接着就可以使用 apt-get install openssh-client openssh-server 来安装openssh服务了

需要把此镜像保存一下：

	docker commit [container-id] [image-id]
在把刚刚的container干掉：

	docker stop [container-id]
嗯，还需要将这个container删除掉

	docker rm [container-id]

最后，加载刚刚保存到的最新的image，放入到新的容器中去：
	docker run --name [image-name] -i -t -p 50001:22 [image-id]

##### 4，启动openssh服务

	/etc/init.d/ssh start

##### 5，此时可以从其他机器登陆到这个docker容器里了

##### 6，可能出现一些错误使得一登陆进去就直接关闭连接了：

	[root@Wshare84 start_docker_sh]# ssh root@10.10.2.84 -p 50001
	The authenticity of host '[10.10.2.84]:50001 ([10.10.2.84]:50001)' can't be established.
	RSA key fingerprint is aa:05:84:4c:f2:15:f3:04:89:9c:04:33:0d:15:14:1f.
	Are you sure you want to continue connecting (yes/no)? yes
	Warning: Permanently added '[10.10.2.84]:50001' (RSA) to the list of known hosts.
	root@10.10.2.84's password: 
	Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 2.6.32-431.el6.x86_64 x86_64)
	
	 * Documentation:  https://help.ubuntu.com/
	Last login: Wed Jan 21 01:25:17 2015 from 172.17.42.1
	Connection to 10.10.2.84 closed.

此时解决方案：
	ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
	ssh-keygen -t rsa -f  /etc/ssh/ssh_host_rsa_key
	
	echo 'root:yourpasswd' | chpasswd //设置root密码
	
	 vi  /etc/ssh/sshd_config 
	将PermitRootLogin 改为 yes，将UsePAM 改为 no。

重启服务：

	/etc/init.d/ssh restart