# Go 项目目录

* 引用来自[project-layout/README_zh.md at master · golang-standards/project-layout (github.com)](https://github.com/golang-standards/project-layout/blob/master/README_zh.md)

## Go 目录

### `/cmd`

* 本项目的主干。

* 每个应用程序的目录名应该与你想要的可执行文件的名称相匹配(例如，`/cmd/myapp`)。

* 不要在这个目录中放置太多代码。
  * 如果你认为代码可以导入并在其他项目中使用，那么它应该位于 `/pkg` 目录中。
  * 如果代码不是可重用的，或者你不希望其他人重用它，请将该代码放到 `/internal` 目录中。你会惊讶于别人会怎么做，所以要明确你的意图!

通常有一个小的 `main` 函数，从 `/internal` 和 `/pkg` 目录导入和调用代码，除此之外没有别的东西。

有关示例，请参阅 [`/cmd`](https://github.com/golang-standards/project-layout/blob/master/cmd/README.md) 目录。

### `/internal`

* 用于存放项目内部的私有库，只能在项目内部引用，不会被其他项目引用。

### `/pkg`

* 用于存放项目内部的私有库，只能在项目内部引用，不会被其他项目引用。
  * 其他项目会导入这些库，希望它们能正常工作，所以在这里放东西之前要三思

## 服务应用程序目录

### `/api`

* 该目录用于存放与 HTTP 接口相关的代码，例如处理 HTTP 请求的 handler 和定义路由的 router。

  * 创建`handler` 与`router`，通过这种方式，可以将请求和处理逻辑进行有效地分离，使代码更加结构化和易于维护。

  * ![image-20230731103819788](C:\Users\hello\AppData\Roaming\Typora\typora-user-images\image-20230731103819788.png)

    * `api/handler/` 目录： 这个目录用于存放**HTTP**请求处理的代码，通常包含与具体请求和业务逻辑相关的处理函数。在这个目录中，有一个名为 `handler.go` 的文件，它包含了处理HTTP请求的处理器函数的实现。
    * `api/router/` 目录： 这个目录用于存放**路由**相关的代码，主要包含定义和配置HTTP路由的功能。在这个目录中，有一个名为 `router.go` 的文件，它包含了路由器的实现，可以将不同的HTTP请求映射到相应的处理函数上。

    * 综合来看，`api/handler/` 目录和`api/router/` 目录是配合使用的。
      * 在 `router.go` 中配置了各种路由规则，将不同的HTTP请求映射到相应的处理函数上。
      * 而 `handler.go` 中实现了这些处理函数，负责具体的业务逻辑处理。

    


## Web 应用程序目录

### `/web`

* 用于存放静态资源文件，如 HTML、CSS、JavaScript 等。

## 通用应用目录

### `/configs`

* 配置文件模板或默认配置。

### `/init`

* 该目录用于存放系统初始化（systemd、upstart、sysv）和进程管理/supervisor（runit、supervisor）配置。

### `/scripts`

* 这里存放执行各种构建、安装、分析等操作的脚本，用于简化项目的构建和维护过程。

### `/build`

* 在这个目录中放置打包和持续集成相关的配置和脚本。例如，用于构建云(AMI)、容器(Docker)、操作系统(deb、rpm、pkg)包的配置和脚本。

### `/deployments`

* 在这个目录中放置IaaS、PaaS、系统和容器编排部署配置和模板，例如docker-compose、kubernetes/helm、mesos、terraform、bosh等。(?)

### `/test`

* 在这个目录中放置额外的外部测试应用程序和测试数据，用于测试代码功能。

## 其他目录

### `/docs`

* 在这个目录中放置额外的外部测试应用程序和测试数据，用于测试代码功能。

### `/tools`

* 这个项目的支持工具。注意，这些工具可以从 `/pkg` 和 `/internal` 目录导入代码。

### `/examples`

* 在这个目录中放置额外的外部测试应用程序和测试数据，用于测试代码功能。

### `/third_party`

* 外部辅助工具，分叉代码和其他第三方工具(例如 Swagger UI)。

### `/githooks`

* Git hooks。用于在特定事件发生时触发自定义脚本

### `/assets`

* 与存储库一起使用的其他资产(图像、徽标等)。

### `/website`

* 如果你不使用 Github 页面，则在这里放置项目的网站数据。

## 你不应该拥有的目录

### `/src`

* 有些 Go 项目确实有一个 `src` 文件夹，但这通常发生在开发人员有 Java 背景，在那里它是一种常见的模式。如果可以的话，尽量不要采用这种 Java 模式。你真的不希望你的 Go 代码或 Go 项目看起来像 Java:-)

# Tictok 项目结构

- [constant](constant) 表示常量
  - [biz](constant/biz) - 业务逻辑相关常量（Business logic）
  - [config](constant/config)
    - [env.go](constant/config/env.go) - 环境变量配置  这个文件可能包含设置和获取环境变量的代码
    - [service.go](constant/config/service.go) - 服务名称和端口  这个文件可能包含定义服务名称和对应端口的常量或配置信息。
- [idl](idl)  (Interface Definition Language)的缩写，即接口定义语言。这个目录包含了各个服务的RPC接口定义文件。
  - [auth.proto](idl/auth.proto) - 认证服务 RPC 定义
  - [comment.proto](idl/comment.proto) - 评论服务 RPC 定义
  - [favorite.proto](idl/favorite.proto) - 点赞服务 RPC 定义
  - [feed.proto](idl/feed.proto) - 视频流服务 RPC 定义
  - [publish.proto](idl/publish.proto) - 视频发布服务 RPC 定义
  - [relation.proto](idl/relation.proto) - 关注服务 RPC 定义
  - [user.proto](idl/user.proto) - 用户服务 RPC 定义
  - [wechat.proto](idl/wechat.proto) - 聊天服务 RPC 定义
- [kitex_gen](kitex_gen) - 由 Kitex 自动生成的代码，这个目录可能包含Kitex框架生成的一些代码文件。
- [logging](logging) - 日志相关的配置文件配置
- [manifests-dev](manifests-dev) - Kubernetes 清单文件
- [repo](repo) - 数据库概要和由 Gorm Gen 自动生成的代码
- [service](service)
  - [auth](service/auth) - 鉴权服务实现
  - [comment](service/comment) - 评论服务实现
  - [favorite](service/favorite) - 点赞服务实现
  - [feed](service/feed) - 视频流服务实现
  - [publish](service/publish) - 视频发布服务实现
  - [relation](service/relation) - 关注服务实现
  - [user](service/user) - 用户服务实现
  - [web](service/web) - Web API 网关
    - [mw](service/web/mw) - Hertz 中间件
  - [wechat](service/wechat) - 聊天服务实现
- [storage](storage) - 对象存储中间件，支持 Amazon S3 和本地存储和火山引擎 [ImageX](https://www.volcengine.com/products/imagex)
- [test](test)
  - [e2e](test/e2e) - 端到端测试
  - [mock](test/mock) - 用于单元测试的 mock 数据

