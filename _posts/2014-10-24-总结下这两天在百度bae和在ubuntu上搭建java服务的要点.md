---
layout: post
---
总结下这两天在百度bae和在ubuntu上搭建java服务的要点


### 20150330 ubuntu环境安装
##### 在本地环境创建ssh key
ssh-keygen -t rsa

##### 将key上传到服务器
cat ~/.ssh/id_rsa.pub | ssh -i yangaw_aws_privatekeyfile.pem ubuntu@54.148.227.28 "cat >> .ssh/authorized_keys"


cat ~/.ssh/id_rsa.pub | ssh   root@107.170.219.187 "cat >> .ssh/authorized_keys"

##### 在本地快速登录服务器
在本地ssh目录下创建cofig文件
    Host aws1
        HostName 54.148.227.28
        User ubuntu

在terminal中ssh aws1即可

##### 安装tomcat7
sudo apt-get update
sudo apt-get install tomcat7
sudo apt-get install default-jdk
sudo apt-get install ant git


#### 安装nginx
    sudo apt-get install nginx
    sudo service nginx start
    update-rc.d nginx defaults
    
##### 测试nginx安装
    ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
    
    curl http://icanhazip.com
    
##### nginx配置php站点

    server {
    listen 80;

    root /home/www/shopper/phpmyadmin;
    index index.html index.htm index.php;

    # Make site accessible from http://localhost/
    server_name ad.aigo100.com;

    location / {
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
        try_files $uri $uri/ =404;
        # Uncomment to enable naxsi on this location
        # include /etc/nginx/naxsi.rules
    }

    # Only for nginx-naxsi used with nginx-naxsi-ui : process denied requests
    #location /RequestDenied {
    #   proxy_pass http://127.0.0.1:8080;    
    #}

    #error_page 404 /404.html;
        error_page 404 /404.html;
        error_page 500 502 503 504 /50x.html;
    location = /50x.html {
            root /usr/share/nginx/html;
        }

    location ~ \.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
}    

##### nginx配置目录
/etc/nginx/
检查配置是否正确
nginx -t


##### 安装mysql server
sudo apt-get install mysql-server
sudo mysql_install_db
sudo mysql_secure_installation

##### mysql 导入导出数据
mysql -u用户名 -p 数据库名 < 数据库名.sql 
mysqldump -uroot -p abc > abc.sql 


##### 修改mysql的my.cnf文件中的字符集键值

	1、在[client]字段里加入default-character-set=utf8，如下：
	
	[client]
	port = 3306
	socket = /var/lib/mysql/mysql.sock
	default-character-set=utf8
	
	2、在[mysqld]字段里加入character-set-server=utf8，如下：
	
	[mysqld]
	port = 3306
	socket = /var/lib/mysql/mysql.sock
	character-set-server=utf8
	
	3、在[mysql]字段里加入default-character-set=utf8，如下：
	
	[mysql]
	no-auto-rehash
	default-character-set=utf8
##### 安装php
sudo apt-get install php5-fpm php5-mysql

##### 配置php
sudo nano /etc/php5/fpm/php.ini

cgi.fix_pathinfo=0

sudo service php5-fpm restart

##### 配置nginx访问php页面

	server {
	    listen 80 default_server;
	    listen [::]:80 default_server ipv6only=on;
	
	    root /usr/share/nginx/html;
	    index index.php index.html index.htm;
	
	    server_name server_domain_name_or_IP;
	
	    location / {
	        try_files $uri $uri/ =404;
	    }
	
	    error_page 404 /404.html;
	    error_page 500 502 503 504 /50x.html;
	    location = /50x.html {
	        root /usr/share/nginx/html;
	    }
	
	    location ~ \.php$ {
	        try_files $uri =404;
	        fastcgi_split_path_info ^(.+\.php)(/.+)$;
	        fastcgi_pass unix:/var/run/php5-fpm.sock;
	        fastcgi_index index.php;
	        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	        include fastcgi_params;
	    }
	}
	
