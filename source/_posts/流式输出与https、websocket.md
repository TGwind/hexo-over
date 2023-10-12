---
title: 微信小程序websock实现流式输出
tags:
- WebSocket
---

openai流式输出项目地址：https://github.com/Grt1228/chatgpt-steam-output

小程序端使用websocket连接springboot

springboot在内置的tomcat要配置https证书，可以用阿里云的免费证书

阿里云下载tomcat版的证书，解压后

![image-20230506205525109](http://myblog.over2022.top/image-20230506205525109.png) 

将pfx文件复制到项目resource路径下

![image-20230506205618624](http://myblog.over2022.top/image-20230506205618624.png) 

编辑application.properties

```properties
spring.web.resources.static-locations= classpath:/static/, classpath:/templates/
chatgpt.apiKey=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
chatgpt.apiHost=https://api.openai.com/
server.max-http-header-size=81920
#项目端口号
server.port=33117
#证书位置
server.ssl.key-store=classpath:chat.pfx
#证书密码，在解压后的txt文件里面
server.ssl.key-store-password=qwcowq9j

```

重定向

在启动类底下添加；(把指定的*http8080*端口重定向到*https8888*)

```java
@Bean

    public ServletWebServerFactory servletContainer() {

        TomcatServletWebServerFactory tomcat = new TomcatServletWebServerFactory();

        tomcat.addAdditionalTomcatConnectors(createHTTPConnector());

        return tomcat;

    }

    private Connector createHTTPConnector() {

        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");

        //同时启用http（8080）、https（8443）两个端口

        connector.setScheme("http");

        connector.setSecure(false);

        connector.setPort(8080);

        connector.setRedirectPort(443);

        return connector;

    }
```

443端口就是https端口号

打包上传服务器

### Nginx配置

```nginx
user  www www;
worker_processes auto;
error_log  /www/wwwlogs/nginx_error.log  crit;
pid        /www/server/nginx/logs/nginx.pid;
worker_rlimit_nofile 51200;

stream {
    log_format tcp_format '$time_local|$remote_addr|$protocol|$status|$bytes_sent|$bytes_received|$session_time|$upstream_addr|$upstream_bytes_sent|$upstream_bytes_received|$upstream_connect_time';
  
    access_log /www/wwwlogs/tcp-access.log tcp_format;
    error_log /www/wwwlogs/tcp-error.log;
    include /www/server/panel/vhost/nginx/tcp/*.conf;
}

events
    {
        use epoll;
        worker_connections 51200;
        multi_accept on;
    }

http
    {
        include       mime.types;
		#include luawaf.conf;

		include proxy.conf;

        default_type  application/octet-stream;

        server_names_hash_bucket_size 512;
        client_header_buffer_size 32k;
        large_client_header_buffers 4 32k;
        client_max_body_size 50m;

        sendfile   on;
        tcp_nopush on;

        keepalive_timeout 60;

        tcp_nodelay on;

        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
        fastcgi_buffer_size 64k;
        fastcgi_buffers 4 64k;
        fastcgi_busy_buffers_size 128k;
        fastcgi_temp_file_write_size 256k;
		fastcgi_intercept_errors on;

        gzip on;
        gzip_min_length  1k;
        gzip_buffers     4 16k;
        gzip_http_version 1.1;
        gzip_comp_level 2;
        gzip_types     text/plain application/javascript application/x-javascript text/javascript text/css application/xml;
        gzip_vary on;
        gzip_proxied   expired no-cache no-store private auth;
        gzip_disable   "MSIE [1-6]\.";

        limit_conn_zone $binary_remote_addr zone=perip:10m;
		limit_conn_zone $server_name zone=perserver:10m;

        server_tokens off;
        access_log off;

server
    {
        listen 888;
        server_name phpmyadmin;
        index index.html index.htm index.php;
        root  /www/server/phpmyadmin;

        #error_page   404   /404.html;
        include enable-php.conf;

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }

        location ~ .*\.(js|css)?$
        {
            expires      12h;
        }

        location ~ /\.
        {
            deny all;
        }

        access_log  /www/wwwlogs/access.log;
    }
    
server {


        #charset koi8-r;

        #access_log  logs/host.access.log  main;


      }
map $http_upgrade $connection_upgrade { 
	default upgrade; 
	'' close; 
} 
upstream wsbackend{ 
	server 127.0.0.1:33117; 
	keepalive 1000; 
} 
server{
     #监听的端口号
        listen      443 ssl;
        #设置访问的二级域名
        server_name  chat.over2022.top;
	ssl_certificate    /www/server/nginx/cert/top.pem;
	ssl_certificate_key /www/server/nginx/cert/top.key;
	ssl_session_timeout 20m;
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_prefer_server_ciphers on;
	ssl_verify_client off;
	
	location /{
	  proxy_http_version 1.1;
	  proxy_pass http://wsbackend;
	  proxy_redirect off; 
	  proxy_set_header Host $host; 
	  proxy_set_header X-Real-IP $remote_addr; 
	  proxy_read_timeout 3600s; 
	  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
	  proxy_set_header Upgrade $http_upgrade; 
	  proxy_set_header Connection $connection_upgrade; 
	}
	
	 location /websocket {
        #配置访问的项目路径（注:这里重点）
       proxy_pass  http://wsbackend/;
        # root html;
       # index index.html index.htm;
        proxy_set_header           Host $host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header           X-Forwarded-For       $proxy_add_x_forwarded_for;
        client_max_body_size  100m;
        root   html;
        index  index.html index.htm;
        }
}
server {
    listen 80;
    server_name huiblog.top;
    #将请求转成https
    rewrite ^(.*)$ https://$host$1 permanent;
}

include /www/server/panel/vhost/nginx/*.conf;
}


```

进入项目目录运行

```sh
nohup java -jar 项目.jar
```

在浏览器访问 https://chat.over2022.top:33117即可打开项目

小程序内可通过 wss://chat.over2022.top:33117/websocket 即可通过websocket连接后端（要提前配置合法域名）

参考教程：

[SpringBoot - 内置的Tomcat服务器配置详解](https://www.hangge.com/blog/cache/detail_2457.html#:~:text=SpringBoot%20-%20%E5%86%85%E7%BD%AE%E7%9A%84Tomcat%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%85%8D%E7%BD%AE%E8%AF%A6%E8%A7%A3%EF%BC%88%E9%99%84%EF%BC%9A%E5%90%AF%E7%94%A8HTTPS%E6%9C%8D%E5%8A%A1%EF%BC%89%202019-08-28%20%E5%8F%91%E5%B8%83%EF%BC%9Ahangge%20%E9%98%85%E8%AF%BB%EF%BC%9A26011%20%E5%9C%A8%20Spring,%E7%AD%89%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%B9%E5%99%A8%E3%80%82%20%E5%BD%93%E6%88%91%E4%BB%AC%E6%B7%BB%E5%8A%A0%E4%BA%86%20spring-boot-starter-web%20%E4%BE%9D%E8%B5%96%E5%90%8E%EF%BC%8C%E9%BB%98%E8%AE%A4%E4%BC%9A%E4%BD%BF%E7%94%A8%20Tomcat%20%E4%BD%9C%E4%B8%BA%20Web%20%E5%AE%B9%E5%99%A8%E3%80%82)

[springboot配置https](https://blog.csdn.net/yucaifu1989/article/details/124384022)
