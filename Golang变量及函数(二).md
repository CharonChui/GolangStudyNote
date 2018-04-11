Golang变量及函数(二)
===


基本数据类型
---

```go
bool  // 一个字节，值是true或者false

string

int int8 int16 int32 int64 // 带符号的

uint uint8 uint16 uint32 uint64 uintptr // 无符号的

byte // 占用8位，一个字节，相当于uint8

rune // int32相同

float32 float64

complex64 complex128 // 复数带有float32实部和虚部  复数带有float64实部和虚部
```

`int`、`uint`和`uintptr(无符号整数用于存储指针值的未解释位)`类型在`32`位的系统上一般是`32`位，而在`64`位系统上是`64`位。
当你需要使用一个整数类型时，应该首选`int`，仅当有特别的理由才使用定长整数类型或者无符号整数类型。

引用(结构化)类型
---

- slice  
- map
- channel
- struct
- array

结构化的类型没有真正的值，它使用`nil`作为默认值（在`Objective-C`中是`nil`，在`Java`中是`null`，在`C`和`C++`中是`NULL`或`0`）。值得注意的是，`Go`语言中不存在类型继承。

接口类型
---

- interface

函数类型
---

- func

其他类型
---

- arry
- struct
- string


变量
---

`var`语句定义了一个变量的列表；跟函数的参数列表一样，类型在后面。就像在这个例子中看到的一样，`var`语句可以定义在包
或函数级别。变量在定义时没有明确的初始化时会赋值为 零值 。

```go
package main

import "fmt"

var c, python, java bool // boolean 类型

func main() {
    var i int
    fmt.Println(i, c, python, java) // 打印出来都是默认的值0 false false false
}
```

变量定义可以包含初始值，每个变量对应一个。如果初始化是使用表达式，则可以省略类型；变量从初始值中获得类型。
```go
package main

import "fmt"

var i, j int = 1, 2

func main() {
    var c, python, java = true, false, "no!"
    fmt.Println(i, j, c, python, java)
}
```


- `=`是赋值，在使用之前必须先用`var`将变量进行声明
- `:=`是声明变量并赋值，并且系统会自动腿短类型，不需要使用`var`关键字声明，`:=`结构不能使用在函数外。

下面的这几种写法都是声明变量并赋值:   
```go
var a
a = 100
var b = 100
var c int = 100
d := 100
```



常量
---

常量的定义与变量类似，只不过不是使用`var`而是使用`const`关键字。

常量可以是字符、字符串、布尔或数字类型的值。常量不能使用`:=`语法定义。
```go
package main

import "fmt"

const Pi = 3.14

func main() {
    const World = "世界"
    fmt.Println("Hello", World)
    fmt.Println("Happy", Pi, "Day")

    const Truth = true
    fmt.Println("Go rules?", Truth)
}
```

如果有多个敞亮也可以一起声明: 
```go
const (
    Name = "a"
    Age = 18
)
```

类型转换
---

`Go`的在不同类型之间的项目赋值时需要显式转换。   
```go
var i int = 42
var f float64 = float64(i) // 将int转换为float64
var u uint = uint(f) // 将float64转换为unit
```
类型声明
---

变量或表达式的类型定义了对应存储值的属性特征，例如数值在内存的存储大小（或者是元素的`bit`个数），它们在内部是如何表达的，
是否支持一些操作符，以及它们自己关联的方法集等。在任何程序中都会存在一些变量有着相同的内部结构，但是却表示完全不同的概念。
例如，一个`int`类型的变量可以用来表示一个循环的迭代索引、或者一个时间戳、或者一个文件描述符、或者一个月份；
一个`float64`类型的变量可以用来表示每秒移动几米的速度、或者是不同温度单位下的温度；
一个字符串可以用来表示一个密码或者一个颜色的名称。

一个类型声明语句创建了一个新的类型名称，和现有类型具有相同的底层结构。新命名的类型提供了一个方法，用来分隔不同概念的类型，
这样即使它们底层类型相同也是不兼容的:        
```go
type 类型名字 底层类型
```
类型声明语句一般出现在包一级，因此如果新创建的类型名字的首字符大写，则在外部包也可以使用。
```go
package tempconv

import "fmt"

type Celsius float64    // 摄氏温度
type Fahrenheit float64 // 华氏温度

const (
    AbsoluteZeroC Celsius = -273.15 // 绝对零度
    FreezingC     Celsius = 0       // 结冰点温度
    BoilingC      Celsius = 100     // 沸水温度
)

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }

func FToC(f Fahrenheit) Celsius { return Celsius((f - 32) * 5 / 9) }
```