##### 安装phpmyadmin
sudo apt-get install phpmyadmin


#####  ubuntu 下mysql导入出.sql文件
 
	1.导出整个数据库
	
	　　mysqldump -u 用户名 -p 数据库名 > 导出的文件名
	
	　　mysqldump -u wcnc -p smgp_apps_wcnc > wcnc.sql
	
	　　2.导出一个表
	
	　　mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名
	
	　　mysqldump -u wcnc -p smgp_apps_wcnc users> wcnc_users.sql
	
	　　3.导出一个数据库结构
	
	　　mysqldump -u wcnc -p -d –add-drop-table smgp_apps_wcnc >d:\wcnc_db.sql
	
	　　-d 没有数据 –add-drop-table 在每个create语句之前增加一个drop table
	
	　　4.导入数据库
	
	　　常用source 命令
	
	　　进入mysql数据库控制台，
	
	　　如mysql -u root -p
	
	　　mysql>use 数据库
	
	　　然后使用source命令，后面参数为脚本文件(如这里用到的.sql)
	
	　　mysql>source /home/pt/test.sql
	或直接导入命令为： mysql -h localhost -u root -p temp
	
##### 	tomcat的nginx配置

	server {
	        listen 80 default_server;
	        listen [::]:80 default_server ipv6only=on;
	
	        root /home/www/shopper;
	        index index.html index.htm index.jsp;
	
	        # Make site accessible from http://localhost/
	        server_name 121.40.180.206;
	
	        location / {
	                # First attempt to serve request as file, then
	                # as directory, then fall back to displaying a 404.
	                try_files $uri $uri/ =404;
	                # Uncomment to enable naxsi on this location
	                # include /etc/nginx/naxsi.rules
	        }
	
	
	                location / {
	                # First attempt to serve request as file, then
	                # as directory, then fall back to displaying a 404.
	                try_files $uri $uri/ =404;
	                # Uncomment to enable naxsi on this location
	                # include /etc/nginx/naxsi.rules
	        }
	
	        # Only for nginx-naxsi used with nginx-naxsi-ui : process denied requests
	        #location /RequestDenied {
	        #       proxy_pass http://127.0.0.1:8080;
	        #}
	
	        #error_page 404 /404.html;
	        error_page 404 /404.html;
	        error_page 500 502 503 504 /50x.html;
	        location = /50x.html {
	                root /usr/share/nginx/html;
	        }
	
	}

## 百度bae
**一句话总结：非常坑爹，各种限制，除了访问速度快没有，好像没有优点了。（国内服务，速度好像。。。）**

1、创建应用引擎，这个不需要多说，照百度文档做就是了。（tomcat/git）
2、创建数据库服务器，256M内存，1G空间，很抠门，不过数据导入导出和phpMyadmin不需要自己安装，这个很方便。
3、数据导入以后，git clone https://git.duapp.com/XXXX 到本地
4、将本地代码打包成ROOT.war,注意数据链接需要修改成


    db.dialect= org.hibernate.dialect.MySQLDialect
    db.driverClass=com.mysql.jdbc.Driver
    db.url=jdbc:mysql://sqld.duapp.com:4050/dbname
    db.user=api key
    db.password=secret key

5、git push origin master 将代码提交到服务器
6、如果服务发布出错，可以在

关闭mysql

    stop msyql
    这条命令不顶用：sudo mysqladmin shutdown -u root -p
启动mysql

    start mysql
    ／／sudo mysqld_safe -user=mysql &

## 在ubuntu上安装开发环境

#### 安装git
    sudo apt-get install -y git-core

#### 安装heroku
    wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

#### 安装nginx
    sudo apt-get install nginx
    sudo service nginx start
    update-rc.d nginx defaults

