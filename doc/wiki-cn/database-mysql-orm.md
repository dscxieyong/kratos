# 准备工作

推荐使用[kratos工具](kratos-tool.md)快速生成项目，如我们生成一个叫`kratos-demo`的项目。目录结构如下：

```
├── CHANGELOG.md
├── CONTRIBUTORS.md
├── LICENSE
├── README.md
├── cmd
│   ├── cmd
│   └── main.go
├── configs
│   ├── application.toml
│   ├── grpc.toml
│   ├── http.toml
│   ├── log.toml
│   ├── memcache.toml
│   ├── mysql.toml
│   └── redis.toml
├── go.mod
├── go.sum
└── internal
    ├── dao
    │   └── dao.go
    ├── model
    │   └── model.go
    ├── server
    │   └── http
    │       └── http.go
    └── service
        └── service.go
```

# 开始使用

## 配置

创建项目成功后，进入项目中的configs目录，mysql.toml，我们可以看到：

```toml
[demo]
	addr = "127.0.0.1:3306"
	dsn = "{user}:{password}@tcp(127.0.0.1:3306)/{database}?timeout=1s&readTimeout=1s&writeTimeout=1s&parseTime=true&loc=Local&charset=utf8mb4,utf8"
	readDSN = ["{user}:{password}@tcp(127.0.0.2:3306)/{database}?timeout=1s&readTimeout=1s&writeTimeout=1s&parseTime=true&loc=Local&charset=utf8mb4,utf8","{user}:{password}@tcp(127.0.0.3:3306)/{database}?timeout=1s&readTimeout=1s&writeTimeout=1s&parseTime=true&loc=Local&charset=utf8,utf8mb4"]
	active = 20
	idle = 10
	idleTimeout ="4h"
	queryTimeout = "200ms"
	execTimeout = "300ms"
	tranTimeout = "400ms"
```

在该配置文件中我们可以配置mysql的读和写的dsn、连接地址addr、连接池的闲置连接数idle、最大连接数active以及各类超时。

如果配置了readDSN，在进行读操作的时候会优先使用readDSN的连接。

## 初始化

进入项目的internal/dao目录，打开dao.go，其中：

```go
var (
    dc struct {
        Demo *sql.Config
    }
)
checkErr(paladin.Get("mysql.toml").UnmarshalTOML(&dc))
```
使用paladin配置管理工具将上文中的mysql.toml中的配置解析为我们需要使用mysql的相关配置。

# TODO：补充常用方法

# 扩展阅读

[tidb模块说明](database-tidb.md)
[hbase模块说明](database-hbase.md)

-------------

[文档目录树](summary.md)
