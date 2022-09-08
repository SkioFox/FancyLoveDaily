# 项目介绍

来了来了，他来了，整套婚礼结婚用的项目来了，前端程序员的福利项目

基于云函数的原生微信小程序

基于VUE的H5抽奖+弹幕

基于nest框架的node后端,配合H5使用

具体内容可具体见各个项目下的README文档

## 云函数现在改成免费1个月，后面收费了，给为量力而行吧，不愧是TX。
会在22年底完成API重构吧。（说不准会🕊）
提供两个版本吧。
一个nestjs写的独立后台版本替代云函数。
一个是原来的。

## 难度说明（根据各自能力选择适合自己的就好，难度高适合横向提高自己能力）

**入门※**：直接使用wedding-weapp，创建自己的婚礼小程序，难度最低

**进阶※※※**：在入门的基础上，额外增加weeding-nest-server+wedding-lucky-h5，在小程序的基础上，额外扩展了婚礼现场H5弹幕+抽奖功能，只需要用本地开发环境将前后端跑起来即可使用

**终级※※※※※**:在进阶的基础上，把前后端项目打包，将项目部署在服务器上（选配购买云服务器+域名），需要一定的linux和nginx基础,附我自己云服务器的nginx配置

## nginx配置参考（搭配终极）
```nginx
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user root;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}
http {
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
    '$status $body_bytes_sent "$http_referer" '
    '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    gzip on;
    gzip_min_length 1k;
    gzip_buffers 4 16k;
    #gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
    gzip_vary off;
    gzip_disable "MSIE [1-6]\.";


    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name www.xtybusiness.cn;
        root /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location /api/ {
            proxy_pass http://127.0.0.1:3000/; # 转发规则
            proxy_set_header Host $proxy_host; # 修改转发请求头，让8080端口的应用可以受到真实的请求
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        location /wedding {
            alias /root/workspace/wedding-fullstack/wedding-lucky-h5/dist;
            index index.html;
        }

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
}

```
