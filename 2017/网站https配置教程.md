## 为网站配置SSL

### Nginx on CentOS/RHEL 7

- ```shell
  yum -y install yum-utils
  ```

- ```shell
  yum-config-manager --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional
  ```

- 安装certbot

- ```
  sudo yum install certbot
  ```

- 自动认证

- ```shell
  certbot certonly
  ```

## nginx配置

```
#
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;


# 配置负载均衡
upstream message.jiangtao.tech {
    # server  172.104.87.49:9090 weight=5;
    server  139.162.78.252:9090 weight=5;
}

server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        # 将所有HTTP资源永久重定向到相应的HTTPS资源
            rewrite ^ https://$host$request_uri? permanent;
        }

        location ^~ /.well-known/ {
            root /usr/share/nginx/html/;
            try_files $uri =404;
        }
        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

server {
    listen       80;
    server_name  message.jiangtao.tech;

        location / {
                proxy_redirect off;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://message.jiangtao.tech;
                rewrite ^ https://$host$request_uri? permanent;
        }
        location ^~ /.well-known/ {
            root /usr/share/nginx/html/;
            try_files $uri =404;
        }
}
# Settings for a TLS enabled server.

    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  message.jiangtao.tech;
        root         /usr/local/tomcat8/webapps/tigase;

        ssl_certificate "/etc/letsencrypt/live/message.jiangtao.tech/fullchain.pem";
        ssl_certificate_key "/etc/letsencrypt/live/message.jiangtao.tech/privkey.pem";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

#        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://message.jiangtao.tech;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }
        #        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

}
```

