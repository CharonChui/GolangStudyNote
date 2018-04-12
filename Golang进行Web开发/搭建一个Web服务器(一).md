搭建一个Web服务器(一)
===

`Web`是基于`http`协议的一个服务，`Go`语言里面提供了一个完善的`net/http`包，通过`http`包可以很方便的就搭建起来一个可以运行的`Web`服务。同时使用这个包能很简单地对`Web`的路由，静态文件，模版，`cookie`等数据进行设置和操作。

[有关网络请求相关知识请查看之前的文章网络请求相关内容总结](https://github.com/CharonChui/AndroidNote/blob/master/JavaKnowledge/%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9%E6%80%BB%E7%BB%93.md)


下面就用`go`实现一个简单的`web`服务器:   
```go
import (
	"net/http"
	"log"
	"fmt"
)

func main() {
	http.HandleFunc("/", sayHello)
	err := http.ListenAndServe(":9999", nil)
	if err != nil {
		log.Fatalf("ListenAndServe: ", err)
	}
}
func sayHello(writer http.ResponseWriter, request *http.Request) {
	// 解析参数，默认是不会解析的
	request.ParseForm()

	// 下面是打印一些请求信息
	fmt.Println(request.Form)
	fmt.Println("path is :" + request.URL.Path)
	fmt.Println("scheme is :" + request.URL.Scheme)

	for k, v := range request.Form {
		fmt.Println("key : ", k)
		fmt.Println("val :", v)
	}
	// 向浏览器客户端输入信息
	fmt.Fprintf(writer, "Hello World")
}
```

这个时候其实已经在`9999`端口监听`http`链接请求了。

在浏览器输入`http://localhost:9999`
或者加参数:`http://localhost:9999/?username"haha"&sex="male"`

<img src="https://raw.githubusercontent.com/CharonChui/Pictures/master/go_web_hello_world.png?raw=true" width="100%"/>

在服务器端输出的信息如下:    
```go
map[username:["haha"] sex:["male"]]
path is :/
scheme is :
[]
key :  username
val : ["haha"]
key :  sex
val : ["male"]
map[]
path is :/favicon.ico
scheme is :
[]
```

惊喜不惊喜，意外不意外，它不用和`java`是的需要`tomcat`，直接使用`net/http`的包调用几个方法就可以了，因为它可以直接就监听`tcp`端口了。


---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 