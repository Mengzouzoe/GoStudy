## Go基础语法

###基础知识

##### Go程序的一般结构

```go
// go b当前程序的包名
package main

// 导入其它的包
import "fmt"

// 常量的定义
const PI = 3.14

// 全局变量的声明与赋值
var globalvar = "world"

// 一般类型声明
type IntType int

// 结构的声明
type gopher struct{}

// 接口的声明
type golang interface{}

// 由 main 函数作为程序入口点启动
func main() {
  fmt.Println("Hello World!")
}
```



#####包的导入

程序通过package 组织

只有package名称为main的包可以包含main函数

一个可执行程序有且仅有一个main包



关键字const进行常量的定义

通过在函数体外部使用var关键字来进行全局变量的声明

通过type关键字来进行结构(struct)或接口(interface)的声明



##### Package别名以及省略调用

package别名

```go
import std "fmt"

func main() {
  std.Println("Hello World!")
}
```



省略调用

易混淆，不建议使用；不可与别名同时使用

```go
import . "fmt"

func main() {
  Println("Hello World!")
}
```



#####可见性规则

可见性规则

使用大小写来决定该常量、变量、类型、接口、结构或函数是否可以被外部包所调用

函数名首字母小写即为private， 函数名首字母大写即为public



###类型与变量

#####基本类型

布尔型 不可以用数字代表true或false

整型 根据运行平台为32/64

8位整型 int8/uint8

字节型 byte(unit8别名)

rune(int32别名)

足够保存指针的32位或64位整型 uintptr



#####类型零值

零值并不等于空值，而是当变量被声明为某种类型后的默认值，

通常情况下值类型的默认值为0，bool为false，string为空字符串



#####类型的声明与赋值

```go
//变量的声明格式：var <变量名称> <变量类型>
//变量的赋值格式：<变量名称> = <表达式>
var a int 
a = 123

//声明的同时赋值
var b int = 123
//省略变量类型
var c =321
//最简写法
d := 456
```



######多个变量的声明与赋值

-全局变量的声明可使用 var() 的方式进行简写

-全局变量的声明不可以省略 var，但可使用并行方式

-所有变量都可以使用类型推断

-局部变量不可以使用 var() 的方式简写，只能使用并行方式

```go
var (
	aaa = "hello"
  bbb, ccc = 111, 222
  //不可以省略var
  //ddd := 333
)

var a, b, c int
a, b, c = 1, 2, 3

var d, e, f int = 4, 5, 6
//var d, e, f = 4, 5, 6

g, h := 7, 8
```



#####类型转换

-Go中不存在隐式转换，所有类型转换必须显式声明

-转换只能发生在两种相互兼容的类型之间

```go
var a float32 = 3.1
b := int(a)

//以下表达式是无法通过编译
var c bool = true
d := int(c)
```



#####作业

```go
//string() 表示将数据转换成文本格式，输出A
func main() {
	var a int = 65
	b := string(a)
	fmt.Println(b)
}
```

利用strconv包实现整型与字符型转换

```go
//int -> str
strconv.Itoa()

//str -> int
strconv.Atoi()
```



###常量与运算符

#####常量的定义

常量的值在编译时已经确定

等号右侧必须是常量或者常量表达式

常量表达式中的函数必须是内置函数

私有常量以_和c开头



#####常量的枚举

```go
const (
  //第一个常量不可省略表达式
  a = "A"
  //在定义常量组时，如果不提供初始值，则表示将使用上行的表达式,故b为"A"
  b
  c = iota
  d //iota是常量的计数器，从0开始，组中每定义1个常量自动递增1，此处d值为3
)

const (
  e = iota
  //每遇到一个const关键字，iota就会重置为0,故f值为1
  f
)
```

使用相同的表达式不代表具有相同的值



#####运算符

判断^为一元还是二元运算符：两边是否为数字

&& 若第一个式子错误不需要计算第二个

1 << 10 (KB) << 10(MB)  >> 10(KB)



##### 作业

-请尝试结合常量的iota与<<运算符实现计算机储存单位的枚举

```go
const (
	_ = iota
  KB float64 = 1 << (iota * 10)
  MB
  GB
  TB
  PB
  EB
  ZB
  YB
)
```



### 指针

采用”.”选择符来操作指针目标对象的成员

-操作符”&”取变量地址，使用”*”通过指针间接访问目标对象

-默认值为 nil 而非 NULL



### 控制语句

