# Go工程实践

## Gin 框架

* 如果是第一次搭建，要设置一下代理（国内访问不稳定）
  * `go env -w GOPROXY=https://goproxy.cn,direct`全局效应
* 下载gin框架
  * `get -v github.com/gin-gonic/gin@v1.4`



# uer模块

## 用户注册

* "net/http" ，"io/ioutil"，这两个包什么用？

  * "net/http"包：
    - 该包提供了创建HTTP客户端和服务器的功能，以及处理HTTP请求和响应的方法。
  * "io/ioutil"包：
    - 该包提供了一些方便的工具函数，用于读取和写入文件内容

* `"/douyin/user/register/?username=&password="`

  * 在这个示例中，URL 的路径是` "/douyin/user/register/"`，表示注册用户的路径。
  * 而查询参数则是以问号（?）开始，以键值对的形式出现在问号后面。
    * "username="：表示用户名的键，后面没有提供具体的值，即为空。
    * "password="：表示密码的键，后面没有提供具体的值，即为空。

* `req, err := http.NewRequest(method, url, nil)`

  * `http.NewRequest(method, url, nil)` 这行代码创建了一个新的 HTTP 请求，将返回的请求实例赋值给 `req` 变量，并将可能发生的错误赋值给 `err` 变量。

    * `method` 是请求的方法，例如 "GET"、"POST" 等。

    * `url` 是请求的URL。

    * `body` 是请求的主体内容，可以是一个 `io.Reader` 接口类型的对象。在这个例子中，传递了 `nil`，表示请求没有主体内容。

      ```go
      import (
          "net/http"
          "strings"
      )
      
      func main() {
          method := "POST"
          url := "http://example.com"
          body := "Hello, World!"req, err := http.NewRequest(method, url, strings.NewReader(body))
      if err != nil {
          // 处理错误
      }
      
      // 继续设置请求的其他属性、发送请求等操作
      ```
      }

* `return` 语句用于终止当前函数的执行

  * 在 `return` 语句之后不应该跟任何东西，因为它表示函数的结束。

* `defer res.Body.Close()`

  * 是一个在Go编程语言中常见的用法，用于确保在函数执行完成后关闭一个资源（通常是一个打开的文件、网络连接或HTTP响应体）。

    * `defer` 是一个关键字，这意味着无论代码中的何处，只要使用了 `defer` 关键字，这个函数调用都会被推迟执行，直到包含它的函数返回为止。
    * `defer` 的主要用途是在函数结束时进行一些清理操作，比如关闭文件、释放资源等。它可以帮助资源的正确释放，从而避免资源泄漏问题。

  * 在Go语言中，当你发送一个HTTP请求并且得到服务器的响应时，响应的内容（数据）被包装在一个叫做`res.Body`的对象中。

    （假设你从服务器获取了一个HTML页面作为响应，那么这个HTML页面的内容就储存在了`res.Body`中。）

  * 现在，当你读取完响应内容后，为了释放资源，你需要关闭这个`res.Body`。这就是为什么代码中使用`defer res.Body.Close()`的原因，它会确保在函数结束之前关闭这个响应体，以便释放资源。

* `req.Header.Add("User-Agent", "Apifox/1.0.0 (https://apifox.com)")`

  * 这句代码的作用是设置HTTP请求头部中的 "User-Agent" 字段，以便在发送HTTP请求时告诉服务器请求的发起者是哪个应用程序，以及应用程序的版本和附加信息。
  * `req`: 这是一个代表HTTP请求的结构体（通常是`http.Request`类型的实例）。
  * `.Header`: 这是`req`结构体中的一个属性，它表示HTTP请求的头部信息。
  * `.Add("User-Agent", "Apifox/1.0.0 (https://apifox.com)")`: 这是调用`Header`对象的`Add`方法，用于向请求头中添加一个新的键值对。第一个参数是键（"User-Agent"），第二个参数是对应的值。


