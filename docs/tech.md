# 技术选型与部署

## 技术选型

### 前端

前端总体基于 React 框架进行编写，测试框架采用 Jest，为 UI 编写方便使用了 antd 组件库。

为统一不同成员间的编程风格，我们配置了面向编辑器的 `.editorconfig`，安装了 ESLint 与 Prettier 进行自动代码风格格式化。我们还安装了 husky 这个 Git 钩子，使得每次 commit 之前都必须通过 ESLint 风格检测。

我们要求成员使用函数式组件进行编程，使用 react-router 与 react-redux 并将其深度融合，管理组件状态。从整体来看，我们的前端奉行分层解耦合的设计理念，分为 UI 层、逻辑层与网络层三部分。UI 层主要是面向用户的组件渲染呈现，逻辑层主要负责全局变量的管理与数据流的管理，网络层主要负责与后端进行通信。

### 后端

后端基于 Django 与 Django REST framework 进行编写。测试框架采用 pytest。生产环境中，我们使用uWSGI进行部署。

为统一不同成员间的编程风格，我们配置了面向编辑器的 `.editorconfig`，安装了 pylint 并对其进行了配置，安装了 black 这一自动代码风格格式化的工具。

REST framework 的存在大大简化了对数据库进行增删查改的操作。我们编写了鉴权用的 Middleware，只需作为函数装饰器调用便可方便地进行鉴权。

与 GitLab 的对接时，我们采用了定时任务和 Webhooks 两种对接方式。对于WebHook，我们实现了在 $O(1)$ 的时间内完成推送数据的增删查改，更新十分及时；对于定时任务，我们会定时请求整个仓库，使得远程仓库修改能够同步。

在定时任务中，我们也会维护commit/merge/issue与SR的关联；此时在commit/merge/issue发布后添加的SR将会与创建前的对应远端commit/merge/issue进行关联。

###　数据库

我们开发与部署环境数据库版本为 MySQL 5.7，开箱即用。
在开发过程中，我们对数据库进行周期性备份，维护生产环境的数据安全。

### 文档

本文档基于 MkDocs 进行部署，采用了 Material for MkDocs 主题。

## 部署

感谢软件工程课程平台 SECODER 提供了容器构建-部署的一键式解决方案，我们使用其提供的 deployer 工具配合 GitLab 的 CI/CD 实现自动读取 Dockerfile 之后构建，然后自动替换当前正在运行的容器这一流程。我们为 dev 分支和 master 分支部署了不同的网站，并将数据库隔离，以实现测试环境和生产环境的隔离与全仿真。
