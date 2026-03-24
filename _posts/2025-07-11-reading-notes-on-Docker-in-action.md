---
title:      《Docker实战（第2版）》读书笔记
description:   《Docker实战（第2版）》（Docker in Action(2rd Edition)）读书笔记
date:       2025-07-11 00:00:00+0800
author:     kode4fun
categories: [tech,docker]
tags:
    - 读书笔记
    - Docker
---

## 获取 Docker 帮助

```bash
docker help <command>
docker command --help
```
## docker container

| options | appliction                                                                    |
| :-----: | :---------------------------------------------------------------------------- |
| attach  | Attach local standard input, output, and error streams to a running container |
| commit  | Create a new image from a container's changes                                 |
|   cp    | Copy files/folders between a container and the local filesystem               |
| create  | Create a new container                                                        |
|  diff   | Inspect changes to files or directories on a container's filesystem           |
|  exec   | Execute a command in a running container                                      |
| export  | Export a container's filesystem as a tar archive                              |
| inspect | Display detailed information on one or more containers                        |
|  kill   | Kill one or more running containers                                           |
|  logs   | Fetch the logs of a container                                                 |
|   ls    | List containers                                                               |
|  pause  | Pause all processes within one or more containers                             |
|  port   | List port mappings or a specific mapping for the container                    |
|  prune  | Remove all stopped containers                                                 |
| rename  | Rename a container                                                            |
| restart | Restart one or more containers                                                |
|   rm    | Remove one or more containers                                                 |
|   run   | Create and run a new container from an image                                  |
|  start  | Start one or more stopped containers                                          |
|  stats  | Display a live stream of container(s) resource usage statistics               |
|  stop   | Stop one or more running containers                                           |
|   top   | Display the running processes of a container                                  |
| unpause | Unpause all processes within one or more containers                           |
| update  | Update configuration of one or more containers                                |
|  wait   | Block until one or more containers stop, then print their exit codes          |

### docker create

|        options        | appliction                       | memo or demo                           |
| :-------------------: | :------------------------------- | :------------------------------------- |
|       `--name`        | container 名称                   |                                        |
|   `--detach`  `-d`    | 后台启动 container               |                                        |
| `--interactive`  `-i` | 交互式启动 container             | 按 `Ctrl+P` + `Ctrl+Q` 分离            |
|        `--tty`        | 创建虚拟终端                     | 可将 `--interactive -- tty` 写作 `-it` |
|        `--rm`         | 在容器停止后自动删除             |                                        |
|     `--read-only`     | 只读挂载外部文件系统或卷         |                                        |
|       `--tmpfs`       | 为容器提供常驻内存的临时文件系统 | `--tmpfs  /tmp \ `                     |
|     `--env`  `-e`     | 注入环境变量                     | `-e web_root=/path/to/root`            |
|      `--restart`      | 容器的重启策略                   | `--restart always`                     |
|        `--pid`        | 容器内的进程 pid 与宿主的关系    | `--pid host` 可使容器与宿主共享进程    |
|       `--link`        |                                  |                                        |
|     `--entrypoin`     | 入口点（P46）                    |                                        |
|      `--cidfile`      |                                  | `--cidfile /path/to/cid/file`          |

### docker run
与 `docker create` 参数相同

### docker logs

|     options     | appliction                 | memo or demo |
| :-------------: | :------------------------- | :----------- |
| `--follow` `-f` | 显示日志并同时监视日志变化 |              |
|  `--tail` `-n`  | 显示日志的条目数           |              |


### docker exec

|        options        | appliction           | memo or demo                           |
| :-------------------: | :------------------- | :------------------------------------- |
|   `--detach`  `-d`    | 后台启动 container   |                                        |
|     `--env`  `-e`     | 注入环境变量         | `-e web_root=/path/to/root`            |
| `--interactive`  `-i` | 交互式启动 container | 按 `Ctrl+P` + `Ctrl+Q` 分离            |
|        `--tty`        | 创建虚拟终端         | 可将 `--interactive -- tty` 写作 `-it` |
|     `--user` `-u`     | 运行程序的用户名     | 格式为`<name\|uid>[:<group\|gid>]`     |
|   `--workdir` `-w`    | 工作目录             |                                        |

### docker ps
`docker ps` 可列出容器，默认只列出运行中的容器。

|      options       | appliction                 |
| :----------------: | :------------------------- |
|   `--all`  `-a`    | 显示所有容器（包括已停止） |
| `--no-trunc`  `-e` | 显示完整 container id      |
|   `--size`  `-s`   | 显示容器大小               |

### docker start

|        options        | appliction           |
| :-------------------: | :------------------- |
|   `--attach`  `-a`    | 附着                 |
| `--interactive`  `-i` | 交互式启动 container |

### docker restart

 |      options      | appliction |
 | :---------------: | :--------- |
 | `--signal`  `-s`  | 发送的信号 |
 | `--timeout`  `-t` | 超时时间   |

