Golang指针、结构体及接口(六)
===


指针
---

一个指针可以指向任何一个值的内存地址,它指向那个值的内存地址，在32位机器上占用4个字节，在64位机器上占用8个字节，
并且与它所指向的值的大小无关。当然，可以声明指针指向任何类型的值来表明它的原始性或结构性；
你可以在指针类型前面加上`*`号来获取指针所指向的内容，这里的`*`号是一个类型更改器。使用一个指针引用一个值被称为间接引用。

在`Go`语言中，直接砍掉了`C`语言指针最复杂的指针运算部分，只留下了获取指针（`&`运算符）和获取对象（`*`运算符）的运算，
用法和`C`语言很类似。    
`Go`语言中一个指针被定义后没有分配到任何变量时，它的值为`nil`。

`Go`语言自带指针隐式解引用:对于一些复杂类型的指针，如果要访问成员变量时候需要写成类似`*p.field`的形式时，
只需要`p.field`即可访问相应的成员。

每个变量都是一个内存位置，每个内存位置都有其定义的地址，可以使用符号`(&)`运算符来访问它，
这个运算符表示内存中的地址。参考下面的例子，它将打印定义的变量的地址：
```go
package main

import "fmt"

func main() {
   var a int = 10   

   fmt.Printf("Address of a variable: %x\n", &a  ) // Address of a variable: c420014080
}
```

指针是一个变量，其值是另一个变量的地址，即存储器位置的直接地址。类似变量或常量一样，必须要先声明一个指针，然后才能使用它来存储任何变量地址。
指针变量声明的一般形式是：
类型`*T`是指向类型`T`的值的指针。指针变量声明的一般形式是:  
```go
var var-name *var-type
```

这里，`var-type`是指针的基类型; 它必须是有效的`Go`数据类型，`var-name`是指针变量的名称。
用于声明指针的星号`(*)`与用于乘法的星号相同。但是，在此语句中，星号`(*)`用于将变量指定为指针。
```go
package main

import "fmt"

func main() {
   var  ptr *int

   fmt.Printf("The value of ptr is : %x\n", ptr  )
}
```

当上面的代码编译和执行时，它产生结果是`The value of ptr is 0`，    
在大多数操作系统上，程序不允许访问地址0处的内存，因为该内存是由操作系统保留的。然而存储器地址`0`具有特殊意义; 它表示指针不打算指向可访问的存储器位置。
但是按照惯例，如果指针包含`nil`(零)值，则假设它不指向任何东西。

要检查是否为`nil`指针，可以使用`if`语句，如下所示:   
```go
if(ptr != nil)     /* succeeds if p is not nil */
if(ptr == nil)    /* succeeds if p is null */
```


`&`符号会生成一个指向其作用对象的指针。
```go
i := 42
p = &i
```
`*`符号表示指针指向的底层的值。
```go
fmt.Println(*p) // 通过指针 p 读取 i
*p = 21         // 通过指针 p 设置 i
```

这也就是通常所说的“间接引用”或“非直接引用”。
与`C`不同，`Go`没有指针运算。
```go
package main

import "fmt"

func main() {
    i, j := 42, 2701

    p := &i         // point to i
    fmt.Println(*p) // read i through the pointer
    *p = 21         // set i through the pointer
    fmt.Println(i)  // see the new value of i

    p = &j         // point to j
    *p = *p / 37   // divide j through the pointer
    fmt.Println(j) // see the new value of j
}
```

new函数
---

使用`new`函数给一个新的结构体变量分配内存，它返回指向已分配内存的指针:   
```go
var t *T = new(T)
```

变量`t`是一个指向`T`的指针，此时结构体字段的值是它们所属类型的零值。

```go
package main

import (
    "fmt"
)

type Student struct {
    name   string
    age    int
    weight float32
    score  []int
}

func main(){
	//使用new关键字创建一个指针
    pp := new(Student)                                  
    *pp = Student{"xiaoming", 23, 65.0, []int{2, 3, 6}}
    fmt.Printf("stu pp have %d subjects\n", len((*pp).score))
    fmt.Printf("stu pp have %d subjects\n", len(pp.score)) //Go语言自带隐式解引用
}
```

