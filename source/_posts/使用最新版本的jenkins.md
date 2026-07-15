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

![全局工具配置-1](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE095450b3ce7542a6821a82be389d1f93/10551)

![全局工具配置-2](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE44ef78cec80f40e793b1d1449ca25f1b/10552)

![全局工具配置-3](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE0f7d666e8bf64253afd36d20764e9463/10553)

## 五、插件安装与配置

- 新增Maven插件
- 安装Publish Over SSH插件

![插件安装](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE162f78202ffa460d9ba0566e2530ded3/10554)

- 插件安装完成后，配置Over SSH

![Over SSH配置](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE3dc4df08fd244a24b58f1e2ff045ff52/10555)

## 六、测试与注意事项

- 测试成功，注意Docker内网访问需配置防火墙
- Jenkins的SSH账户需使用root，否则无权限

![测试结果](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCEc6422fbbc0014737b810253db384c394/10556)

## 七、Jenkins容器地址说明

- 当前Jenkins的Docker地址为：172.17.0.3

![Jenkins容器地址-1](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCEf5c383c7f93f4e73ac74fd8cb366eb0d/10557)

![Jenkins容器地址-2](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE3d9a2824001b45b4ab3974c157ee7619/10558)

![Jenkins容器地址-3](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE0a944bc5594a4338b52580799914a1cf/10559)

![Jenkins容器地址-4](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE63eba3373d2140109a2ab0ece9707b53/10560)

![Jenkins容器地址-5](https://note.youdao.com/yws/public/resource/cc09381137439bfec17aab8ac9385f73/xmlnote/WEBRESOURCE1f0b352c534747bab91431529e8dae56/10561)

## 八、后续工作与参考资料

- 部署工作尚未全部完成，后续将继续推进
- 参考链接：https://blog.csdn.net/qq_46487100/article/details/126289377
