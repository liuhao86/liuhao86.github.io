---
layout: post
title:  "Nginx配置 stream ssl gzip"
date:  2017-06-14 14:00:51 +0800
categories:
  - Nginx
tag:
  - nginx

---

* content
{:toc}


### Nginx Stream配置
```
stream {
    upstream socket_5227 {
        hash $remote_addr consistent;
        server 172.20.9.46:5227 max_fails=3 fail_timeout=600s;
        server 172.20.9.47:5227 max_fails=3 fail_timeout=600s;
        server 172.20.9.48:5227 max_fails=3 fail_timeout=600s;
    }

    server {
        listen 5227;
        listen 5222;
        listen 5223;
        proxy_connect_timeout 600s;
        proxy_timeout 600s;
        proxy_pass socket_5227;
    }
}
```

### Websocket配置
合并ssl和plain端口, 注意务必取消`ssl on;`, 否则连接plain端口会出现`400`的错误码.
```

    server {
        listen 5220;
        listen 5229;
        listen 5221 ssl;
        server_name im01.upesn.com;
        #ssl on;
        ssl_certificate ssl.crt;
        ssl_certificate_key ssl.key;
        ssl_session_timeout 10m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";
        ssl_prefer_server_ciphers   on;

        location / {
            proxy_pass http://qrcode_login_socket;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            # WebSocket support (nginx 1.4)
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }

        access_log /data/www/nginx/logs/access_qrcode_login.log  access;
    }
```

`http`下需要配置:
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}
```

### gzip配置
```
gzip on;
gzip_min_length  1k;
gzip_buffers 4 16k;
gzip_http_version 1.0;
gzip_comp_level 2;
gzip_types text/plain application/x-javascript text/css application/xml application/json;
gzip_vary on;
gzip_proxied expired no-cache no-store private auth;
gzip_disable "MSIE [1-6]\.";
```
