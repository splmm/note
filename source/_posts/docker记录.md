---
title: Docker记录
layout: post
date: 2022-06-20 04:58:06
categories: Linux
tags:
    - docker
---

# Docker 记录

## Docker 基础操作

### 进入容器环境
```bash
docker ps
docker exec -it <container> bash
```

**注意事项:**
- 进入容器后避免修改系统配置
- 对于正在运行的容器,建议先记录容器ID
- 修改配置可能导致容器更新,造成数据丢失

### 常用命令

#### 容器管理
1. 启动新容器
```bash
docker run -it --privileged=true -v /home/oracle/download:/usr/Downloads centos /bin/bash
```

2. 查看容器
```bash
docker ps -a  # 查看所有容器
docker ps     # 查看运行中容器
```

3. 提交容器
```bash
docker commit <CONTAINER ID> docker.io/ubuntu
```

4. 查看容器信息
```bash
# 查看root密码
docker logs <容器名orID> 2>&1 | grep '^User: ' | tail -n1

# 查看容器日志
docker logs -f <容器名orID>
```

#### 容器操作
```bash
# 停止容器
docker stop <容器名orID>

# 启动容器 
docker start <容器名orID>

# 删除容器
docker rm <容器名orID>
docker rm $(docker ps -a -q)  # 删除所有容器
```

#### 镜像管理
```bash
# 查看镜像
docker images

# 删除镜像
docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)

# 拉取镜像
docker pull <镜像名:tag>

# 保存/加载镜像
docker save <镜像名> > save.tar
docker load < save.tar
```

#### 高级操作
1. 运行带映射的容器
```bash
docker run --name redmine -p 9003:80 -p 9023:22 -d \
    -v /var/redmine/files:/redmine/files \
    -v /var/redmine/mysql:/var/lib/mysql \
    sameersbn/redmine
```

2. 容器连接
```bash
docker run -i -t --name sonar -d -link mmysql:db tpires/sonar-server
```

3. 构建镜像
```bash
docker build -t <镜像名> <Dockerfile路径>
```