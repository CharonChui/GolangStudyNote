Golang数组、切片及映射(四)
===


数组
---

数组就是指一系列同一类型数据的集合。数组中包含的每个数据被称为数组元素`element`，一个数组包含的元素个数被称为数组的长度。需要强调的一点是`Go`语言中数组的长度固定，无法扩容。

类型`[n]T`是一个有`n`个类型为`T`的值的数组。

定义变量`a`是一个有十个整数的数组:    
```go
var a [10]int // 10个int型的数组，初始值是10个0,数组“零值”状态

var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance[4] = 50.0 // 将第四个元素的值设为50

var balance2 = []float32{1000.0, 2.0, 3.4, 7.0, 50.0} // 如果省略数组的大小，则只创建一个足够容纳初始化的数组。

q := [...] int {1,2,3} //不声明长度
r := [...] int {99:-1}  //长度为100的数组，只有最后一个是-1，其他都是0

```

`Go`语言中，可以使用数组下标来访问数组中的元素。数组下标从`0`开始，`len(arr)-1`则表示最后一个元素的下标。

只有有效的索引可以被使用，当使用等于或者大于`len(arr)`的索引时:如果编译器可以检测到，会给出索引超限的提示信息；如果检测不到的话编译会通过而运行时会`panic`。


`Go`语言中数组是具有固定长度而且拥有零个或者多个相同或相同数据类型元素的序列。由于数组长度固定，所以在`Go`语言比较少直接使用。而`slice`长度可增可减，
使用场合比较多。更深入的区别在于:数组在使用的过程中都是值传递，将一个数组赋值给一个新变量或作为方法参数传递时，是将源数组在内存中完全复制了一份，
而不是引用源数组在内存中的地址。为了满足内存空间的复用和数组元素的值的一致性的应用需求，`slice`出现了，每个`slice`都是都源数组在内存中的地址的一个引用，源数组可以衍生出多个`slice`，`slice`也可以继续衍生其他`slice`。

切片`(slice)`
---

`Go`语言中数组是值类型，长度不可伸缩；而`slice`是引用类型，长度可动态增长。

`Go`语言中，`slice`表示一个拥有相同类型元素的可变长度序列。`slice`通常被写为`[]T`，其中元素的类型都是`T`；它看上去就像没有长度的数组类型。

`slice`有三个属性: 

- 指针:指针通常是从指向数组的第一个可以从`slice`中访问的元素，这个元素不一定是数组的第一个元素
- 长度:长度指的是`slice`中的元素个数，它不能超过`slice`的容量
- 容量:容量的大小通常大于等于长度，会随着元素个数增多而动态变化

一个`slice`会指向一个序列的值，并且包含了长度信息。     
`[]T`是一个元素类型为`T`的切片`(slice)`。     
`len(s)`返回`slice s`的长度。
要创建非零长度的空切片，请使用内置`make()`函数。这里创建一个长度为3的字符串(初始为零值)。

```go
package main

import "fmt"

func main() {
    s := []int{2, 3, 5, 7, 11, 13}
    fmt.Println("s ==", s)

    for i := 0; i < len(s); i++ {
        fmt.Printf("s[%d] == %d\n", i, s[i])
    }
}
```
打印结果为:   

```
s == [2 3 5 7 11 13]
s[0] == 2
s[1] == 3
s[2] == 5
s[3] == 7
s[4] == 11
s[5] == 13
```

`Go`语言的内置函数`len`和`cap`用来返回`slice`的长度和容量。在追加元素时，如果容量`cap`不足时，`cap`**一般**变为原来的2倍来实现扩容。

`slice`的`cap`扩容规则:   

- 如果新的`cap`大小是当前`cap`的`2`倍以上，则直接扩容为这个新的`cap`； 
- 否则循环以下操作：如果当前`cap`小于1024，按每次2倍增长，否则每次按当前大小1/4增长。直到增长的大小超过或等于新的`cap`。




切片`(slice)`的切片
---

`Go`切片`(Slice)`是`Go`数组的一个抽象。 由于`Go`数组允许定义类型的变量，可以容纳相同数据类型的几个数据项，但它不提供任何内置的方法来动态增加其大小或获取自己的子数组。切片就没有这样的限制。 它提供了数组所需的许多实用功能，并广泛用于`Go`编程。

要定义切片，可以将其声明为数组，而不指定大小或使用`make`函数创建一个。

```go
var numbers []int /* a slice of unspecified size */
/* numbers == []int{0,0,0,0,0}*/
numbers = make([]int,5,5) /* a slice of length 5 and capacity 5*/
```

切片`(slice)`可以包含任意的类型，包括另一个`slice`。

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    // Create a tic-tac-toe board.
    game := [][]string{
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
    }

    // The players take turns.
    game[0][0] = "X"
    game[2][2] = "O"
    game[2][0] = "X"
    game[1][0] = "O"
    game[0][2] = "X"

    printBoard(game)
}

func printBoard(s [][]string) {
    for i := 0; i < len(s); i++ {
        fmt.Printf("%s\n", strings.Join(s[i], " "))
    }
}
```

运行结果是:   
```
X _ X
O _ _
X _ O
```


因为切片`(Slice)`是数组上的抽象。 它实际上使用数组作为底层结构体.`len()`函数返回切片中存在的元素数量，
其中`cap()`函数返回切片`(Slice)`的容量(大小)，即可容纳多少个元素。 以下是解释切片`(Slice)`的用法的示例:    
```go
package main

