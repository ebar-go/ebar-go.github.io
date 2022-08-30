# 说明
介绍http请求组件的使用方式。

## GET请求
```go
package main

import "github.com/ebar-go/ego/component"

func main() {
    var address = "http://localhost:8080/api"

    curl := component.Provider().Curl()
    response, err := curl.Get(address)
    if err != nil {
        panic(err)
    }
    fmt.Println(response.Bytes()) // 也可以是byte
    var obj struct{}
    err = response.Decode(&obj) // 也可以解析到一个结构体
    response.Release() // 释放bytes数组
}
```

## POST请求
```go
params := make(url.Values)
params.Set("id", "1")
response, err := curl.Post(address, strings.NewReader(params.Encode()))
```

## PUT请求
```go
response, err := curl.Put(address, nil)
```

## PUT请求
```go
response, err := curl.Put(address, nil)
```


## Patch请求
```go
response, err := curl.Patch(address, nil)
```

## Delete请求
```go
response, err := curl.Delete(address, nil)
```

## 自定义请求
```go
request := http.NewRequest(http.MethodPost, url, body)
response, err := curl.Send(req)
if err != nil {
    return err
}
```
