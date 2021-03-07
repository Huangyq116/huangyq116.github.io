---
title: Nginx配置文件
permalink: 
date: 2019-07-30 18:39:00
categories:
  - 4. ToolsNotes
  - Tool
tags:
  - Tool
---

### 学习网址

[nginx documentation — DevDocs](https://devdocs.io/nginx/) 



### 配置文件信息

```nginx
server {
   listen   80 ;
   listen   [::]:80  ipv6only=on;
   listen   443 default ssl;
   listen   [::]:443 default ssl;
   #listen   [::]:443 ssl ;
   #listen   443 ssl;
   ssl off; 
   ssl_certificate     /etc/nginx/sslkey/kcs.default.crt;
   ssl_certificate_key /etc/nginx/sslkey/kcs.default.key;

    server_name test1116.com;
    #max_ranges 0;
    autoindex on;
    autoindex_exact_size off;
    autoindex_localtime on;

    error_log /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    
　　#302跳转使用
    location = /test302.html {
        root html;
        rewrite ^/(.*)$  http://127.0.0.1/a/123.mp4 redirect;
        
    }
    
    location = /index12.html {
        root /home/root/auto_test/Report/html/;
    }


    location  /a/ {
          root html;                
          rewrite ^/(.*)$  http://127.0.0.1/b/123.mp4 redirect;
    }

　　# 配合html/php上传文件使用
    location ~ .*\.(php|php5)?$ {
        fastcgi_pass   127.0.0.1:9000;
        client_max_body_size 30m;
    #   fastcgi_param    HTTPS on;
        fastcgi_param HTTPS  $https if_not_empty;
        fastcgi_param  SCRIPT_FILENAME    /etc/nginx/html$fastcgi_script_name;
        include        fastcgi_params; 
        #include    fastcgi.conf;
    }


    error_page 405 =200 $uri;
    #error_page 405 =200 @405;
       #location @405 {
    #    root /etc/nginx/html;
           #root  /tmp/upload_tmp2;
    #    proxy_method GET;
       #} 


    #location /b {
    #      root html;
    #      rewrite /(.*)   http://127.0.0.1/c/yunkong.mp4 redirect;
    # }

    #location /c {
    #       root html;
    #       rewrite /(.*)  http://127.0.0.1/d/yunkong.mp4 redirect;
    # }
         
                
    #location /d {
    #       root html;
    #}
    
    location /refresh30.html {
           expires 30s;
        root html;        
    }

    location ~^/.* {
        root /etc/nginx/html;
        add_header 123 123;   #添加请求接口响应头信息        
        gzip on;  #响应信息是否进行压缩
#       gzip_types text/html text/plain application/javascript application/x-javascript text/javascript text/xml text/css;
        #gzip_vary on;
    }

}
```