##### 配置ssh 解决permission deny
    cat ~/.ssh/id_rsa.pub | ssh -i aws.pem ubuntu@ec2-54-201-40-222.us-west-2.compute.amazonaws.com "cat >> .ssh/authorized_keys"

    cat ~/.ssh/id_rsa.pub | ssh   root@107.170.219.187 "cat >> .ssh/authorized_keys"

   在服务器上添加key

#### 安装nodejs
    sudo apt-get update
     ＃Install a special package
     sudo apt-get install -y python-software-properties python g++ make
     # Add a new repository for apt-get to search
     sudo add-apt-repository ppa:chris-lea/node.js
     # Update apt-get’s knowledge of which packages are where
    sudo apt-get update
    # Now install nodejs and npm
     sudo apt-get install -y nodejs

#### 安装forever
    npm install forever -g

    forever start --spinSleepTime 10000 main.js

    forever start --spinSleepTime -s 10000 main.js

    $ forever start app.js          #启动
    $ forever stop app.js           #关闭
    $ forever start -l forever.log -o out.log -e err.log app.js   #输出日志和错误

命令语法及使用 https://github.com/nodejitsu/forever

#### 在nginx中配置nodejs app
    nano /etc/nginx/conf.d/example.com.conf
    

    server {
      listen 80;
    
      server_name www.salehao.com;
    
      location / {
          proxy_pass http://localhost:2368;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
      }
    }

#### 安装ghost
http://ghosted.co/install-ghost-digitalocean/

###### 下载ghost

    cd /path/to/unzipped/ghost/folder
    scp -r ghost-0.4.2.zip root@*your-server-ip*:~/www/ghost

###### 解压和安装

    cd ~/www/ghost
    unzip ghost-0.4.2.zip
    
###### 修改config.sample.js中的域名和端口

    npm install --production
    npm start

###### 在nginx中配置ghost
安装ngingx，如果没有
    
    apt-get install nginx


创建salehao的配置文件

    cd /etc/nginx/conf.d
    touch www.salehao.com.conf
    
    nano  www.salehao.com.conf

    server {                                                                        
        listen 80;                                                            
        server_name  www.salehao.com;                                     
    
        location / {                                                            
                proxy_pass http://localhost:2368/;                              
                proxy_set_header Host $host;                                    
                proxy_buffering off;
             proxy_redirect off;
        }                                                                       
    }
    
##### 重启nginx
     service nginx restart
     
##### 安装forever
     npm install -g forever
     
##### 启动ghost     
     forever start --spinSleepTime 10000 ghost.js
    
#### 在ubuntu上配置第二个ghost

下载ghost 

git clone 

解压到web目录
安装

npm install --produciton
修改端口等
nano config.js

启动
npm start

增加nginx配置
cp xx.confg yy.config

重启ngnix
    service nginx restart
done
     

#### 安装mysql
    sudo apt-get update
    sudo apt-get install mysql-server php5-mysql
设置密码:root12345678
###### 启动
    sudo mysql_install_db
    sudo /usr/bin/mysql_secure_installation

#### 安装php

    sudo apt-get install php5-fpm
###### 编辑配置文件
    sudo nano /etc/php5/fpm/php.ini
         cgi.fix_pathinfo=0
    sudo nano /etc/php5/fpm/pool.d/www.conf
         listen = /var/run/php5-fpm.sock
    sudo service php5-fpm restart
###### 编辑nginx
    sudo nano /etc/nginx/sites-available/default
    [...]
    server {
            listen   80;
            root /usr/share/nginx/www;
            index index.php index.html index.htm;
    
            server_name example.com;
    
            location / {
                    try_files $uri $uri/ /index.html;
            }
    
            error_page 404 /404.html;
    
            error_page 500 502 503 504 /50x.html;
            location = /50x.html {
                  root /usr/share/nginx/www;
            }
    
            # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
            location ~ \.php$ {
                    try_files $uri =404;
                    fastcgi_pass unix:/var/run/php5-fpm.sock;
                    fastcgi_index index.php;
                    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                    include fastcgi_params;
            }
    
    }
    [...]
    
    sudo service nginx restart