##### If

-条件表达式没有括号

-支持一个初始化表达式（可以是并行方式）

-左大括号必须和条件语句或else在同一行

-支持单行模式

-初始化语句中的变量为block级别，同时隐藏外部同名变量

```go
func main() {
	a := true
	if a, b, c := 1, 2, 3; a+b+c > 6 {
		fmt.Println("More than 6.")
	} else {
		fmt.Println("Equal or Less than 6.")
		fmt.Println(a)
	}
	fmt.Println(a)
}
```

Equal or Less than 6.
1
true



##### For

-初始化和步进表达式可以是多个值

-提前计算好条件并以变量或常量代替在条件语句中使用函数

-左大括号必须和条件语句在同一行

```go
//Out: 4
//1
func main() {
	a := 1
	for {
		a++
		if a > 3 {
			break
		}
	}
	fmt.Println(a)
}


//2
func main() {
	a := 1
	for a <= 3 {
		a++
	}
	fmt.Println(a)
}

//3
package main

import "fmt"

func main() {
	a := 1
	for i := 0; i < 3; i++ {
		a++
	}
	fmt.Println(a)
}
```



#####Switch

-可以使用任何类型或表达式作为条件语句

-不需要写break，一旦条件符合自动终止

-如希望继续执行下一个case，需使用fallthrough语句

-支持一个初始化表达式（可以是并行方式），右侧需跟分号

-左大括号必须和条件语句在同一行



```go
//Out:
a=1
1

//1
func main() {
	a := 1
	switch a {
	case 0:
		fmt.Println("a=0")
	case 1:
		fmt.Println("a=1")
	}
	fmt.Println(a)
}

//Out:
a=0
a=1
1
//2
func main() {
	a := 1
	switch {
	case a >= 0:
		fmt.Println("a=0")
		fallthrough
	case a >= 1:
		fmt.Println("a=1")
	}
	fmt.Println(a)
}

//Out:
a=0
a=1
//3
package main

import "fmt"

func main() {
	switch a := 1; {
	case a >= 0:
		fmt.Println("a=0")
		fallthrough
	case a >= 1:
		fmt.Println("a=1")
	}
}
```



##### goto, break, continue

-标签名区分大小写，若不使用会造成编译错误

-Break与continue配合标签可用于多层循环的跳出

-Goto是调整执行位置，与其它2个语句配合标签的结果并不相同

```go
//OUT:
0
1
2
func main() {
LABEL:
	for {
		for i := 0; i < 10; i++ {
			if i > 2 {
				break LABEL
			} else {
				fmt.Println(i)
			}
		}
	}
}

//OUT:
0 1 2 3 4 5 6 7 8 9
func main() {
LABEL:
	for i := 0; i < 10; i++ {
		for {
			fmt.Println(i)
			continue LABEL
		}
	}
}
```



### 数组Array

##### Array的定义

定义数组的格式：var <varName> [n]<type>，n>=0

数组长度也是类型的一部分，因此具有不同长度的数组为不同类型

数组在Go中为值类型

```go
//Out: [0 0]
func main() {
  var a [2]int
  var b [2]int
  a = b
  fmt.Println(b)
}

//Out: [0 0 1]
func main() {
  a := [3]int{2: 1}
  fmt.Println(a)
}

//Out: [1 2 3 4]
func main() {
  a := [...]int{1, 2, 3, 4}
  fmt.Println(a)
}

//Out: [1 2 3]
func main() {
  a := [...]int{0: 1,1: 2,2: 3}
  fmt.Println(a)
}
```



##### 指向数组的指针以及指针数组

```go
func main() {
  //指针数组
  x, y := 1, 2
  a := [...]*int{&x, &y}
  fmt.Println(a)
  
  //指向数组的指针
  var p *[3]int = &a
  fmt.Println(p)
}
```



##### 数组之间的比较

```go
func main() {
  //Out: true
  a := [2]int{1, 2}
  b := [2]int{1, 2}
  fmt.Println(a == b)
  
  //Out: false
  a := [2]int{1, 2}
  b := [2]int{1, 3}
  fmt.Println(a == b)
  
  //Out: worng!无法比较
  a := [2]int{1, 2}
  b := [1]int{1}
  fmt.Println(a == b)
}
```



##### new函数创建数组

```go
func main() {
  //返回指向数组的指针
  p := new([3]int)
  fmt.Println(p)
  
  //通过索引的方式对数组中某一元素赋值
  p[1] = 2
  fmt.Println(p)
}
```