函数
---

不支持嵌套、重载和默认参数。 
函数的声明是通过`func`关键字:   
```go
func 函数名(参数列表) (返回值列表) {
    // 函数体
}
```
如:   
```go
func add(x int, y int) int {
    return x + y
}
```
注意上面的参数类型是在变量名之后的。      
当两个或多个连续的函数命名参数是同一类型，则除了最后一个类型之外，其他都可以省略，
上面的例子也可以写成:   
```go
func add(x, y int) int {
    return x + y
}
```

函数可以返回任意数量的返回值，上面的`add`函数返回了一个`int`类型的返回值。
下面:  
```go
package main
import "fmt"
func swap(x, y string) (string, string) {
    return y, x
}

func main() {
    a, b := swap("hello", "world") // 注意了这里用的是 := 这是为什么？ 为什么不用=
    fmt.Println(a, b) // 打印出来的结果是world hellow
}
```

可变参数的函数可以用任何数量的参数来调用。 例如，`fmt.Println()`就是一个常见的可变函数。

<img src="https://raw.githubusercontent.com/CharonChui/Pictures/master/go_print_params.png?raw=true" width = "100%"/>


可以使用`... XXX`来表示接受任意数量的`XXX`类型的参数。  
```go
func sum(nums ...int) {
    fmt.Print(nums, " ")
    total := 0
    for _, num := range nums {
        total += num
    }
    fmt.Println(total)
}
```


上面使用了`_`这是什么？ 

空白符
---


空白符用来匹配一些不需要的值，然后丢弃掉。

下面的例子中，`ThreeValues`是拥有三个返回值的不需要任何参数的函数，在下面的例子中，我们将第一个与第三个返回值赋给了
`i1`与`f1`。第二个返回值赋给了空白符`_`然后自动丢弃掉。
```go
package main

import "fmt"

func main() {
    var i1 int
    var f1 float32
    i1, _, f1 = ThreeValues()
    fmt.Printf("The int: %d, the float: %f \n", i1, f1)
}

func ThreeValues() (int, int, float32) {
    return 5, 6, 7.5
}
```

输出结果:   
```
The int: 5, the float: 7.500000 
```


匿名函数(闭包)
---

`Go`语言支持匿名函数，可以形成闭包。匿名函数在想要定义函数而不必命名时非常有用。

```go
package main

import "fmt"

// This function `intSeq` returns another function, which
// we define anonymously in the body of `intSeq`. The
// returned function _closes over_ the variable `i` to
// form a closure.
func intSeq() func() int {
    i := 0
    return func() int {
        i += 1
        return i
    }
}

func main() {

    // We call `intSeq`, assigning the result (a function)
    // to `nextInt`. This function value captures its
    // own `i` value, which will be updated each time
    // we call `nextInt`.
    nextInt := intSeq()

    // See the effect of the closure by calling `nextInt`
    // a few times.
    fmt.Println(nextInt())
    fmt.Println(nextInt())
    fmt.Println(nextInt())

    // To confirm that the state is unique to that
    // particular function, create and test a new one.
    newInts := intSeq()
    fmt.Println(newInts())
}
```
执行结果是:   
```
1
2
3
1
```

内置函数
----



`Go`语言拥有一些不需要进行导入操作就可以使用的内置函数。它们有时可以针对不同的类型进行操作，例如:`len`、`cap`和`append`，
或必须用于系统级的操作，例如:`panic`。因此，它们需要直接获得编译器的支持。

以下是一个简单的列表:    

- `close`:用于管道通信
- `len、cap`:`len`用于返回某个类型的长度或数量（字符串、数组、切片、`map`和管道）`cap`是容量的意思，
    用于返回某个类型的最大容量（只能用于切片和`map`）
- `new、make`:`new`和`make`均是用于分配内存:`new`用于值类型和用户定义的类型，如自定义结构，
    `make`用于内置引用类型（切片、`map`和管道）。它们的用法就像是函数，但是将类型作为参数:`new(type)`、`make(type)`。
    `new(T)`分配类型`T`的零值并返回其地址，也就是指向类型`T`的指针它也可以被用于基本类型`v := new(int)`。
    `make(T)`返回类型`T`的初始化之后的值，因此它比`new`进行更多的工作，`new()`是一个函数，不要忘记它的括号
- `copy、append`:用于复制和连接切片
- `panic、recover`: 两者均用于错误处理机制
- `print、println`:底层打印函数，在部署环境中建议使用`fmt`包
- `complex、real imag`:用于创建和操作复数


  
---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 