import "fmt"

func main() {
   var numbers = make([]int,3,5)

   printSlice(numbers)
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x) // len=3 cap=5 slice=[0 0 0]
}
```

如果缺省情况下声明没有输入切片，则将其初始化为`nil`。 其长度和容量为零。 
```go
package main

import "fmt"

func main() {
   var numbers []int

   printSlice(numbers)

   if(numbers == nil){
      fmt.Printf("slice is nil")
   }
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

切片`(Slice)`允许指定下界和上界，以使用`[lower-bound：upper-bound]`获取它的子切片。

如`numbers[1:4]`

切片`(Slice)`允许使用`append()`函数增加切片的容量(大小)。使用`copy()`函数，将源切片的内容复制到目标切片。以下是示例:   
```go
package main

import "fmt"

func main() {
   var numbers []int
   printSlice(numbers)

   /* append allows nil slice */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* add one element to slice*/
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* add more than one element at a time*/
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* create a slice numbers1 with double the capacity of earlier slice*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* copy content of numbers to numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)   
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

执行结果是:   
```
len=0 cap=0 slice=[]
len=1 cap=2 slice=[0]
len=2 cap=2 slice=[0 1]
len=5 cap=8 slice=[0 1 2 3 4]
len=5 cap=16 slice=[0 1 2 3 4]
```

切片的合并
---

`append`的第一个参数是`slice`，第二个参数是元素。结合下`Go`语言函数的不定参数，我们可以用`append`优雅地实现两个`slice`的拼接。例如，下面的函数能合并两个`slice`的全部元素到一个新的`slice`，并返回新`slice`的长度`len`和容量`cap`:   

```go
func main() {
  a := []int{1, 3, 5}
  b := []int{2, 4, 6}
  c := append(a, b...)
  fmt.Print(len(c), cap(c))
}
```

映射
---

`Go`语言中`map`是一种特殊的数据结构:一种元素对`(pair)`的无序集合，`pair`的一个元素是`key`，对应的另一个元素是`value`，所以这个结构也称为关联数组或字典。这是一种快速寻找值的理想结构:给定`key`，对应的`value`可以迅速定位。简单的理解就是`java`中的`map`集合，用来存储键值对。 

`map`是引用类型，内存用`make`方法来分配。一旦容量不够，它会自动扩容。
必须要使用`make`函数来创建映射。

```go
/* declare a variable, by default map will be nil*/
var map_variable map[key_data_type]value_data_type

/* define the map as nil map can not be assigned any value*/
map_variable = make(map[key_data_type]value_data_type)

make(map[KeyType]ValueType, initialCapacity)
make(map[KeyType]ValueType)
map[KeyType]ValueType{}
map[KeyType]ValueType{key1 : value1, key2 : value2, ... , keyN : valueN}

```

示例:   
```go
package main

import "fmt"

func main() {
   var countryCapitalMap map[string]string
   /* create a map*/
   countryCapitalMap = make(map[string]string)

   /* insert key-value pairs in the map*/
   countryCapitalMap["France"] = "Paris"
   countryCapitalMap["Italy"] = "Rome"
   countryCapitalMap["Japan"] = "Tokyo"
   countryCapitalMap["India"] = "New Delhi"

   /* print map using keys*/
   for country := range countryCapitalMap {
      fmt.Println("Capital of",country,"is",countryCapitalMap[country])
   }

   /* test if entry is present in the map or not*/
   capital, ok := countryCapitalMap["United States"]
   /* if ok is true, entry is present otherwise entry is absent*/
   if(ok){
      fmt.Println("Capital of United States is", capital)  
   }else {
      fmt.Println("Capital of United States is not present") 
   }

   delete(countryCapitalMap,"France"); // delete()用来根据key来删除对应的数据
}
```
结果:   
```
Capital of India is New Delhi
Capital of France is Paris
Capital of Italy is Rome
Capital of Japan is Tokyo
Capital of United States is not present
```

这里需要强调下，根据键值索引某个元素时，也会返回两个值：索引到的值和本次索引是否成功（这里可能会因为索数值越界或者索引键值有误而导致索引失败）。
```go
package main

import(
    "fmt"
)

func main(){
    ages01 := map[string]int{
        "alice":31,
        "bob":13,
    }

    age,ok := ages01["bo"]   //age才是根据键值索引到的值
    if !ok{
        fmt.Printf("索引失败，bo不是map的键值，此时age=%d",age)  //索引失败会返回value的零值，这里是int类型，所以是0
    }   else{
        fmt.Printf("索引成功，age=%d",age)
    }
}
```

范围`(range)`
---

`for`循环的`range`格式可以对`slice`或者`map`进行迭代循环。

当使用`for`循环遍历一个`slice`时，每次迭代`range`将返回两个值。 第一个是当前下标(序号)，
第二个是该下标所对应元素的一个拷贝。

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
    for i, v := range pow {
        fmt.Printf("2**%d = %d\n", i, v)
    }
}
```


---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 