##### 多维数组

```go
//Out: [[1 1 1] [2 2 2]]
func main() {
  a := [2][3]int {
    {1, 1, 1},
    {2, 2, 2}
  }
  fmt.Println(a)
}

//Out: [[0 1 0] [0 0 2]]
func main() {
  a := [2][3]int {
    {1: 1},
    {2: 2}
  }
  fmt.Println(a)
}

//Out: [[0 1 0] [0 0 2]]
func main() {
  a := [...][3]int {
    {1: 1},
    {2: 2}
  }
  fmt.Println(a)
}
```



##### 冒泡排序

```go
//冒泡排序
func main() {
	// 未排序数组
	sort := [...]int{1, 7, 4, 2, 5}
	fmt.Println(sort)

	// 冒泡排序，由大到小
	num := len(sort)
	for i := 0; i < num; i++ {
		for j := i + 1; j < num; j++ {
			// 比较大小
			if sort[i] < sort[j] {
				temp := sort[i]
				sort[i] = sort[j]
				sort[j] = temp
			}
		}
	}

	fmt.Println(sort)
}
```



### 切片

##### slice概述

##### 创建slice

```go
//1
//Out: []
func main() {
  a := []int
  fmt.Println(a)
}

//2
//Out: [5 6 7 8 9]
func main() {
  a := [10]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9} 
  s1 := a[5: 10]
  fmt.Println(s1)
}
func main() {
  a := [10]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9} 
  s1 := a[5: len(a)]
  fmt.Println(s1)
}
func main() {
  a := [10]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9} 
  s1 := a[5:]
  fmt.Println(s1)
}

//3
//Out: 3 10
func main() {
  s1 := make([]int, 3, 10)
  fmt.Println(len(s1), cap(s1))
}

//Out: 3 3
func main() {
  s1 := make([]int, 3)
  fmt.Println(len(s1), cap(s1))
}

```



##### reslice概述

```go
func main() {
  a := []byte{'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l'}
  sa := a[2: 5]
  //Out: 3 9
  fmt.Println(len(sa), cap(sa))
  
  //Out: 3 8
  sb := a[3: 5]
  fmt.Println(len(sb), cap(sb))
}
```



##### append()与slice

```go
func main() {
  s1 := make([]int, 3, 6)
  fmt.Println("%p\n", s1)
  
  //如果最终长度未超过追加到slice的容量则返回原始slice
  s1 = append(s1, 1, 2, 3)
  fmt.Println("%v %p\n", s1, s1)
  
  //如果超过追加到的slice的容量则将重新分配数组并拷贝原始数据
  s1 = append(s1, 4, 5, 6)
  fmt.Println("%v %p\n", s1, s1)
}
```



```go
//Out:
[3 4 5] [2 3]
[9 4 5] [2 9]
func main() {
  a := []int{1, 2, 3, 4, 5}
  s1 := a[2: 5]
  s2 := a[1: 3]
  fmt.Println(s1, s2)
  s1[0] = 9
  fmt.Println(s1, s2)
}

//Out:
[3 4 5] [2 3]
[9 4 5] [2 3 1 2 2 3 4 5 6]
func main() {
  a := []int{1, 2, 3, 4, 5}
  s1 := a[2: 5]
  s2 := a[1: 3]
  fmt.Println(s1, s2)
  s2 = append(s2, 1, 2, 2, 3, 4, 5, 6)
  s1[0] = 9
  fmt.Println(s1, s2)
}
```



##### copy与slice

```go
//Out: [7 8 9 4 5 6]
func main() {
  s1 := []int{1, 2, 3, 4, 5, 6}
  s2 := []int{7, 8, 9}
  copy(s1, s2)
  fmt.Println(s1)
}

//Out: [1 2 3]
func main() {
  s1 := []int{1, 2, 3, 4, 5, 6}
  s2 := []int{7, 8, 9}
  copy(s2, s1)
  fmt.Println(s2)
}

//Out: [7 8 2 3 1 1 1]
func main() {
  s1 := []int{1, 2, 3, 4, 5, 6}
  s2 := []int{7, 8, 9, 1, 1, 1, 1}
  copy(s2[2: 4], s1[1: 3])
  fmt.Println(s2)
}
```



###map

##### map概述

##### 简单map的创建与使用