执行结果:   
```
stu pp have 3 subjects
stu pp have 3 subjects
```


结构体
---

可以理解成是`java`中的类。   
要定义结构，必须使用`type`和`struct`语句。`struct`语句定义了一个新的数据类型，在程序中有多个成员。
`type`语句在例子中绑定一个类型为`struct`的名字。`struct`语句的格式如下:   

```go
type Student struct {
    name   string
    age    int
    weight float32
    score  []int
}
```

要想调用结构体中的字段直接使用使用点号来访问。

```go
package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    fmt.Println(Vertex{1, 2}) // {1 2}
    v := Vertex{1, 2}
    v.X = 4
    fmt.Println(v.X)    // 4
}
```
结构体字段可以通过结构体指针来访问。
通过指针间接的访问是透明的。
```go
package main

import (
    "fmt"
)

type Student struct {
    name   string
    age    int
    weight float32
    score  []int
}

func main() {
	//按照字段顺序进行初始化
    stu01 := Student{"xiaomi", 23, 55.5, []int{95, 96, 98}}                
    //通过 field:value 形式初始化，该方式可以自定义初始化字段的顺序
    stu02 := Student{age: 23, weight: 55.5, score: []int{97, 98}, name: "xiaohong"} 
    stu01.age = 25
    fmt.Println(stu01.age)
    fmt.Println(stu02)
}
```

结构体符文表示通过结构体字段的值作为列表来新分配一个结构体。
使用`Name:`语法可以仅列出部分字段。(字段名的顺序无关。)
特殊的前缀`&`返回一个指向结构体的指针。

```go
package main

import "fmt"

type Vertex struct {
    X, Y int
}

var (
    v1 = Vertex{1, 2}  // 类型为 Vertex
    v2 = Vertex{X: 1}  // Y:0 被省略
    v3 = Vertex{}      // X:0 和 Y:0
    p  = &Vertex{1, 2} // 类型为 *Vertex
)

func main() {
    fmt.Println(v1, p, v2, v3) // {1 2} &{1 2} {1 0} {0 0}
}
```


结构体new函数
---

一般在进行例如`type T struct {a, b int}`的结构体定义之后，习惯使用`t := new(T)`给该结构体变量分配内存，它返回指向已分配内存的指针。
变量`t`是一个指向`T`的指针，此时结构体字段的值是它们所属类型的零值。     
声明`var t  T`也会给`t`分配内存，并零值化内存，但是这个时候`t`是类型`T`。在这两种方式中，`t`通常被称做类型T的一个实例`（instance）`或对象`（Object）`。

一个简单的结构体`new`函数的例子如下:    
```go
package main
import "fmt"

type struct1 struct {
    i1  int
    f1  float32
    str string
}

func main() {
    ms := new(struct1) // ms是结构体struct1的指针
    ms.i1 = 10
    ms.f1 = 15.5
    ms.str= "Chris"

    fmt.Printf("The int is: %d\n", ms.i1)
    fmt.Printf("The float is: %f\n", ms.f1)
    fmt.Printf("The string is: %s\n", ms.str)
    fmt.Println(ms)
}
```
执行结果:   
```
The int is: 10
The float is: 15.500000
The string is: Chris
&{10 15.5 Chris}
```

匿名字段
---

