# bluebell

Go Web 进阶课程 bluebell 项目，提供如下功能：

- 利用 gin 框架 + redis + mysql 搭建一套 Go web 开发通用脚手架
- 模仿 stack overflow 构建一个帖子投票网站，用户可以对帖子进行投票并且可以创建帖子
- 用户可以浏览到根据分数或者时间排序的帖子列表
- 利用令牌桶对某些接口进行限流

bluebell 项目具体代码详解可见 Go Web 进阶课程，建议使用之前阅读。

## 使用准备

首先当然是克隆代码并进入 bluebell 文件夹：

```
git clone https://github.com/StudentCWZ/bluebell.git
cd bluebell
```

## 使用要求

可以通过两种方式来运行代理池，一种方式是使用 Docker（推荐），另一种方式是常规方式运行，要求如下：

### Docker

如果使用 Docker，则需要安装如下环境：

- Docker
- Docker-Compose

安装方法自行搜索即可。

### 常规方式

常规方式要求有 Golang 环境、Redis 环境、MySQL 环境，具体要求如下：

- Golang >= 1.14
- Redis
- MySQL

## Docker 运行

如果安装好了 Docker 和 Docker-Compose，只需要一条命令即可运行。（注意：此时要把 config.yaml 文件中 mysql 和 redis 中的 host 改为容器名，端口改为 3306 和 6379）

```shell script
docker-compose up
```

运行结果类似如下：

```
go_bluebell     | [GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
go_bluebell     |  - using env: export GIN_MODE=release
go_bluebell     |  - using code:    gin.SetMode(gin.ReleaseMode)
go_bluebell     |
go_bluebell     | [GIN-debug] GET    /static/*filepath         --> github.com/gin-gonic/gin.(*RouterGroup).createStaticHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] HEAD   /static/*filepath         --> github.com/gin-gonic/gin.(*RouterGroup).createStaticHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /                         --> bluebell/router.Setup.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /ping                     --> bluebell/router.Setup.func2 (3 handlers)
go_bluebell     | [GIN-debug] GET    /swagger/*any             --> github.com/swaggo/gin-swagger.CustomWrapHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] POST   /api/v1/signup            --> bluebell/controller.SignUpHandler (3 handlers)
go_bluebell     | [GIN-debug] POST   /api/v1/login             --> bluebell/controller.LoginHandler (3 handlers)
go_bluebell     | [GIN-debug] GET    /api/v1/community         --> bluebell/controller.CommunityHandler (3 handlers)
go_bluebell     | [GIN-debug] GET    /api/v1/community/:id     --> bluebell/controller.CommunityDetailHandler (3 handlers)
go_bluebell     | [GIN-debug] GET    /api/v1/post/:id          --> bluebell/controller.GetPostDetailHandler (3 handlers)
go_bluebell     | [GIN-debug] GET    /api/v1/posts             --> bluebell/controller.GetPostListHandler (3 handlers)
go_bluebell     | [GIN-debug] GET    /api/v1/posts2            --> bluebell/controller.GetPostListTwoHandler (3 handlers)
go_bluebell     | [GIN-debug] POST   /api/v1/post              --> bluebell/controller.CreatePostHandler (5 handlers)
go_bluebell     | [GIN-debug] POST   /api/v1/vote              --> bluebell/controller.PostVoteHandler (5 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/             --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/cmdline      --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/profile      --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] POST   /debug/pprof/symbol       --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/symbol       --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/trace        --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/allocs       --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/block        --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/goroutine    --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/heap         --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/mutex        --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
go_bluebell     | [GIN-debug] GET    /debug/pprof/threadcreate --> github.com/gin-contrib/pprof.pprofHandler.func1 (3 handlers)
```

可以看到 Server 已经启动成功。

这时候访问 [http://localhost:9090/](http://localhost:9090/) 即可获取帖子列表信息。