```go
//初始化
//1
//Out: map[]
func main() {
  var m map[int]string
  m = map[int]string
  fmt.Println(m)
}

//2
//Out: map[]
func main() {
  var m map[int]string = make(map[int]string)
  fmt.Println(m)
}

//3
//Out: map[]
func main() {
  m := make(map[int]string)
  fmt.Println(m)
}
```



```go
//使用
//Out: map[1: OK] OK 

func main() {
  m := make(map[int]string)
  m[1] = "OK"
  fmt.Println(m)
  a := m[1]
  fmt.Println(a)
  
  delete(m, 1)
  fmt.Println(a)
}
```



#####复杂map与键值对操作

```go
func main() {
  var m map[int]map[int]string
  m = make(map[int]map[int]string)
  m[1] = make(map[int]string)
  m[1][1] = "OK"
  a := m[1][1]
  fmt.Println(a)
}
```



判断是否初始化

```go
//Out: GOOD true
func main() {
  var m map[int]map[int]string
  m = make(map[int]map[int]string)
  a, ok := m[2][1]
  if !ok {
    m[2] = make(map[int]string)
  }
  m[2][1] = "GOOD"
  a, ok = m[2][1]
  fmt.Println(a, ok)
}
```



##### map与slice的迭代操作

```go
func main() {
  sm := make([]map[int]string, 5)
  for i := range sm {
    sm[i] = make(map[int]string, 1)
    sm[i][1] = "OK"
    fmt.Println(sm[i])
  }
  fmt.Println(sm)
}
```



##### map的间接排序

```go
//Out:[1 2 3 4 5]
func main() {
  m := map[int]string{1: "a", 2: "b", 3: "c", 4: "d", 5: "e"}
  s := make([]int, len(m))
  i := 0
  for k, _ := range m {
    s[i] = k
    i++
  }
  sort.Ints(s)
  fmt.Println(s)
}
```



### 函数

##### 函数概述

##### 函数的定义与使用

```go
func A(a int, b string) (int, string) {}

func A(a, b, c int) int {}

//简写多个同类型返回值必命名
func A() (a, b, c int) {
  a, b, c = 1, 2, 3
  return
}

func A() (int, int, int) {
  a, b, c := 1, 2, 3
  return a, b, c
}
```



##### 不定长变参

```go
func main() {
  A(1, 2)
}

func A(a ...int) {
  fmt.Println(a)
}
```



##### 传递值类型和引用类型

```go
//传递值类型
//Out: [1 2] [1 2]
func main() {
  a, b := 1, 2
  A(a, b)
  fmt.Println(a, b)
}

func A(s ...int) {
  s[0] = 3
  s[1] = 4
  fmt.Println(s)
}


//引用类型
//Out: [3 4] [3 4]
func main() {
  s1 := []int{1, 2}
  A(s1)
  fmt.Println(s1)
}

func A(s []int) {
  s[0] = 3
  s[1] = 4
  fmt.Println(s1)
}

//
func main() {
  a := 1
  A(&a)
  fmt.Println(a)
}

func A(a *int) {
  *a = 2
  fmt.Println(a)
}
```







##### 匿名函数和闭包

```go
//函数类型的使用
//Out: Func A
func main() {
  a := A
  a()
}

func A() {
  fmt.Println("Func A")
}
```



```go
//匿名函数
//Out: Func A
func main() {
  a := func() {
    fmt.Println("Func A")
  }
  a()
}
```



```go
//闭包
func main() {
  f := closure(10)
  fmt.Println(f(1))
  fmt.Println(f(2))
}

func closure(x int) func(int) int {
  fmt.Println("%p\n", &x)
  return func(y int) int {
    fmt.Println("%p\n", &x)
    return x + y
  }
}
```



##### defer用法

```go
//Out: 2 1 0
func main() {
  for i := 0; i < 3; i++ {
    defer fmt.Println(i)
  }
}

//Out: 3 3 3 
func main() {
  for i := 0; i < 3; i++ {
    defer func() {
      fmt.Println(i)
    }()
  }
}
```



##### panic与recover

```go
func main() {
  A()
  B()
  C()
}

func A() {
  fmt.Println("Func A")
}

func B() {
  defer func() {
    if err := recover(); err != nil {
      fmt.Println("Recover in B")
    }
  }()
  panic("Panic in B")
}

func C() {
  fmt.Println("Func A")
}
```



### 结构

##### 结构的定义与使用

