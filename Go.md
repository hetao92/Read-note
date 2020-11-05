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



or 循环的 range 格式可以对 slice、map、数组、字符串等进行迭代循环。格式如下：

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