### docker stop
参数与 `docker restart` 相同

### docker rename
`docker rename <old_container_name> <new_container_name>`

### docker kill
`docker kill <CONTAINER>` 强制删除容器
  
### docker rm


|     options      | appliction |
| :--------------: | :--------- |
| `--force`  `-f`  | 强制删除   |
| `--volume`  `-v` | 删除卷     |

## docker image

| options | appliction                                                               |
| :-----: | :----------------------------------------------------------------------- |
|  build  | Build an image from a Dockerfile                                         |
| history | Show the history of an image                                             |
| import  | Import the contents from a tarball to create a filesystem image          |
| inspect | Display detailed information on one or more images                       |
|  load   | Load an image from a tar archive or STDIN                                |
|   ls    | List images                                                              |
|  prune  | Remove unused images                                                     |
|  pull   | Download an image from a registry                                        |
|  push   | Upload an image to a registry                                            |
|   rm    | Remove one or more images                                                |
|  save   | Save one or more images to a tar archive (streamed to STDOUT by default) |
|   tag   | Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE                    |

### docker pull
```bash
docker pull quay.io/dockerinaction/ch3_hello_registry:latest
```

###  docker rmi

`docker rmi <image_name>` 删除镜像

## docker volume

| options | appliction                                          |
| :-----: | :-------------------------------------------------- |
| create  | Create a volume                                     |
| inspect | Display detailed information on one or more volumes |
|   ls    | List volumes                                        |
|  prune  | Remove unused local volumes                         |
|   rm    | Remove one or more volumes                          |


## docker network

|  options   | appliction                                           |
| :--------: | :--------------------------------------------------- |
|  connect   | Connect a container to a network                     |
|   create   | Create a network                                     |
| disconnect | Disconnect a container from a network                |
|  inspect   | Display detailed information on one or more networks |
|     ls     | List networks                                        |
|   prune    | Remove all unused networks                           |
|     rm     | Remove one or more networks                          |

### 探索桥接网络

```bash
# 建立桥接网络
docker network create \
  --driver bridge \
  --label projection=dockerinaction \
  --label chapter=5 \
  --attachable \
  --scope local \
  --subnet 10.0.42.0/24 \
  --ip-range 10.0.42.128/25 \
  user-network

# 运行加入该网络的容器
docker run --it --name tom \
  --network user-network \
  --alpine:latest sh

# 容器内运行，查看网络地址
ip -f inet -4 -o addr
```

按 `Ctrl+P` 和 `Ctrl+Q` 分离终端

```bash
# 建立第二个桥接网络
docker network create \
  --driver bridge \
  --label projection=dockerinaction \
  --label chapter=5 \
  --attachable \
  --scope local \
  --subnet 10.0.43.0/24 \
  --ip-range 10.0.43.128/25 \
  user-network2

# 将容器连接到网络
docker network connect user-network2 tom

# 将终端再次挂接到容器
docker attach tom
```

使用 `nmap` 查看网络
```bash
apk update && apk add nmap
nmap -sn 10.0.42.* -sn 10.0.43.* -oG /dev/stdout | grep Status
```

### host 网络
```bash
docker run --rm --network host alpine:latest ip -o addr
```
### none 网络
```bash
docker run --rm --network none alpine:latest ip -o addr
```

## 存储和卷
### 绑定挂载

```bash
--mount type=bind,src=$(SRC),dst=${dst},readonly=true
```
### 常驻内存

```bash
--mount type=tmpfs,dst=${dst},tmpfs-size=16k,tmpfs-mode=1770
```
### 卷
```bash
docker volume create --driver local --label example=location location-example
docker volume inspect --format "{{ json.Mountpoint }}" location-example
```
## Dockfile
### 从 container 建立 image
```bash
# 列出 container 的修改情况
docker container diff <CONTAINER>

# 提交镜像
docker container commit -a '@author' \
  -m 'memo info' \
  --entry-point <APP> \
  --env ENV1=VAL1 \
  <CONTAINER> <IMAGE>

# 为镜像重新打标签
docker image tag <OLD> <NEW>

# 列出 image 的历史
docker image history <IMAGE>
```
### dockerfile 语法

```dockerfile
FROM alpine:latest
LABEL maintainer="admin@google.com"
ENV APPROOT="~" VERSION="0.1"
LABEL base.name="dockerfile demo" base.version="${VERSION}"
WORKDIR $APPROOT
RUN apk update && apk add git
ENTRYPOINT ["git"]
EXPOSE 4321
```

```bash
docker image build -t kode4fun/alpine-git:0.1 -f demo.df .
```

### 文件系统指令

|             command             |       memo        |
| :-----------------------------: | :---------------: |
| `COPY [FILE1,FILE2,...,TARGET]` |                   |
|      `VOLUME [VOL1,VOL2]`       |                   |
|        `CMD [ARG1,ARG2]`        | ENTRYPOINT 的参数 |

### dockerignore 文件
