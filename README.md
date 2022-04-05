# bluebell

Go Web 进阶课程 bluebell 项目，提供如下功能：

- 利用 Gin 框架 + Redis + MySQL 搭建一套 Go Web 开发通用脚手架
- 模仿 Stack Overflow 构建一个帖子投票网站，用户可以对帖子进行投票并且可以创建帖子
- 用户可以浏览到根据分数或者时间排序的帖子列表
- 利用令牌桶对某些接口进行限流

bluebell 项目具体代码详解可见「[Go Web 进阶课程](https://study.163.com/course/introduction/1210171207.htm)」，建议使用之前阅读。

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

## 常规方式运行

如果不使用 Docker 运行，配置好 Python、Redis、MySQL 环境之后也可运行，步骤如下。

### MySQL 和 Redis 安装配置
本地安装 MySQL、Docker 启动 MySQL、远程 MySQL 都是可以的，只要能正常连接使用即可，这里建议用 Docker 起一个 MySQL 。

本地安装 Redis、Docker 启动 Redis、远程 Redis 都是可以的，只要能正常连接使用即可，这里建议用 Docker 起一个 Redis 。

修改配置文件，确保项目连接的 MySQL 和 Redis 是用 Docker 启动的 MySQL 和 Redis，同时要在 MySQL 新建 bluebell 库。

### 安装依赖包
安装相关依赖包需要 Go 开发环境，如果还未安装，可以去看我的[博客](https://www.yuque.com/cuicuiaixiedaima/bhgmys/udybgl)。
然后下载相关依赖即可：

```shell script
go mod tidy
```

### 运行服务
第一种方式运行服务：

```shell script
go run main.go
```

第二种方式运行服务：

```shell script
go build
bluebell
```
如果 server 启动成功，这时候访问 [http://localhost:9090/](http://localhost:9090/) 即可获取帖子列表信息。

## 待开发

- [ ] 开发一个爬虫接口，提供源源不断的帖子数据
- [ ] 使用情况统计分析
- [ ] 单元测试的覆盖率

## LICENSE

MIT