```go
//Out: {joe 19}
type person struct {
  Name string
  Age int
}

func main() {
  a := person{}
  a.Name = "Joe"
  a.Age = 19
  fmt.Println(a)
}
```



##### 使用字面值初始化

```go
//Out: {joe 19}
type person struct {
  Name string
  Age int
}

func main() {
  a := person{
    Name: "Joe"
    Age: 19
  }
  fmt.Println(a)
}

//Out:{joe 19}/ A {joe 13}/ {joe 19}
type person struct {
  Name string
  Age int
}

func main() {
  a := person{
    Name: "Joe"
    Age: 19
  }
  fmt.Println(a)
  A(a)
  fmt.Println(a)
}

func A(per person) {
  per.Age = 13
  fmt.Println("A", per)
}

//Out:{joe 19}/ A {joe 13}/ {joe 13}
type person struct {
  Name string
  Age int
}

func main() {
  a := person{
    Name: "Joe"
    Age: 19
  }
  fmt.Println(a)
  A(&a)
  fmt.Println(a)
}

func A(per *person) {
  per.Age = 13
  fmt.Println("A", per)
}


//最常用
//Out:{ok 19}/ A {ok 13}/ {ok 13}
type person struct {
  Name string
  Age int
}

func main() {
  a := &person{
    Name: "Joe"
    Age: 19
  }
  a.Name = "ok"
  fmt.Println(a)
  A(a)
  fmt.Println(a)
}

func A(per *person) {
  per.Age = 13
  fmt.Println("A", per)
}
```



##### 匿名结构与字段

```go
func main() {
  a := struct {
    Name string
    Age int
  }{
    Name: "Joe"
    Age: 19
  }
  fmt.Println(a)
}
```



```go
type person struct {
	string
  int
}

func main() {
  a := person("Joe", 19)
  fmt.Println(a)
}
```



```go
func main() {
  Name string
  Age int
  Contact struct {
    Phone, City string
  }
}

func main() {
  a := person{Name: "Joe", Age: 19}
  a.Contact.Phone = "1234567890"
  a.Contact.city = "beijing"
  fmt.Println(a)
}
```



##### 结构间的赋值与比较

```go
type person struct {
  Name string
  Age int 
}


func main() {
  a := person(Name: "joe", Age: 19)
  var b person
  b = a
  fmt.Println(b)
}
```



```go
//不同结构（结构名不同等）之间不可比较
//Out: false
type person struct {
  Name string
  Age int 
}


func main() {
  a := person(Name: "joe", Age: 19)
  b := person(Name: "joe", Age: 10)
  fmt.Println(a == b)
}
```



##### 嵌入结构

```go
type human struct {
  Sex int
}

type teacher struct {
  human 
  Name string
  Age int
}

type student struct {
  human 
  Name string
  Age int
}

func main() {
  a := teacher(Name: "Joe", Age: 19, human: human{Sex: 0})
  b := student{}
  b.Name = "Daniel"
  b.Age = 28
  //b.huma.Sex = 1
  b.Sex = 1
  fmt.Println(a. b);
}
```



### 方法

##### 方法的声明与使用

```go
type A struct {
  Name string
}

func main() {
  a := A{}
  a.Print()
}

func (a A) Print() {
  fmt.Println("A")
}
```



```go
//Out:A AA 
type A struct {
  Name string
}

func main() {
  a := A{}
  a.Print()
}

func (a *A) Print() {
  a.Name = "AA"
  fmt.Println("A")
}
```



##### 类型别名与方法

```go
//Out: TZ
type TZ int

func main() {
  var a TZ
  a.Print()
}

func (a *TZ) Print() {
  fmt.Print("TZ")
}
```



##### Method Vaule 与Method Expression

```go
type TZ int

func main() {
  //Method Vaule 
  var a TZ
  a.Print()
  
  //Method Expression
  (*TZ).Print(&a)
}

func (a *TZ) Print() {
  fmt.Print("TZ")
}
```



##### 方法名称冲突与字段访问权限



通过显示说明来实现receiver来实现与某个类型的组合

只能为同一包中的类型定义方法

Receiver可以是类型的值或者指针

不存在方法重载

可以使用值或指针来调用方法，编译器会自动完成转换

若果外部结构和嵌入结构存在同名方法，则优先调用外部结构方法

类型别名不会拥有底层类型的方法

方法可以调用结构中的非公开字段



###接口

interface

