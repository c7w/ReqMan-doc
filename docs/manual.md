# 用户手册

感谢您使用 ReqMan 需求管理系统！

本平台地址：[ReqMan | 需求管理跟踪助手 (secoder.net)](https://frontend-undefined.app.secoder.net/)

## 注册与登录

注册时请绑定邮箱进行注册，可以使用该邮箱找回密码；注册账号时可以使用项目邀请码加入已有项目。其中，用户名、邮箱不必与 Git 相同。

## 个人信息管理

可点击头像进入个人信息设置界面，完善个人信息。

### 个人邮箱

个人信息中包含两类邮箱：
+ 主邮箱
+ 其他邮箱

其中，主邮箱只有一个，默认为注册时绑定的邮箱，用于登录账号以及找回账号的密码；其他邮箱可以设置多个，用于匹配 Git 提交信息中的 commit 记录，需要与 Git 中的邮箱相同；其他邮箱可以替换当前的主邮箱成为新的主邮箱。

### 远程用户名

此处远程用户名不同于账号注册时的用户名：远程用户名用于匹配远程 Git 平台的 Issue 以及 MergeRequest 记录，注册时用户名用于本平台登录。

远程用户名需要与远程地址所用用户名相同，可以在个人信息页面进行主动更新，目前仅支持 gitlab.secoder.net。

## 项目管理

点击 Top Bar 中的 **我的项目** 可以进入项目管理界面。

### 新建项目

可以新建项目，建立者自动成为项目管理员，具有最高权限。

### 管理项目

点击对应项目条目后可以进入到项目中，点击左侧 Side Bar 中项目设置可以管理项目基本信息。

#### 项目邀请码

每个项目自动生成项目邀请码，用于邀请本平台其他用户加入本项目。

#### 远程仓库管理

可以在此处添加远程仓库，用于同步远程仓库内的 Git 记录，有两级 Token 配置：

+ Access Token，用于定时主动拉取仓库信息
+ Secret Token，用于被动收取仓库 Webhooks 推送信息，即时性较强。

在创建远程仓库选项卡内，仓库 Id 需要与远程仓库 Id 相同，Token 需填入 Access Token。

![image.png](https://s2.loli.net/2022/05/09/5dNheGXfVnogycB.png){.img-fluid tag=2}

##### Id 获取

以 GitLab 为例，在远程仓库的 项目概览 -> 详情 页面顶部查到对应仓库的 Id

![image.png](https://s2.loli.net/2022/05/09/R2ZJb1h4pqgoaP5.png){.img-fluid tag=2}

![image.png](https://s2.loli.net/2022/05/09/I26zetQcThHPGdq.png){.img-fluid tag=2}

##### Access Token 获取

以 GitLab 为例，在远程仓库的 项目设置 -> 访问令牌 页面配置 Access Token。

![image.png](https://s2.loli.net/2022/05/09/yHazjreP714hMAC.png){.img-fluid tag=2}

本平台所需 Access Token 权限包括：

+ read_api
+ read_repository
+ read_registry

![image.png](https://s2.loli.net/2022/05/09/3MdWhlAj1vx7Epu.png){.img-fluid tag=2}

##### Secret Token 配置

配置 Secret Token 需先配置 Access Token。

以 GitLab 为例，在远程仓库的 项目设置 -> Webhooks 页面配置 Secret Token。

其中：

+ URL 填入**https://backend-undefined.app.secoder.net/rdts/webhook/**
+ Secret Token 可在本平台配置完 Access Token 后复制，并填入 GitLab 对应位置

![image.png](https://s2.loli.net/2022/05/09/euk4Lrtg6WUXcRs.png){.img-fluid tag=2}

本平台所需 Trigger 包含：
+ Push events
+ Issues events
+ Merge request events

![image.png](https://s2.loli.net/2022/05/09/R5wKSHVigvFqo8j.png){.img-fluid tag=2}

### 加入项目

项目管理员可将项目邀请码分享给对应用户，对应用户可依据邀请码加入项目，成为项目成员。

项目管理员可在 项目-> 项目成员 页面管理已有项目成员，可以编辑对应成员身份权限，不同身份权限信息参考 [需求分析 - ReqMan Docs | 需求管理跟踪助手帮助文档 (secoder.net)](https://doc-undefined.app.secoder.net/analysis/#_4)

## 项目合并管理

本平台自动拉取远程仓库 Merge Request 来生成合并管理，可自动关联功能需求。

自动关联功能需求：

1. 功能需求名称为 **SR.xxx.xxx**，其中 'xxx' 部分为三位十进制数，例：'SR.006.818'
2. 远程仓库 Merge Request 合并请求信息标题开头为 '[SR.xxx.xxx]'，其中为需要匹配的功能需求的名称，例：'[SR.006.818] Merge Dev to Master'

## 项目缺陷追踪管理

项目中可以自动匹配远程仓库中代表缺陷的 Issue。缺陷 Issue 需在远程仓库 Issue 页面添加标签为 bug。

## 原始需求关联信息查看

在**原始需求管理**页面，可点击原始需求对应的**标题**查看原始需求的相关关联信息，点击**关联功能需求**可以添加管理的功能需求。

## 功能需求关联信息查看

在**功能需求管理**页面，可以点击功能需求对应的**标题**查看功能需求的相关关联信息，可点击**编辑**管理描述、状态、迭代、服务以及负责人。

自动关联远程仓库 commit 提交记录：

1. 功能需求名称为 **SR.xxx.xxx**，其中 'xxx' 部分为三位十进制数，例：'SR.006.818'
2. 远程仓库 commit 提交信息开头为 '[SR.xxx.xxx]'，其中为需要匹配的功能需求的名称，例：'[SR.006.818] Init the repo'

自动关联缺陷追踪：

1. 功能需求名称为 **SR.xxx.xxx**，其中 'xxx' 部分为三位十进制数，例：'SR.006.818'
2. 远程仓库存在 Issue，该 Issue 标题以 '[SR.xxx.xxx]' 开头，且带有 bug 标签