---
title: 使用最新版本的jenkins(1)
layout: post
date: 2026-07-15 15:16:00
categories: Jenkins
tags:
    - Jenkins
    - Docker
    - CI/CD
---

# Jenkins环境部署工作汇报

> 原始分享链接: https://share.note.youdao.com/s/o3QXhHG  
> 原始参考链接: https://blog.csdn.net/qq_46487100/article/details/126289377

## 一、Docker方式部署Jenkins

操作步骤如下：

1. 拉取Jenkins镜像

```bash
docker pull jenkins/jenkins:jdk11
```

2. 创建数据目录

```bash
mkdir /data/jenkins
```

3. 启动Jenkins容器

   - 基础配置

```bash
docker run -u root -it --name jenkins -p 8080:8080 -p 50000:50000 \
-v /data/jenkins:/var/jenkins_home \
-v /data/maven/apache-maven-3.3.9:/usr/local/maven \
-v /data/java/jdk-11.0.19:/usr/local/java \
jenkins/jenkins:jdk11
```

   - 增加maven仓库映射

```bash
docker run -u root -it --name jenkins -p 8080:8080 -p 50000:50000 \
-v /data/jenkins:/var/jenkins_home \
-v /data/maven/apache-maven-3.3.9:/usr/local/maven \
-v /data/maven/repository:/data/maven/repository \
-v /data/java/jdk-11.0.19:/usr/local/java \
jenkins/jenkins:jdk11
```

## 二、非Docker方式部署建议

如需采用非Docker方式部署，可尝试以下脚本：

- [preclear.sh](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCEa5ed4c2183fe481097ed0c392e796805/10547)

## 三、启动脚本说明

- [222.sh](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE8f0ff4dfa51a4bfab890bc54c56aa5da/10549)

## 四、Jenkins初始化信息

- 管理员账户：admin
- 初始密码：ec7b6ce9d3fd45dbbc0e98ccaa309bac
- 访问端口：8080
- 基于以上映射配置，进行全局工具配置

> 初始化密码只适合首次部署使用，完成初始化后应尽快修改管理员密码。

### 全局工具配置

![全局工具配置-1](/images/jenkins/global-tool-config-1.png)

![全局工具配置-2](/images/jenkins/global-tool-config-2.png)

![全局工具配置-3](/images/jenkins/global-tool-config-3.png)

## 五、插件安装与配置

- 新增Maven插件
- 安装Publish Over SSH插件

![插件安装](/images/jenkins/plugin-install.png)

- 插件安装完成后，配置Over SSH

![Over SSH配置](/images/jenkins/over-ssh-config.png)

## 六、测试与注意事项

- 测试成功，注意Docker内网访问需配置防火墙
- Jenkins的SSH账户需使用root，否则无权限

![测试结果](/images/jenkins/test-result.png)

## 七、Jenkins容器地址说明

- 当前Jenkins的Docker地址为：172.17.0.3

![Jenkins容器地址-1](/images/jenkins/container-address-1.png)

![Jenkins容器地址-2](/images/jenkins/container-address-2.png)

![Jenkins容器地址-3](/images/jenkins/container-address-3.png)

![Jenkins容器地址-4](/images/jenkins/container-address-4.png)

![Jenkins容器地址-5](/images/jenkins/container-address-5.png)

## 八、后续工作与参考资料

- 部署工作尚未全部完成，后续将继续推进
- 参考链接：https://blog.csdn.net/qq_46487100/article/details/126289377
