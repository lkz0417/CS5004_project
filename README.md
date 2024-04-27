This project is an online takeout ordering system developed using the Spring Boot framework.

Technology stack
Backend framework
SpringBoot (3.1.2)
mybatis
database
MySql
Redis
Front-end framework
Vue
Uniapp -ElementUI
Front-end and back-end communication
RESTful API
Windows development environment setup
Install Java JDK 17 and configure environment variables

Install MySQL and Redis databases and create corresponding databases

Install Maven

Download and install Nginx and complete the following configurations

# Configure the following content under the http item

map $http_upgrade $connection_upgrade{
	default upgrade;
	'' close;
}

upstream webservers{
  server 127.0.0.1:8080 weight=90 ;
  #server 127.0.0.1:8088 weight=10 ;
}

server {
    listen       80;
    server_name  localhost;

    location / {
        root   html/sky;
        index  index.html index.htm;
    }

    # Reverse proxy, handles requests sent by the management end
    location /api/ {
		proxy_pass   http://localhost:8080/admin/;
        #proxy_pass   http://webservers/admin/;
    }

	# Reverse proxy, handles requests sent by the client
    location /user/ {
        proxy_pass   http://webservers/user/;
    }

	# WebSocket
	location /ws/ {
        proxy_pass   http://webservers/ws/;
		proxy_http_version 1.1;
		proxy_read_timeout 3600s;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "$connection_upgrade";
    }

    location /media {
        root configures the media file location; # eg: D:/static
         # Note: Create a media folder in the D:/static directory
    }
}


Modify the configuration file application.yml

spring:
  datasource:
    url: jdbc:mysql://url
    username: root
    password: database password
  data:
    redis:
      password: redis database password
Create a new application-env.yml file in the resources directory and write the following configuration

sky:
  wechat:
    appid: 申请微信小程序可获得
    secret: 申请微信小程序可获得
    mchid: 商户号
    mchSerialNo:
    privateKeyFilePath:
    apiV3Key:
    weChatPayCertFilePath:
    notifyUrl:
    refundNotifyUrl:
run final project/sky-take-out/sky-server/src/main/java/com/sky/SkyApplication.java
