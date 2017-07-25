# Nginx配置Yii2 Rewrite

Yii2支持不同形式的url重写，同时需要配置Nginx和Appache才可以正确使用。主要原理是通过url可以正常访问到yii框架。

## 一般配置

一般情况下，Nginx的`root`是指向Yii2站点的web目录下的，里面就包含这Yii2站点的启动文件`index.php`。这种情况下需要配置

````nginx
server{
  location / {
    if( !-e $request_filename){
      rewrite ^/(.*) /index.php last;
      
    }
  }
}
````

## 多重目录

如果Yii站点是存在于多层目录下的，需要正确配置`rewrite`命令。例如`root`为`/mnt/hfgs/htdocs`的nginx配置。配置文件形如：

````nginx
server {
    listen       80;
    server_name  localhost 10.5.1.110;

    root   /mnt/hgfs/htdocs;

    #charset koi8-r;
    access_log  /var/log/nginx/log/localhost.access.log  main;
    error_log /var/log/nginx/log/localhost.error.log;

    location / {
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
        index  index.html index.htm index.php;

    	# Yii路由重写
        if ( !-e $request_filename){
            rewrite ^(/.*/web)/(.*) $1/index.php last;
        }
    }

    # (optional) nginx status page
    location /nginx_status{
        stub_status on;
        access_log off;
    }

    # (optional) php-fpm status page
    location ~ ^/(status|ping)$ {
        include fastcgi_params;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_param SCRIPT_FILENAME $fastcgi_script_name;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    #location = /50x.html {
    #    root   /usr/share/nginx/html;
    #}

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}

````

