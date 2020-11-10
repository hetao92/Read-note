[TOC]

# Go

+ 包声明
+ 引入包
+ 函数
+ 变量
+ 语句 & 表达式
+ 注释

```Go
package main

import "fmt"

func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```



当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 protected ）。



**标识符**

标识符实际上就是一个或是多个字母(A~Z和a~z)数字(0~9)、下划线_组成的序列，但是第一个字符必须是字母或下划线而不能是数字。且不能为关键字，不准携带运算符



## 数据类型

+ 布尔型 bool
+ 数字类型 整型 int 和浮点型 float32/float 64 等等
+ 字符串 
+ 派生类型
  + 指针类型 Pointer
  + 数组类型
  + 结构化类型 struct
  + Channel 类型
  + 函数类型
  + 切片类型
  + 接口类型 interface
  + Map 类型



## 变量

### 声明变量

1. 指定变量类型   

   ```go
   var v_name v_type
   v_name = value
   ```

2. 根据值自行判定变量类型 `var v_name = value`
3. 省略 `var`，使用`:=`，确保左侧必须有声明新变量，且只能在函数体中出现，否则会编译错误   `v_name := value`

空白标识符 _ 也被用于抛弃值，如值 5 在：_, b = 5, 7 中被抛弃。



**多变量声明**

```go
//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3

var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断

vname1, vname2, vname3 := v1, v2, v3 // 出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误


// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)
```



### 常量

常量只能是布尔型、数字型和字符串型

`const identifier [type] = value`

`const c_name1, c_name2 = value1, value2`



**iota**

一种特殊常量，可以被编译器修改。在 const 关键字出现时将被重置为 0，然后在 const 每新增一行常量声明都将使 iota 计数一次

```go
func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```



## 运算符

+ 算术运算符
+ 关系运算符
+ 逻辑运算符
+ 位运算符
+ 赋值运算符
+ 其他



## For 循环

三种形式

+ `for init; condition; post { }`
+ `for condition { }`
+ `for {}`



for 循环的 range 格式可以对 slice、map、数组、字符串等进行迭代循环。格式如下：

```go
for key, value := range oldMap {
    newMap[key] = value
}

strings := []string{"google", "runoob"}
for i, s := range strings {
  fmt.Println(i, s)
}
// 0 google
// 1 runoob
```



## 函数

**闭包**

```go
func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}

// 左括号不能单独一行
```



## 数组

```go
var variable_name [SIZE] variable_type
// 初始化数组
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
// {} 中的元素个数不能大于 [] 中的数字
//也可以不设置数组大小，用 [...]，go 会根据元素个数来设置
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```



## 指针

指针变量指向一个值的内存地址

取地址符是 `&`，放到一个变量前使用就会返回相应变量的内存地址。

声明指针

`var name *type` 

type 即为指针类型，name 为指针变量名，`*`用于指定变量时作为一个指针



## 结构体

结构体中可以定义不同项的不同数据类型

```go
type struct_variable_type struct {
   member definition
   member definition
   ...
   member definition
}

// 声明
variable_name := structure_variable_type {value1, value2...valuen}
```



## 切片

切片是对数组的抽象，Go 数组的长度不可改变，相比之下切片的长度不固定，可以追加元素

`var identifier []type`

或使用`make()`来创建切片

```go
var slice1 []type = make([]type, len, capacity)
// capacity 可选参数，指定容量
// 也可以简写为
slice1 := make([]type, len)

s := arr[:] 
// 初始化切片 s 是数组 arr 的引用
s := arr[startIndex:endIndex] 
// 将arr中从下标startIndex到endIndex-1 下的元素创建为一个新的切片, 两个 index 都可以省略

```

`len(s)` 获取切片s长度

`cap(s)` 获取切片 s 最长容量



切片在未初始化之前默认为 nil，长度为 0

增加切片的容量必须创建一个新的更大的切片并把原分片的内容都拷贝过来，`append()` `copy()`

```go
var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)
 	/* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)  
```



## Range

`range`用于`for`循环中迭代数组 array、切片 slice、通道 channel 或集合 map 的元素，返回对应的索引/索引值或 key/value 对

```go
nums := []int{2, 3, 4}
sum := 0
for _, num := range nums {
  sum += num
}
fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
```



## Map 集合

Map 是一种**无序**的键值对的集合，可以迭代，但无法决定返回顺序

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

​    **删除元素** delete(countryCapitalMap, key)



## 接口

接口数据类型将所有具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口

```go
/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}

// example
type Phone interface {
    call()
}

type NokiaPhone struct {
}

func (nokiaPhone NokiaPhone) call() {
    fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {
}

func (iPhone IPhone) call() {
    fmt.Println("I am iPhone, I can call you!")
}

func main() {
    var phone Phone

    phone = new(NokiaPhone)
    phone.call()

    phone = new(IPhone)
    phone.call()

}
```



## 错误处理

error 类型是一个接口类型

```go
type error interface {
    Error() string
}
```



## Channel

通过 channel 管道并发核心单元就可以发送或者接收数据进行通讯

操作符为 `<-` 箭头的指向就是数据的流向

channel 必须先创建再使用，可选容量 capacity 代表 channel 容纳的最多元素的数量，代表 channel 的缓存大小

`ch := make(chan int[, capacity])`

若capacity未设置或为 0 则说明 channel 没有缓存，只有 sender 和 receiver 都准备好了通讯才会发生，如果设置了缓存，则有可能不发生阻塞，只有 buffer 满了后再发送才会阻塞

**类型**

```go
ChannelType = ( "chan" | "chan" "<-" | "<-" "chan" ) ElementType 
// 若没有指定方向，那 channel 就是双向的，既可以接收数据，也可以发送数据
chan T          // 可以接收和发送类型为 T 的数据
chan<- float64  // 只可以用来发送 float64 类型的数据
<-chan int      // 只可以用来接收 int 类型的数据
```



`v, ok := <-ch` 可以用来检查 channel 是否已经被关闭了