```go
//Homework

type int TZ

func (tz *TZ) Increase(num int) {
  *tz += TZ(num)
}

func main() {
  var a TZ
  a.Increase(100)
  fmt.Println(a)
}
```





接口只有方法声明，没有实现，没有数字字段

接口可以匿名嵌入其他接口，或嵌入到数据结构中

将对象赋值给接口时，会发生拷贝，而接口内部存储的是指向该复制品的指针，接口无法改变复制品的状态，也无法获取指针

只有当接口存储的类型和对象都为nil时，接口才等于nil

接口调用不会做receiver的自动转换

接口同样支持匿名字段方法

接口可以实现类似OOP的多态

空接口可以作为任何类型数据的容器



接口声明和使用

```go

```

PhoneConnecter

Connected:PhoneConnecter



接口嵌套

```go

```

Connected:PhoneConnecter

Disconnected.



接口调用实现其接口的类型中的属性

Ok pattern

```go

```

Connected:PhoneConnecter

Disconnected:PhoneConnecter



更简单的方法进行类型判断

type switch

```go

```

Connected:PhoneConnecter

Disconnected:PhoneConnecter



接口转换

```go

```

Connected:PhoneConnecter



```go
type USB interface {
  Name() string
  Connecter
}

type Connecter ineterface {
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

type TVConnecter struct {
  name string
}

//TV仅实现该方法
func (tv TVConnector) Connect() {
  fmt.Println("Connected:", tv.name)
}

func main() {
  tv := TVConnector{"TVConnecter"}
  var a USB
  a = USB(pc)
  a.Connect()
}
```

错误示范



复制问题

```go
type USB interface {
  Name() string
  Connecter
}

type Connecter ineterface {
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

func main() {
  pc := PhoneConnector{"PhoneConnecter"}
  var a Connecter
  a = Connecter(pc)
  a.Connect()
  
  pc.name = "pc"
  a.Connect()
}
```

Connected:PhoneConnecter

Connected:PhoneConnecter



空接口

```go
type USB interface {
  Name() string
  Connecter
}

type Connecter ineterface {
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

func main() {
  var a interface{}
  fmt.Println(a == nil)
  
  var p *int = nil
  a = p
  fmt.Println(a == nil)
}
```

true

false



#####接口的定义与基本操作

接口是一个或多个方法签名的集合

只要某个类型拥有该接口的所有方法签名，即算实现该接口，无需显示声明实现了哪个接口

```go
type USB interface {
  Name() string
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

func main() {
  var a USB
  a = PhoneConnector{"PhoneConnecter"}
  //前两句等同 a := PhoneConnector{"PhoneConnecter"}
  a.Connect()
  Disconnect(a)
}

//测试第二种方法是否真的实现USB interface
/*func Disconnect(usb USB) {
  fmt.Println("Disconnected.")
}*/
```



#####嵌入接口

```go
type USB interface {
  Name() string
  Connecter
}

type Connecter ineterface {
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

func main() {
  a := PhoneConnector{"PhoneConnecter"}
  a.Connect()
  Disconnect(a)
}

func Disconnect(usb USB) {
  fmt.Println("Disconnected.")
}
```



#####类型断言

```go
type USB interface {
  Name() string
  Connecter
}

type Connecter ineterface {
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

func main() {
  a := PhoneConnector{"PhoneConnecter"}
  a.Connect()
  Disconnect(a)
}

func Disconnect(usb USB) {
  if pc, ok := usb.(PhoneConnecter); ok {
    fmt.Println("Disconnected:", pc.name)
    return
  }
  fmt.Println("Unkown Devices.")
}
```



空接口与type switch

```go
type USB interface {
  Name() string
  Connecter
}

type Connecter ineterface {
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

func main() {
  a := PhoneConnector{"PhoneConnecter"}
  a.Connect()
  Disconnect(a)
}

func Disconnect(usb interface()) {
  switch v := usb.(type) {
    case PhoneConnecter:
    	fmt.Println("Disconnected:", v.name)
    default:
    	fmt.Println("Unkown Devices.")
  }
}
```



接口转换

```go
type USB interface {
  Name() string
  Connecter
}

type Connecter ineterface {
  Connect()
}

type PhoneConnecter struct {
  name string
}

func (pc PhoneConnector) Name() string {
  return pc.name
}

func (pc PhoneConnector) Connect() {
  fmt.Println("Connected:", pc.name)
}

func main() {
  pc := PhoneConnector{"PhoneConnecter"}
  var a Connecter
  a = Connecter(pc)
  a.Connect()
}
```



