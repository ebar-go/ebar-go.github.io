## 服务
支持以下类型的服务：
- http
- grpc
- websocket

## 接口
也可以自定义服务，只需实现以下接口：
```go
type Server interface {
	Serve(stop <-chan struct{})
}
```

