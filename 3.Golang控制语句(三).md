Golang控制语句(三)
===


循环
---

`Go`中只有一种循环结构--`for`循环。 

基本的for循环包含三个由分号分开的组成部分:   

- 初始化语句：在第一次循环执行前被执行 
- 循环条件表达式：每轮迭代开始前被求值 
- 后置语句：每轮迭代后被执行

基本格式为:   
```go
for 初始化语句; 条件语句; 修饰语句 {
    //循环语句
}
```

注意:不像`C`和`Java`等其他语言，在`go`中`for`语句的三个组成部分并不需要用括号括起来，但循环体必须用`{ }`括起来。
```go
package main

import "fmt"

func main() {
    sum := 0
    for i := 0; i < 10; i++ {
        sum += i
    }
    fmt.Println(sum)
}
```
循环初始化语句和后置语句都是可选的，所以上面的示例代码也可以这样写:   
```go
func main() {
    sum := 1
    for ; sum < 1000; {
        sum += sum
    }
    fmt.Println(sum)
}
```

而且上面的分号也可以省略:  
```go
func main() {
    sum := 1
    for sum < 1000 {
        sum += sum
    }
    fmt.Println(sum)
}
```
如果省略了循环条件，就是死循环:   
```go
func main() {
    for {// 无退出条件，变成死循环
    }
}
```

if
---

就像`for`循环一样，`Go`的`if`语句也不要求用`( )`将条件括起来，同时，`{ }`还是必须有的。

```go
if b {
    
} else {
    
}
```

switch
---

相比较`C`和`Java`等其它语言而言，`Go`语言中的`switch`结构使用上更加灵活。它接受任意形式的表达式。  
`switch`的条件从上到下的执行，当匹配成功的时候停止。
格式:   
```go
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```

变量`var1`可以是任何类型，而`val1`和`val2`则可以是同类型的任意值。类型不被局限于常量或整数，但必须是相同的类型；或者最终结果为相同类型的表达式。

示例:   
```go
switch result := calculate(); {
    case result < 0:
        ...
    case result > 0:
        ...
    default:
        // 0
}
```
`switch`语句中，如果在执行完每个分支的代码后，还希望继续执行后续分支的代码，可以使用`fallthrough`关键字来达到目的。
```go
package main

import(
  "fmt"
)

func main() {
  i :=2
  switch i {
      case 0:
          fmt.Printf("0")
      case 1:
          fmt.Printf("1")
      case 2:
          fallthrough  //fallthrough会强制执行后面的case代码
      case 3:
          fmt.Printf("3")
      default:
          fmt.Printf("Default")
  }
}
```
上面的执行结果是:3


另外:`Go`也像`java`一样支持`lable`、`goto`、`break`、`continue`等，这里就不介绍了。

---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 