接口的使用注意事项



###反射

#####反射基本操作

```go
import {
  "fmt"
  "reflect"
}

type USE struct {
  Id int 
  Name string
  Age int
}

func (u User) Hello() {
  fmt.Println("Hello World.")
}

func main() {
  u := User{1, "OK", 12}
  Info(u)
}

func Info(o interface{}) {
  t := reflect.TypeOf(o)
  fmt.Println("Type:", t.Name())
  
  //判断传入的类型是否满足reflect.Struct
  if k := t.Kind(); k != reflect.Struct {
    fmt.Println("XX")
    return
  }
  
  v := reflect.ValueOf(o)
  fmt.Println("Fields:")
  
  for i := 0; i < t.NumField(); i ++ {
    f := t.Field(i)
    val := v.Field(i).Interface()
    fmt.Printf("%6s: %v = %v\n", f.Name, f.Type, val)
  }
  
  for i := 0; i < t.NumMethod; i ++ {
    m := t.Method()
    fmt.Printf("%6s: %v\n", m.Name, m.Type)
  }
}
```

Type: User

Fields:

​    Id: int = 1

  Name: string = OK

   Age: int = 12

​	Method: 

​	Hello  func (main, user)



####反射匿名字段或者嵌入字段

```go
type USE struct {
  Id int 
  Name string
  Age int
}

func Manager struct {
  User
  title string
}

func main() {
  m := Manager{User: User{1, "OK", 12}, title: "123"}
  t := reflect.TypeOf(m)
  
  fmt.Printf("%#v\n", t.FiledByIndex([]int{0, 1}))
}
```



#####修改目标对象

基本类型的操作

```go
func main() {
  x := 123
  v := reflect.ValueOf(&x)
  v.Elem().SetInt(999)
  
  fmt.Println(x)
}
```

999



```go
type USE struct {
  Id int 
  Name string
  Age int
}

func main() {
  u := User{1, "Ok", 12}
  set(&u)
  fmt.Println(u)
}

func Set(o interface{}) {
  v := reflect.ValueOf(o)
  
  if v.Kind() == refect.Ptr && !v.Elem().CanSet() {
    fmt.Println("XXX")
    return
  }else {
    v = v.Elem()
  }
  
  if f := v.FieldByName("Name"); f.Kind() == reflect.String {
    f.SetString("BYEBYE")
  } 
}
```



(1, "BYEBYE", 12)



判断是否真的找到字段

```go
type USE struct {
  Id int 
  Name string
  Age int
}

func main() {
  u := User{1, "Ok", 12}
  set(&u)
  fmt.Println(u)
}

func Set(o interface{}) {
  v := reflect.ValueOf(o)
  
  if v.Kind() == refect.Ptr && !v.Elem().CanSet() {
    fmt.Println("XXX")
    return
  }else {
    v = v.Elem()
  }
  
  //**
  f := v.FieldByName("Name")
  if !f.IsValid() {
    fmt.Println("BAD")
    return
  }
  
  if f.Kind() == reflect.String {
    f.SetString("BYEBYE")
  } 
}
```



#####动态调用方法

利用反射进行动态调用

```go
type USE struct {
  Id int 
  Name string
  Age int
}

func (u User) Hello(name string) {
  fmt.Println("Hello", name, ", my name is", u.Name)
}

func main() {
  u := User{1, "OK", 12}
  v := reflect.ValueOf(u)
  mv := v.MethodByName("Hello")
  
  args := []reflect.Value{reflect.ValueOf("Joe")}
  mv.Call(args)
}
```



反射通过ValeOf和TypeOf函数从接口中获取目标对象信息

反射会将匿名字段作为独立字段（匿名字段本质）

想要利用反射修改对象状态，前提是interface.data是settable，即pointer-interface

通过反射可以动态调用方法



### 并发

并发主要是由切换时间片来实现“同时”运行，并行是利用多核实现多线程的运行

Goroutine通过通信来共享内存，而不是通过共享内存来通信



##### Goroutine的使用

实现最基本的Goroutine

```go
import {
  "fmt"
  "time"
}

func main() {
  go Go()
  time.sleep(2 * time.Second)
}

func Go() {
  fmt.Println("GO GO GO!!!")
}
```

GO GO GO!!!



#####Channel概述

Channel

大都是阻塞同步

