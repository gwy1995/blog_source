---
title: nginx学习笔记1
tags: web
date: 2018-09-10 17:35:48
---


## nginx简介

## nginx启动、停止以及重读配置文件
nginx的启动命令：
```
nginx
```
nginx启动后，可以使用`-s`参数进行控制：
```
nginx -s signal
```
其中`signal`可以使用以下值：
- stop   -- 迅速停止
- quit   -- 优雅停止：等待worker进程完成当前请求后关闭服务
- reload -- 重读配置文件
- reopen -- 重新打开日志文件
<!-- more -->
一旦主进程接收到重载配置文件的命令后，它会先检查配置文件语法的合法性，如果没有错误，则会重新加载配置文件。如果成功，则主进程会重新创建一个子进程并且发送关闭请求给以前的子进程。如果没有成功，主进程会回滚改动并且继续使用以前的配置。老的子进程在接受关闭的命令后，会停止接受新的请求并且继续处理当前的请求，直到处理完毕。之后，该子进程就直接退出了。 

## nginx配置
nginx以及模块的运行都是由配置文件决定的，默认配置文件名为`nginx.conf`，一般在`/usr/local/nginx/conf`，`/etc/nginx`，`/usr/local/etc/nginx`路径下。

### 配置结构
nginx是由一些模块组成的，配置文件中的指令控制这些模块。指令分为简单指令和块级指令。简单的指令由名称和参数组成，名称和参数之间由空格分隔，结尾是一个分号`;`。块级指令与简单指令的结构相似，但是末尾不是分号，而是用大括号`{`和`}`包裹的指令集。如果一个块级指令包含了其他指令，那么这就成为上下文。（如：`events`,`http`,`server`,`location`）

在配置文件中，没有放在任何上下文的指令都是处在主上下文中，如`events`和`http`指令在主上下中。`server`在`http`中，`location`在`server`中。

以`#`开头的行会被作为注释。



### 静态文件服务
web服务器的一个重要任务就是提供静态文件服务（如图片或者静态html页面）。

```
http {
    server {
    }
}
```
`server`区块包含在`http`模块中，一个配置文件可能包含多个`server`区块，这些`server`区块可能监听不同域名的不同端口。一旦nginx选择了一个`server`处理当前请求，它会根据在`server`块中定义好的`location`指令的参数，来匹配请求头中指定的URI。

```
server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```

这是一个可用的服务器配置，监听标准的`80`端口，在本机上可以使用`http://localhost/`进行访问，对于`/images/`开头的uri请求，服务器会返回`/data/images`目录，如`http://localhost/images/example.png`请求，它会返回文件`/data/images/example.png`，如果文件不存在，则返回`404`错误。对于不是`/images/`开头的uri请求，请求将映射到`/data/www`目录下。如`http://localhost/some/example.html`将映射到文件`/data/www/some/example.html`


我的静态服务配置：
```
location /static {
	root   path;
	autoindex on;             #开启索引功能
    autoindex_exact_size off; # 关闭计算文件确切大小（单位bytes），只显示大概大小（单位kb、mb、gb）
    autoindex_localtime on;   # 显示本机时间而非 GMT 时间
}
```

### 搭建一个简单的代理
nginx的一个频繁的用途就是搭建一个代理服务，即服务接收请求，然后将它们传到被代理的服务，然后服务取回处理结果，再传送到客户端。

接下来将搭建一个基本的代理服务，仅返回本地图片文件，而对于其他请求，都转发到另一个被代理服务。两个服务都将配置在同一个nginx实例中。

先搭建一个被代理的服务(这只需在原nginx配置文件中多加一个server块)：
```
server {
    listen 8080;
    root /data/up1;

    location / {
    }
}
```
这个服务器将监听8080端口，然后将请求映射到`/data/up1`的本地文件路径下。

然后是代理服务的配置：
```
server {
    location / {
        proxy_pass http://localhost:8080;
    }

    location /images/ {
        root /data;
    }
}
```

第一个块的配置负责将请求转发到8080端口。
第二个块的配置负责将`/images/`前缀的请求映射到本地文件目录`/data/images/`。

我们可以将第二个块修改成：
```
location ~ \.(gif|jpg|png)$ {
    root /data/images;
}
```
这是一个正则匹配，正则匹配需要以`~`开头。这个配置匹配文件拓展名为`gif`,`jpg`,`png`的文件，才会映射到`/data/images/`目录下。

最终配置如下:

```
server {
    location / {
        proxy_pass http://localhost:8080/;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

### 搭建一个FastCGI代理
nginx可以用来将一些请求转发给FastCGI服务，这些服务器上运行了使用PHP等语言不同框架构建的应用。

最常用的FastCGI代理配置是使用`fastcgi_pass`指令代替`proxy_pass`指令,可以使用`fastcgi_param`指令将一些参数传向FastCGI服务。

下面的配置将一些非图片请求转发向9000端口的FastCGI服务。

在PHP中，使用`SCRIPT_FILENAME`参数指代脚本名，使用`QUERY_STRING`参数传递请求参数。

```
server {
    location / {
        fastcgi_pass  localhost:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param QUERY_STRING    $query_string;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```




## 学习资料
- 淘宝提供的nginx开发教程：[Nginx开发从入门到精通](http://tengine.taobao.org/book/)
- nginx入门教程：[Beginner’s Guide](http://nginx.org/en/docs/beginners_guide.html)