#### 安装phpmyadmin

    sudo apt-get install phpmyadmin
    sudo ln -s /usr/share/phpmyadmin/ /usr/share/nginx/www
    sudo service nginx restart
缺少 mcrypt 扩展

    sudo apt-get install libmcrypt4 php5-mcrypt
    sudo service nginx restart


#####  查看已登录用户

    who
剔除用户

    pkill -KILL XX
    pkill XX

#### 安装mongodb

    执行mongobd_install.bash
    sudo bash mongodb_install.bash

    apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
    echo "deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen" | tee -a /etc/apt/sources.list.d/10gen.list
    apt-get -y update
    apt-get -y install mongodb-10gen

查看运行状态

    ps -ef | grep mongo

manage mongodb with shell
>mongo
>show dbs
>use test
>show collections
>show user;
>show logs;
>db.test.find();

##### 安装mongodb for node
http://127.0.0.1:28017/

    npm install mongodb

test code with node

    var MongoClient = require("mongodb").MongoClient;
    var format = require('util').format;

    MongoClient.connect('mongodb://127.0.0.1:27017/test', function(err, db) {
            if (err) throw err;
    
        var collection = db.collection('test_insert');
        collection.insert({
            a: 2
        }, function(err, docs) {
            collection.count(function(err, count) {
                console.log(format("count = %s", count));
            });
        });
    
        // Locate all the entries using find
        collection.find().toArray(function(err, results) {
            console.dir(results);
            // Let's close the db
            db.close();
        });
    });
    
http://blog.csdn.net/mydeman/article/details/6921723
https://github.com/mongodb/node-mongodb-native
http://mongoosejs.com/docs/index.html


###### 使用db.js
    var mongo=require("mongo");
    var Db=mongo.Db;
    var Connection=mongo.Connection;
    var DbServer=mongo.Server;
    
    var mongodb=function(){
         new Db(dbname,new DbServer(host,port,{}));
    }
    module.exports=mongodb;

###### 使用配置文件config.js
    module.exports={
    dbname:xx,
    dburl:xx,
    dbport:xx
    };

###### 具体访问数据库
    mongodb.open(function(err,db){
         if(err){};
         db.collection("table",function(err,collections){
              if(err){ db.close();callback(err);};
              collections.ensureIndex();
              collections.find().sort().toArray(function(err,result){
                        db.close();
                        callback();
                    });
              collections.insert(data,option,function(err,result){
                        db.close();
                        callback();
                   });
         });
    });




###### ftp 550 permission denied proftpd

    chown -R (FTPUSER) /(path)/(to_your_ftp)/
    
    Or in my case:
    
    chown -R wordpress /home/wordpress/public_html




#### 一些最近关注的nodejs开源项目

* https://github.com/nswbmw/N-blog
* 
* https://github.com/cnodejs/nodeclub/
* 
* https://github.com/TryGhost/Ghost
* 
* http://keystonejs.com/guide/
* 
* https://github.com/nswbmw/N-blog/wiki/_pages
* 
* https://github.com/JedWatson/sydjs-site
* 
* https://github.com/bevry/docpad
* 
* https://github.com/nswbmw/N-blog/wiki/_pages
* https://github.com/nswbmw
* 
* https://github.com/linnovate/mean
* https://github.com/madhums/node-express-mongoose-demo
* https://github.com/madhums/node-express-mongoose/wiki/Apps-built-using-this-approach
* 
* https://github.com/madhums/node-express-mongoose
* https://github.com/ccoenraets/nodecellar
* https://github.com/andzdroid/mongo-express
* https://github.com/ijason/NodeJS-Sample-App
* 
* http://nodeapi.ucdok.com/api/crypto.html
* https://github.com/visionmedia/express/tree/master/examples
* https://github.com/mnutt/hummingbird
