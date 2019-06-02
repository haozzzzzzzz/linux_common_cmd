# Nginx配置示例



## 跨域、proxy_pass header

```nginx
location /api/home_manage/ {
	if ($request_method = 'OPTIONS') { 
            add_header Access-Control-Allow-Origin *; 
            add_header Access-Control-Allow-Credentials true; 
            add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS'; 
            add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type'; 
            return 204; 
    	}

	rewrite /api/(.*) /$1 break;

        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Headers *;
        add_header Access-Control-Allow-Methods "GET,PUT,POST,DELETE,HEAD,OPTIONS";

	proxy_redirect off;
        proxy_connect_timeout 10s;
        proxy_read_timeout 2s;
        proxy_send_timeout 10s;
	proxy_set_header        Host $host;
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        X-Real-PORT $remote_port;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;

	proxy_pass              http://127.0.0.1:18101;
        allow 103.84.19.39;
        deny all;
}
```

