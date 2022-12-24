# Nginx详解

以下知识点是 @b站狂神说 Nginx课程的笔记

> 什么是Nginx？

Nginx是一个高性能的HTTP和反向代理的Web服务器。

Nginx优点：

- 负载均衡：

假设：Nginx代理服务器占了80端口，现在有其他多台服务器，分别是81端口64存储空间，82端口具有32存储空间，83端口具有8存储空间，那么Nginx就会把任务更多地分配给81端口，使得减少83端口因为存储空间不够导致的排队现象。

```
//NGINX.CONF
upstream MQ{
    //负载均衡的设置
    server 127.0.0.1:81 weight = 8
    server 127.0.0.1:82 weight = 1
}
upstream MQ1{
    server 127.0.0.1:83 weight = 8
    server 127.0.0.1:84 weight = 1
}
```

- 反向代理：后端的代理服务器，当一台服务器的存储空间不够，需要横向延伸更多台服务器，但是又希望客户端对于这前台服务器访问的又是同一个地址，可以使用Nginx去进行的一个反向代理，客户端把请求发给Nginx这台代理服务器，然后由他去进行相应的转发。

```
//NGINX.CONF
server{
    listen 80;
    server_name localhost;
    //代理 默认的端口
    location / {
        proxy_pass http://MQ
    }

    location /admin {
        proxy_pass http://Mq1
    }
}
```

- 占用内存少

- 并发能力强

架构：把横向的服务器管理变成树状图