结构体可以包含一个或多个匿名（或内嵌）字段，即这些字段没有显式的名字，只有字段的类型是必须的，
此时类型就是字段的名字。匿名字段本身可以是一个结构体类型，即 结构体可以包含内嵌结构体。
```go
package main

import "fmt"

type innerS struct {
	in1 int
	in2 int
}

type outerS struct {
	b int
	c float32
	int    // anonymous field
	innerS //anonymous field
}

func main() {
	outer := new(outerS)
	outer.b = 6
	outer.c = 7.5
	outer.int = 60
	outer.in1 = 5
	outer.in2 = 10

	fmt.Printf("outer.b is: %d\n", outer.b)
	fmt.Printf("outer.c is: %f\n", outer.c)
	fmt.Printf("outer.int is: %d\n", outer.int)
	fmt.Printf("outer.in1 is: %d\n", outer.in1)
	fmt.Printf("outer.in2 is: %d\n", outer.in2)

	// 使用结构体字面量
	outer2 := outerS{6, 7.5, 60, innerS{5, 10}}
	fmt.Println("outer2 is:", outer2)
}
```
输出结果:    
```
outer.b is: 6
outer.c is: 7.500000
outer.int is: 60
outer.in1 is: 5
outer.in2 is: 10
outer2 is: {6 7.5 60 {5 10}}
```

通过类型`outer.int`的名字来获取存储在匿名字段中的数据，于是可以得出一个结论：
在一个结构体中对于每一种数据类型只能有一个匿名字段。

内嵌结构体
---

同样地结构体也是一种数据类型，所以它也可以作为一个匿名字段来使用，如同上面例子中那样。
外层结构体通过`outer.in1`直接进入内层结构体的字段，内嵌结构体甚至可以来自其他包。

在其他语言中，有继承可以做同样的事情，但是在`Go`语言中，没有继承的概念，`Go`提倡的代码复用的方式是组合，所以这也是嵌入类型的意义所在，组合而不是继承，所以`Go`才会更灵活。
内层结构体被简单的插入或者内嵌进外层结构体。这个简单的“继承”机制提供了一种方式，使得可以从另外一个或一些类型继承部分
或全部实现。
```go
package main

import "fmt"

type A struct {
	ax, ay int
}

type B struct {
	A
	bx, by float32
}

func main() {
	b := B{A{1, 2}, 3.0, 4.0}
	fmt.Println(b.ax, b.ay, b.bx, b.by)
	fmt.Println(b.A)
}
```
执行结果:

```
1 2 3 4
{1 2}
```

同样这种嵌入类型也可以使用到接口中:   
```go
type Reader interface {
    Read(p []byte) (n int, err error)
}
type Writer interface {
    Write(p []byte) (n int, err error)
}
type Closer interface {
    Close() error
}
type ReadWriter interface {
    Reader
    Writer
}
type ReadCloser interface {
    Reader
    Closer
}
type WriteCloser interface {
    Writer
    Closer
}
```

以上是标准库`io`包里，我们常用的接口，可以看到`ReadWriter`接口是嵌入`Reader`和`Reader`接口而组合成的新接口，这样我们就不用重复的定义被嵌入接口里的方法，直接通过嵌入就可以了。



接口
---


`Go`编程提供了另一种称为接口`(interfaces)`的数据类型，它代表一组方法签名。
定义了一个接口之后，一般使用一个自定义结构体`(struct)`去实现接口中的方法。在`Go`语言中，任何实现了接口的函数的类型，都可以看作是接口的一个实现。
在`Go`中，如果你这个类型（基本类型，结构体等都可以）拥有了接口的所有方法，那么就默认为这个类型实现了这个接口，是隐式的，不需要和`java`一样，强制使用`implement`强制实现。

```go
/* define an interface */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* define a struct */
type struct_name struct {
   /* variables */
}

/* implement interface methods*/
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* method implementation */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* method implementation */
}
```
示例:    
```go
package main

import (
	"fmt"
)

type Phone interface {
	call()
}

type NokiaPhone struct{}
type IPhone struct{}

// NokiaPhone结构体实现Phone接口的call方法
func (nokiaPhone NokiaPhone) call() {
	fmt.Println("I am Nokia, I can call you!")
}

// IPhone结构体实现Phone接口的call方法
func (iPhone IPhone) call() {
	fmt.Println("I am iPhone, I can call you!")
}

func main() {
	var phone Phone

	phone = NokiaPhone{}
	phone.call()

	phone = IPhone{}
	phone.call()
}
```
执行结果:   
```shell
I am Nokia, I can call you!
I am iPhone, I can call you!
```




---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 