## 说明
本文将告诉你如果快速的使用`ego`运行起一个包含http和grpc的混合应用。

## 主程序
- main.go
```go
import (
  "github.com/ebar-go/ego"
  "github.com/gin-gonic/gin"
  "net/http"
)
func main() {
   aggregator := ego.NewAggregatorServer()
	// 实例化一个http服务
	httpServer := ego.NewHTTPServer(":8080").
		RegisterRouteLoader(func(router *gin.Engine) { // 注册路由
			router.GET("/", func(ctx *gin.Context) {
				ctx.String(http.StatusOK, "home")
			})
		})
    // 实例化一个grpc服务
    grpcServer := ego.NewGRPCServer(":8081")
    // 支持自定义服务，只需要实现Server接口
    customServer := NewCustomServer()
	aggregator.WithServer(httpServer, grpcServer, customServer)
	aggregator.Run()
}
type CustomServer struct {
}
func (c CustomServer) Serve(stop <-chan struct{}) {
	// do something..
}
func NewCustomServer() *CustomServer {
	return &CustomServer{}
}
```

## 运行
```
go run main.go
```