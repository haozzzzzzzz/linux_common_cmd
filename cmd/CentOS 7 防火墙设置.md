# CentOS 7 防火墙设置

## 停用防火墙

````shell
systemctl stop firewalld.service # 关闭防火墙
systemctl disable firewalld.service # 取消开机启动
````

## 开放端口

````shell
firewall-cmd --zone=public --add-port=80/tcp --permanent
````

**参数含义**

--zone #作用域

--add-port=80/tcp  #添加端口，格式为：端口/通讯协议

--permanent   #永久生效，没有此参数重启后失效