通过make创建，close关闭

引用类型

使用for range来迭代操作channel

可设置单向或双向通道

可设置缓存大小，在未被填满前不会阻塞



Select

可处理一个或多个channel的发送和接收

同时有多个可用的channel时按随机顺序处理

可用空的select来阻塞main函数

可是超时



基本操作

```go
func main() {
  c := make(chan bool)
  go func() {
    fmt.Println("Go GO GO !!!")
    c<-true
  }()
  <-c
}
```

Go GO GO !!!



##### 多个Goroutine打印

迭代操作

```go
func main() {
  c := make(chan bool)
  go func() {
    fmt.Println("Go GO GO !!!")
    c<-true
    //明确关闭
    close(c)
  }()
  for v := range c {
    fmt.Println(v)
  }
}
```



无缓存先取后存 有缓存随意

```go
func main() {
  c := make(chan bool, 1)
  go func() {
    fmt.Println("Go GO GO !!!")
    <-c
  }()
  c<-true
}
```

Go GO GO !!!



```go
func main() {
  c := make(chan bool)
  for i := 0; i < 10; i ++ {
    go Go(c, i)
  }
  <-c
}

func Go(c chan bool, index int) {
  a := 1
  for i := 0; i < 10000000; i ++ {
    a += i
  }
  fmt.Println(index, a)
  
  if index == 9 {
    c <- true
  }
}
```



```go
import {
  "fmt"
  "runtime"
}

func main() {
  runtime.GOMAXPROCS(runtime.NumCPU())
  c := make(chan bool, 10)
  for i := 0; i < 10; i ++ {
    go Go(c, i)
  }
  
  for i := 0; i < 10; i ++ {
    <-c
  }
}

func Go(c chan bool, index int) {
  a := 1
  for i := 0; i < 10000000; i ++ {
    a += i
  }
  fmt.Println(index, a)
  
  c<-true
}
```



```go
import {
  "fmt"
  "runtime"
  "sync"
}

func main() {
  runtime.GOMAXPROCS(runtime.NumCPU())
  wg := sync.WaitGroup{}
  wg.Add(10)
  for i := 0; i < 10; i ++ {
    go Go(&wg, i)
  }
  
  wg.Wait()
}

func Go(wg *sync.WaitGroup, index int) {
  a := 1
  for i := 0; i < 10000000; i ++ {
    a += i
  }
  fmt.Println(index, a)
  
  wg.Done()
}
```



##### selsect概述

```go
func main() {
  c1, c2 = make(chan int), make(chan string)
  o := make(chan bool)
  go func() {
    for {
      select {
        case v, ok := <-c1:
          if !ok {
            o <- true
            break
          }
          fmt.Println("c1", v)
        case v, ok := <-c2:
        	if !ok {
            o <- true
            break
          }
        	fmt.Println("c2", v)
      }
    }
  }()
  
  c1 <- 1
  c2 <- "hi"
  c1 <- 3
  c2 <- "hello"
  
  close(c1)
  close(c2)
  
  <-o
}
```

c1 1

c2 hi

c1 3

c2 hello



错误写法

```go
func main() {
  c1, c2 = make(chan int), make(chan string)
  o := make(chan bool, 2)
  go func() {
    a, b := false, false
    for {
      select {
        case v, ok := <-c1:
          if !ok {
            if !a {
              a = true
              o <- true
            }
            break
          }
          fmt.Println("c1", v)
        case v, ok := <-c2:
        	if !ok {
            if !b {
              b = true
              o <- true
            }
            break
          }
        	fmt.Println("c2", v)
      }
    }
  }()
  
  c1 <- 1
  c2 <- "hi"
  c1 <- 3
  c2 <- "hello"
  
  close(c1)
  close(c2)
  
  for i := 0; i < 2; i ++ {
    <-o
  }
}
```



```go
func main() {
  c := make(chan int)
  go func() {
    for v := range c {
      fmt.Println(v)
    }
  }()
  
  for {
    select {
      case c <- 0:
      case c <- 1:
    }
  }
}
```



```go
import {
  "fmt"
  "time"
}

func main() {
  c := make(chan bool)
  select {
    case v := <-c:
   		fmt.Println(v)
    case <-time.After(3 * time.second):
    	fmt.Println("Timeout.")
  }
}
```

Timeout.



### 补充

#####递增递减语句

在Go当中，++ 与 -- 是作为语句而并不是作为表达式