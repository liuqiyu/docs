# nginx.conf

`nginx.conf`就是nginx服务器的配置文件。

* 配置文件

```json
# 工作进程数，和CPU核数相同
worker_processes 1;

events {
# 每个进程允许的最大连接数
    worker_connections 1024;
}
http {
# 负载均衡就靠它
# 语法格式：upstream name {}
# 里面写的两个server分别对应着不同的服务器
    upstream firstdemo {
        server 39.106.145.33;
        server 47.93.6.93;
    }
	
    server {
		listen       1993;
		server_name  www.abab.com;
		location / {
			root   html;
			index  index.html index.htm;
		}
	}
	
	server {
# isten监督端口号
		listen      4567;
		server_name www.example.com;
# location / {}访问根路径
		location / {
# 实现反向代理
# proxy_pass http://firstdemo，代理到firstdemo里两个服务器上
			proxy_pass http://192.168.220.189:93;
		}
	}
}
```