---
title: 从零开始学习Docker(二)
date: 2018-01-30 15:33:58
tags: docker
categories: docker
keywords: docker
---

Docker 容器是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）。几乎没有性能开销,可以很容易地在机器和数据中心中运行。最重要的是,他们不依赖于任何语言、框架包括系统。

### 创建容器
创建一个新的容器并运行一个命令，可以使用 `docker run` 命令

在命令行中输出"Hello"。输出后容器降停止运行。
```
$ docker run ubuntu_v:dev /bin/echo "Hello"
Hello
```

以交互模式运行容器，为容器重新分配一个伪输入终端
```
$ docker run -it ubuntu_v:dev /bin/bash
root@fb1ca3a02755:/# echo "Hello"
Hello
```
-i: 以交互模式运行容器，通常与 -t 同时使用
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用

### 守护态后台运行容器
守护态后台运行容器,可以用 `-d` 命令
```
$ docker run -d ubuntu_v:dev /bin/bash -c "while true; do echo hello; sleep 1; done"
ba33cbfcdd91a059799b595a7a80f9b5c7f9217d3713da3ed79e291c14895f42
```

查看容器，可以使用 `docker ps` 命令
```
$ docker ps
CONTAINER ID        IMAGE                                                     COMMAND                  CREATED                  STATUS              PORTS                    NAMES
ba33cbfcdd91        ubuntu_v:dev                                              "/bin/bash -c 'while…"   Less than a second ago   Up 2 minutes                                 jovial_blackwell
2571bb3e2348        registry.cn-hangzhou.aliyuncs.com/moojnn/mysql:v7.3.1.3   "docker-entrypoint.s…"   7 days ago               Up 7 days           0.0.0.0:3306->3306/tcp   localtest
```
`docker ps` 是查看当前正在运行的容器。 `docker ps -a`是查看本地所有容器。

查看数据记录，可以使用 `docker logs` 命令
```
$ docker logs ba33cbfcdd91
```
一般这样使用 docker logs -f --tail=10 {container_name或container_id}

另外：
可以使用 `docker stop` 来终止一个运行中的容器。
可以使用 `docker start` 来启动容器。
可以使用 `docker restart` 来启动重新容器（将一个运行态的容器终止，然后再重新启动它）。

### 进入容器
在使用 `-d` 时，容器启动后会进入后台运行。如果需要进入容器进行操作，可以使用 `docker exec` 进入容器
```
i$ docker ps
CONTAINER ID        IMAGE                                                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
ba33cbfcdd91        ubuntu_v:dev                                              "/bin/bash -c 'while…"   7 minutes ago       Up 11 minutes                                jovial_blackwell
2571bb3e2348        registry.cn-hangzhou.aliyuncs.com/moojnn/mysql:v7.3.1.3   "docker-entrypoint.s…"   7 days ago          Up 7 days           0.0.0.0:3306->3306/tcp   localtest
$ docker exec -it ba33cbfcdd91 /bin/bash
root@ba33cbfcdd91:/# 
```

### 删除容器
删除容器，可以使用 `docker rm` 命令

```
$sudo docker rm  ba33cbfcdd91
```