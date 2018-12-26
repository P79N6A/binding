### 声明
binding 模块参考自开源框架gin，能够对标准net/http req 进行model绑定，比较方便。

可以看到，结构体中，设置了binding标签的字段（Name和Age），如果没传会抛错误。非banding的字段（passwd），对于客户端没有传，User结构会用零值填充。对于User结构没有的参数，会自动被忽略。


### 示例
```
package main

import (
    "fmt"
    "net/http"

    "github.com/julienschmidt/httprouter"
    "git.xiaojukeji.com/nuwa/binding"
)

type InStruct struct {
    Name   string `json:"name" binding:"required"`
    Age    int64  `json:"age" binding:"required"`
    Passwd string `json:"passwd"`
}

func HelloHandler(w http.ResponseWriter, r *http.Request, _ httprouter.Params) {

    var inData InStruct
    bind := binding.Default(r.Method, r.Header.Get("Content-Type"))
    err := bind.Bind(r, &inData)
    if err != nil {
        fmt.Println(err)
    }
    fmt.Printf("indata %v\n", inData)
}

func main() {
    router := httprouter.New()
    router.POST("/", HelloHandler)
    http.ListenAndServe(":8080", router)
}
```

### 支持的Content-Type集合
```
    MIMEJSON              = "application/json"
    MIMEHTML              = "text/html"
    MIMEXML               = "application/xml"
    MIMEXML2              = "text/xml"
    MIMEPlain             = "text/plain"
    MIMEPOSTForm          = "application/x-www-form-urlencoded"
    MIMEMultipartPOSTForm = "multipart/form-data"
    MIMEPROTOBUF          = "application/x-protobuf"
    MIMEMSGPACK           = "application/x-msgpack"
    MIMEMSGPACK2          = "application/msgpack"
```
