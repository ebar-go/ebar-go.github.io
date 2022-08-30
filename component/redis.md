# 说明
redis组件是基于`https://github.com/go-redis/redis`实现的。


## 初始化
一般连接redis是在main.go里的init函数，连接redis放在加载配置项成功后。

- Demo
```go
package main
import "github.com/ebar-go/component"
func main() {
    // connect redis
	if err := component.Provider().Redis().Connect(&redis.Options{Addr: "127.0.0.1:6379"}); err != nil {
		panic(err)
	}
	// set some key
	component.Provider().Redis().Set("someKey", "someValue", time.Second)
}
```
