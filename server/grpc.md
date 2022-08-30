## 说明
本文将告诉你如果快速的使用`ego`运行起一个grpc应用。

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

	grpcServer := ego.NewGRPCServer(":8081")

	aggregator.WithServer(grpcServer)

	aggregator.Run()
}
```

## 运行
```
go run main.go
```
