Golang方法(十)
===


方法
---

在`Go`语言中有一个概念和函数极其相似，叫做方法。`Go`语言的方法其实是作用在接收者`（receiver）`上的一个函数，接收者是某种非内置类型的变量。因此方法是一种特殊类型的函数。

接收者类型可以是（几乎）任何类型，不仅仅是结构体类型:任何类型都可以有方法，甚至可以是函数类型，可以是`int`、`bool`、`string`或数组的别名类型。但是接收者不能是一个接口类型。

方法的声明和普通函数的声明类似，只是在函数名称前面多了一个参数，这个参数把这个方法绑定到这个参数对应的类型上。

和函数关系
---

方法是特殊的函数，定义在某一特定的类型上，通过类型的实例来进行调用，这个实例被叫接收者`(receiver)`。
函数将变量作为参数:`Function1(recv)`
方法在变量上被调用:`recv.Method1()`
接收者必须有一个显式的名字，这个名字必须在方法中被使用。
`receiver_type`叫做(接收者)基本类型，这个类型必须在和方法同样的包中被声明。

在`Go`中，(接收者)类型关联的方法不写在类型结构里面，就像类那样；耦合更加宽松；类型和方法之间的关联由接收者来建立。 
方法没有和数据定义（结构体）混在一起：它们是正交的类型；表示（数据）和行为（方法）是独立的。

注意:`Go`语言不允许为简单的内置类型添加方法，所以下面定义的方法是非法的。
```go
package main

import (
	"fmt"
)

func Add(a, b int) { //函数合法
	fmt.Println(a + b)
}

func (a int) Add(b int) { //方法非法！不能是内置数据类型
	fmt.Println(a + b)
}
```

这个时候我们需要用`Go`语言的`type`，来临时定义一个和`int`具有同样功能的类型。这个类型不能看成是`int`类型的别名，它们属于不同的类型，不能直接相互赋值。

修改后合法的方法定义如下:    
```go
package main

import (
	"fmt"
)

type myInt int

func Add(a, b int) { //函数
	fmt.Println(a + b)
}

func (a myInt) Add(b myInt) { //方法
	fmt.Println(a + b)
}

func main() {
	a, b := 3, 4
	var aa, bb myInt = 3, 4
	Add(a, b)
	aa.Add(bb)
}
```

上面的表达式`aa.Add`称作选择子`(selector)`它为接收者`aa`选择合适`的Add`方法。

“类的”方法
---

`Go`语言不像其它面相对象语言一样可以写个类，然后在类里面写一堆方法，但其实`Go`语言的方法很巧妙的实现了这种效果:我们只需要在普通函数前面加个接受者(`receiver`，写在函数名前面的括号里面)，这样编译器就知道这个函数（方法）属于哪个`struct`了。例如:    
```go
package main

import (
	"fmt"
)

type A struct {
	Name string
}

func (a A) foo() { //接收者写在函数名前面的括号里面
	fmt.Println("foo")
}

func main() {
	a := A{}
	a.foo() //foo
}
```


最后插一点知识，上面再讲方法的时候用到了`type myInt int`，这是干什么用的呢？这里是类型等价定义，相当于类型重命名，只要这样定以后`myInt`类型就与`int`等价。 
那`type`到底有哪些作用呢？ 这里总结一下:    

- 定义结构体

```go
type Person struct {
	name string
	age int
}
```

- 类型等价定义，相当于类型重命名

```go
type name string

func main() {
    var myname name = "taozs" //其实就是字符串类型
    l := []byte(myname)       //字符串转字节数组
    fmt.Println(len(l))       //字节长度
}
```

- 定义接口 

```go
type Personer interface {
    Run()
    Name() string
}
```

- 定义函数类型

```go
type handler func(name string) int
针对这个函数类型可以再定义方法，如：
func (h handler) add(name string) int {
    return h(name) + 10
}
```



---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 