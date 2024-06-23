


# go-zero 资源

go-zero 教程 ：https://go-zero.dev/docs/concepts/overview

go-zero 周边 ： https://go-zero.dev/docs/reference/examples

go-zero-looklook  https://github.com/Mikaelemmmm/go-zero-looklook

# go-zero 安装 

window 安装 

**golang 安装**

**goctl 安装**  

1、下载：https://github.com/zeromicro/go-zero/releases/download/tools/goctl/v1.5.6/goctl-v1.5.6-windows-amd64.zip

2、解压后添加到 GOPATH， GOPATH查看路径

```sh 
$ go env GOPATH
```
**protoc 安装** 

通过 goctl 安装， 如下：

```sh 
(base) C:\kane-fang>goctl env check --install --verbose --force
[goctl-env]: preparing to check env

[goctl-env]: looking up "protoc"
[goctl-env]: "protoc" is installed

[goctl-env]: looking up "protoc-gen-go"
[goctl-env]: "protoc-gen-go" is installed

[goctl-env]: looking up "protoc-gen-go-grpc"
[goctl-env]: "protoc-gen-go-grpc" is installed

[goctl-env]: congratulations! your goctl environment is ready!
```
**go-zero  安装**

```sh 
$ go get -u github.com/zeromicro/go-zero@latest
```
**goctl-vscode 安装**

打开 Visual Studio Code | Extensions，搜索 goctl，点击 install 安装



# go-zero 配置 config

go-zero 支持三种格式的文件： yam, json, toml 

程序会自动的通过后缀来加对应配置文件格式。 

conf 目前已经默认自动支持 key 大小写不敏感。 

支持环境变量

支持 tag 校验规则，如：

1、optional  当前字段是可选参数，允许为零值(zero value)
2、options 当前参数仅可接收的枚举值
3、default 当前参数默认值
4、range 当前参数数值有效范围，仅对数值有效，写法规则详情见下文温馨提示
5、env 当前参数从环境变量获取


tag 的 inherit  功能会不会把配置文件搞复杂了？


# go-zero 常用命令

```
goctl api go -api hello.api -dir .
```

api 生成代码的命令：

```sh 
goctl api go -api user.api -dir . -style goZero 
goctl api go  -dir . -style goZero  -api user.api

goctl rpc protoc cag-wx-proxy.proto  --go_out=. --go-grpc_out=. --zrpc_out=. -style goZero
```






# go-zore 开发经验

## RPC 

  



rpc 的 Etcd 配置

zrpc.RpcServerConf 包含了 Redis 的配置， 如果再定义redis 的配置就会冲突. 

```sh
configFile:etc/cagwxproxy.yaml
2024/06/03 17:26:40 error: config file etc/cagwxproxy.yaml, conflict key redis, pay attention to anonymous fields
```

**grpcurl** 

grpc 测试工具 `grpcurl ` , 地址：https://github.com/fullstorydev/grpcurl

安装：

```
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
```

## http client 

https://go-zero.dev/docs/tutorials/http/client/index

分布式链路跟踪中的traceid和spanid代表什么？
https://cloud.tencent.com/developer/article/1832719

https://juejin.cn/post/7272581426331254839

## error 

打包失败，如下：

```
s# go.opentelemetry.io/otel/internal/global

[99](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L99)/go/pkg/mod/go.opentelemetry.io/otel@v1.19.0/internal/global/handler.go:44:18: undefined: atomic.Pointer

[100](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L100)/go/pkg/mod/go.opentelemetry.io/otel@v1.19.0/internal/global/internal_logging.go:30:25: undefined: atomic.Pointer

[101](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L101)note: module requires Go 1.20

[102](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L102)# google.golang.org/grpc

[103](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L103)/go/pkg/mod/google.golang.org/grpc@v1.60.0/server.go:2161:14: undefined: atomic.Int64

[104](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L104)note: module requires Go 1.19

[105](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L105)ok cag-proxy/internal/version 0.028s coverage: 71.4% of statements [no tests to run]

[107](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L107)Cleaning up project directory and file based variables00:00

[109](https://gitlab.vrviu.com/cag/cag-proxy/-/jobs/227002#L109)ERROR: Job failed: command terminated with exit code 1
```

docker 内的 golang 版本过低。 要替换高版本。 

在 `.gitlab-ci.yml` 中 把  `image: default.registry.tke.com/vrviu/gobuild:0.3` 换成 `hub-docker.yuntiancloud.com/ci/gobuild:1.21.3-0.1`

```sh 
root@6020-mg-01:/app/cag-proxy# ./cag-proxy 
./cag-proxy: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.32' not found (required by ./cag-proxy)
./cag-proxy: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.34' not found (required by ./cag-proxy)
```

go build 前加 

```
export CGO_ENABLED=0
```

```sh 
2024-01-03T14:56:10.564+08:00    error  [flow_id-1234567890] wx.GetWebAccessToken err : Get "https://api.weixin.qq.com/sns/oauth2/access_token?appid=wx44217ca4482c833d&code=0611Xb1w340iZ13cPb4w35iH8P01Xb1A&grant_type=authorization_code&secret=7c6526d8a80081dbe7d4bdcbda6eb420": tls: failed to verify certificate: x509: certificate signed by unknown authority    caller=logic/geWxUserInfoLogic.go:31
```