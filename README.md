# Golang学习笔记

## 环境搭建
#### 安装Go开发包
下载对应操作系统的安装包
[官方镜像站](https://golang.google.cn/doc/install?download=go1.14.1.windows-amd64.msi)

#### 安装
Next到底，直到finish。
打开cmd，执行 go version 命令成功打印版本号即安装成功。

#### 设置开发环境
 - 不需要配置环境变量，安装时已经自动配置好了，这里配置的是开发环境
 - 此电脑 → 属性 → 高级系统设置 → 环境变量 → 找到GOPATH变量，编辑到指定目录OK

#### 安装编辑器，推荐VScode
 - 下载  [VScode](https://code.visualstudio.com/Download)
 - 安装，傻瓜式操作
 - 左侧栏搜索并install （Chinese） 插件，设置VScode中文开发环境
 - 左侧栏搜索并install （Go） 扩展

以上程序都安装完成后，恭喜，Go语言开发环境已经搭建完成。

## 第一个Go程序
 - 启动VScode → 文件 → 打开文件夹（开始的时候设置的开发环境文件夹）
 - 然后新建文件即可书写代码了
```go
package main  // 声明文件属于哪个包，工具包/main包为可执行文件

// 导入语句，包
import "fmt"

// 程序入口函数，main()无参数、无返回值
func main() {
	fmt.Println("Hello Go!")
}
```

#### 编译运行
 - 命令：go bulid [ -o 输出文件名字]
1. 在项目目录下通过终端执行命令即可
2. win32会生成.exe文件
3. 在cmd中可直接执行.exe文件

执行后在终端会打印：Hello Go!

#### 附加命令
 - go run HelloGo.go 执行两个操作，先生成.exe文件，再运行
 - go install HelloGo.go 执行三个步骤，前两步同上，第三步将生成文件放到GOPATH下的bin目录中

#### 跨平台编译/交叉编译
只需要指定目标操作系统的平台和处理器架构即可：
 - windows下编译Linux和Mac平台64位可执行程序：
0. SET CGO_ENABLED=0  // 禁用CGO
1. SET GOOS=linux  // 目标平台
2. SET GOARCH=amd64  // 目标处理器框架
3. go build

 - Mac下编译Linux和Windows平台64位可执行程序：
0. CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build
1. CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build

 - Linux下编译Mac和Windows平台64位可执行程序：
0. CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build
1. CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build

## 常量、变量
#### 声明变量
var 变量名 变量类型
```go
var name string
var age int
var isOK bool
```
示例：
```go
package main

import "fmt"

// 声明变量
// Go语言中推荐使用驼峰式命名
var studentName string  // 推荐
var student_name string // 不推荐，这样命名编译器会警告
var StudentName string  // 不推荐，这样命名编译器会警告

// Go语言中声明的变量必须使用，否则编译不通过
var (
	name string
	age  int
	isOK bool
)

func main() {
	fmt.Println("Let's go!") // 自动加换行符
	// 赋值
	name = "GOOOOOOOO"
	age = 16
	isOK = true
	fmt.Print(isOK)                           // 在终端打印完成即结束，无其余工作
	fmt.Printf("Name:%s Age:%d\n", name, age) // 可以使用占位符

	// 声明变量同时赋值
	var s1 string = "zhangsan"
	fmt.Println(s1)
	// 自动类型推导，使用初始化赋值时无需指定类型
	var s2 = "lisi"
	fmt.Println(s2)
	// 简短变量声明 :=
	s3 := "wangwu" // 只能在函数里面使用
	fmt.Println(s3)
}
```
 - 匿名变量
0. 匿名变量相当于 Lua脚本中的哑元变量
1. 它不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明
2. 作用于 保留未赋值的变量、接收函数返回值中我们不需要的部分（Go函数允许有多个返回值）
```go
package main

import "fmt"

func foo() (int, string) {
	return 10, "Let's go!"
}

func main() {
	_, b := foo()
	a, _ := foo()
	fmt.Println(a, b)
}
```
 - 注意：
0. 函数外的每个语句都必须以关键字开始（var、const、func）
1. := 不能使用于函数外
2. _多用于占位，表示忽略值

#### 常量
相对于变量，常量是恒定不变得量，多用于定义程序运行期间不会改变的那些值，常量的声明和变量的声明非常类似，只是把`var`换成了`const`，常量在定义的时候必须赋值。
```go
const pi = 3.1415926
const e = 2.7182

const (
    pi = 3.1415926
    e = 2.7182
)

// const同时声明多个常量时，如果省略了值则表示和上面一行的值相同
const (
    n1 = 100
    n2
    n3
)
```

#### iota
`iota` 常量计数器，只能在常量表达式中使用  
`iota` 在const关键字出现时将被重置为0，const中每新增一行常量声明将使`iota`计数一次  
使用`iota`能简化定义，在定义枚举时很有用
```go
const (
    n1 = iota  // const关键字出现时置0
    n2
    _
    n3
)  // n1 = 0  n2 = 1  n3 = 3

// iota的值置于const行数有关，而不是const变量个数
const (
    m1, m2 = iota + 1, iota + 2  // 1 2
    m3, m4 = iota + 3, iota + 4  // 4 5
)
```
 - 定义数量级
```go
const (
    _ = iota
    KB = 1 << (10 * iota)  // 1左移10位，即为2的10次方，1024
    MB = 1 << (10 * iota)
    BG = 1 << (10 * iota)
    TB = 1 << (10 * iota)
    PB = 1 << (10 * iota)
)
```

## 基本数据类型
 - 整型  
|  类型   | 描述  |
|  ----  |  ----  |
| uint8  | 无符号8位整型（0到255） |
| uint16  | 无符号16位整型（0到65535） |
| uint32  | 无符号32位整型（0到4294967295） |
| uint64  | 无符号64位整型（0到18446744073709551615） |
| int8  | 符号8位整型（-128到127） |
| int16  | 符号16位整型（-32768到32767） |
| int32  | 符号32位整型（-2147483648到2147483647） |
| int64  | 符号64位整型（-9223372036854775808到9223372036854775807） |
 - 特殊整型
|  类型   | 描述  |
|  ----  |  ----  |
|  byte  |  uint8 别名（一个英文字符就是byte类型）  |
|  rune  |  int32 别名（一个其它字符，例如中文字、韩文字等等） |
|  uint  |  32位操作系统表示uint32，64位表示uint64  |
|  int  |  32位操作系统表示int32，64位表示int64  |
|  uintptr  |  无符号整型，用于存放一个指针  |
注意：需要考虑`int`和`uint`可能在不同平台上的差异  
 - 浮点型
|  类型  |  描述  |
|  ----  |  ----  |
|  float32  |  IEEE-754 32位浮点型数 最大范围约为3.4e38 常量定义math.MaxFloat32  |
|  float64  |  IEEE-754 64位浮点型数 最大范围约为1.8e308 常量定义math.MaxFloat64  |
|  complex64  |  32位实数和虚数  |
|  complex128  |  64位实数和虚数  |
```go
package main

import "fmt"

func main() {
	c1 := 'こ'
	c2 := '哦'
	var c3 byte = 'x'
	fmt.Printf("c1:%T c2:%T c3:%T\n", c1, c2, c3)  // int32 int32 uint8

	f1 := 1.23456
	fmt.Printf("%T \n", f1)  // float64，Go语言中默认小数为float64
	f2 := float32(1.23456)
	fmt.Printf("%T \n", f1)  // float32
	// 不能进行赋值：f1 = f2
}
```

 - 布尔值  
只有两个值，`true`和`false`  
注意：  
0. 布尔类型变量默认值为false
1. Go语言中不允许将整型转换为布尔型
2. 布尔型无法参与数值运算，也无法与其它类型进行转换
 - 八进制&十进制数  
Go语言中无法直接定义二进制数，但可以直接定义八进制和十六进制数：
```go
package main

import "fmt"

func main() {
	// 十进制
	a := int8(10)
	fmt.Printf("%d \n", a)  // 10
	fmt.Printf("%b \n", a)  // 1010，占位符 %b 表示二进制
	
	// 八进制，以0开头
	var b int = 077
	fmt.Printf("%o \n", b)  // 77
	
	// 十六进制，以0x开头
	var c int = 0xff
	fmt.Printf("%x \n", c)  // ff
	fmt.Printf("%X \n", c)  // FF
}
```

 - fmt 占位符
```go
 package main

import "fmt"

func main() {
	n := 100
	fmt.Printf("%T \n", n)  // 查看变量的类型
	fmt.Printf("%v \n", n)  // 查看变量的值
	fmt.Printf("%b \n", n)  // 查看变量转为二进制的值
	fmt.Printf("%d \n", n)  // 查看变量的十进制值
	fmt.Printf("%o \n", n)  // 查看变量的八进制值
	fmt.Printf("%x \n", n)  // 查看变量的十六进制值
	fmt.Printf("%X \n", n)  // 查看变量的十六进制值，大写显示
	
	s := "Hello Go!"
	fmt.Printf("%s \n", s)  // 查看字符串的值
	fmt.Printf("%v \n", s)  // 查看字符串的值
	fmt.Printf("#%v \n", s)  // 查看字符串的值并加上 " " 符号
}
```

## 字符串
0. Go语言中的字符串类型是原生数据，就像（int bool float32 float64）一样
1. 字符串内部使用`UTF-8`编码，直接支持中文
2. 字符串的值（只能）用`""`中内容表示，`''`包裹的是字符
3. 原生字符串用反应号：``
4. 可以在Go语言的源码中直接添加非ASCII码字符，例如：
```go
s1 := "Hello Go!"
s2 := "你好，Go！"
s3 := `！@#￥%……&*（）——!@#$%^&*()/\\\n`  // 原生字符串
```

 - Go语言的字符串常见转义符包括回车、换行、单双引号、制表符，如下表所示：
|  转义符  |  含义  |
|  ----  |  ----  |
|  \r  |  回车符（返回行首）  |
|  \n  |  换行符（直接跳到下一行的同列位置）  |
|  \t  |  制表符  |
|  \\'  |  单引号  |
|  \\"  |  双引号  |
|  \\  |  反斜杠  |

 - 字符串的常用操作
|  方法  |  介绍  |
|  ----  |  ----  |
|  len(str)  |  求长度  |
|  +或fmt.Sprintf  |  拼接字符串  |
|  strings.Split  |  分割  |
|  strings.contains  |  判断是否包含  |
|  strings.HasPrefix, strings.HasSuffix  |  前缀/后缀判断  |
|  strings.Index(), strings.LastIndex()  |  子串出现的位置  |
|  strings.Join(a[]string, sep string)  |  join操作  |
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	name := "Goooo"
	nicheng := "haozi"

	// 拼接
	he := name + nicheng
	fmt.Println(he)
	hee := fmt.Sprintf("%s%s", name, nicheng)
	fmt.Println(hee)

	s1 := "123|789|456|789|abc|wan|789"

	// 分割
	ret := strings.Split(s1, "|") // [123 789 456 789 abc wan 789]
	fmt.Println(ret)

	// 包含
	fmt.Println("s1是否包含789：", strings.Contains(s1, "789"))
	fmt.Println("s1是否包含aaa：", strings.Contains(s1, "aaa"))

	// 前/后缀
	fmt.Println("s1前缀是否为123：", strings.HasPrefix(s1, "123"))
	fmt.Println("s1后缀是否为123：", strings.HasSuffix(s1, "123"))

	// 子串
	fmt.Println("子串“789”出现的位置", strings.Index(s1, "789")) // 4
	fmt.Println("子串“789”最后出现的位置", strings.LastIndex(s1, "789"))

	// join操作-拼接
	fmt.Println(strings.Join(ret, "+")) // 123+789+456+789+abc+wan+789
	fmt.Println(strings.Join(ret, "|")) // 123|789|456|789|abc|wan|789
}
```

## byte和rune类型
Go语言的字符有两种：  
0. `uint8`类型，也叫做`byte`型，代表了ASCII码的一个字符
1. `rune`类型，int32别名，代表一个`UTF-8`字符  
组成每个字符串的元素叫做“字符”，可以通过遍历或者单个获取字符串元素获得字符  
字符用 (' ') 包裹起来，如：  
```go
var a := '绍'
var b := 'x'
```

Go使用了特殊的`rune`类型来处理Unicode，让基于Unicode的文本处理更为方便，也可以使用`byte`型进行默认字符串处理，既有性能，也有扩展性。  
#### 修改字符串
Go语言中`字符串不可以修改`，需要先将其转换为`[]rune`或`[]byte`，完成后再转换为string，无论哪种转换，都会重新分配内存，并复制字节数组。
```go
package main

import "fmt"

func main() {
	s1 := "Let's go!"
	// 强制类型转换为[]byte
	byteS1 := []byte(s1)
	byteS1[0] = 'P'
	fmt.Println(string(byteS1))

	s2 := "こんにちは"
	// 强制类型转换为切片[]rune
	runeS2 := []rune(s2)
	runeS2[0] = '你'
	fmt.Println(string(runeS2))
}
```

## 类型转换
 - Go语言中只有强制类型转换，没有隐式类型转换
 - 只能在两个类型之间支持相互转换的时候才能使用
 - bool 不支持与任意类型进行强制类型转换
总结：一般只有`字符串与[]byte、[]rune`、`数值类型`之间能够相互强制类型转换
#### 附加
##### int -> string
 0. str := strconv.Itoa(intVal)
 1. fmt.Sprintf("%s", n) 取返回值
 2. strconv.FormatInt
##### string -> int
intVal, err := strconv.Atoi(str)

##### string -> int64
int64Val, err := strconv.ParseInt(str, 10, 64)  // 10表示十进制，64表示转换为int64类型

##### int64 -> string
str := strconv.FormatInt(int64Val, 10)

##### float -> string
v := 3.1415926535  
s1 := strconv.FormatFloat(v, 'E', -1, 32)  // float32  
s2 := strconv.FormatFloat(v, 'E', -1, 64)  // float64  
第二个参数可选 'f' 'e' 'E' 等，含义如下：  
'b' (-ddddp±ddd，二进制指数)  
'e' (-d.dddde±dd，十进制指数)  
'E' (-d.ddddE±dd，十进制指数)  
'f' (-ddd.dddd，没有指数)  
'g' ('e'：大指数，'f'：其它情况)  
'G' ('E'：大指数，'f'：其它情况)

##### string -> float
s := "3.1415926"
v1, err := strconv.ParseFloat(s, 32)
v1, err := strconv.ParseFloat(s, 64)
##### int、float等数值类型 之间运用强制类型转换
```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// 数值类型之间的转换
	n := 10
	var f float64
	f = float64(n)
	fmt.Printf("f: %f %T\n", f, f) //10.000000 float64

	// int -> string
	n1 := 10
	str := strconv.Itoa(n1)
	fmt.Printf("str: %s %T\n", str, str)

	str1 := fmt.Sprintf("%s", n1) // 警告
	fmt.Printf("str1: %s %T\n", str1, str1)

	str2 := strconv.FormatInt(int64(n1), 10)
	fmt.Printf("str2: %s %T\n", str2, str2)

	// string -> int
	n2, err := strconv.Atoi(str)
	fmt.Printf("n2: %d %T\n", n2, n2)
	fmt.Printf("err: %d %T\n", err, err)

	// float -> string
	n3 := 3.1415926535
	str3 := strconv.FormatFloat(n3, 'E', -1, 32)
	fmt.Printf("s1: %s %T\n", str3, str3)

	// string -> float
	str4 := "3.1415926"
	n4, err1 := strconv.ParseFloat(str4, 32)
	fmt.Printf("n4: %.2f %T\n", n4, n4)
	fmt.Printf("err1: %d %T\n", err1, err1)
}
```

## 流程控制

#### 总述
 - if 条件控制
 - Go语言中只有一种循环结构，即`for`
 - for循环的基本格式如下：
```go
for 初始语句; 条件表达式; 结束语句 (
	循环体语句
)
```
条件表达式返回`true`时循环体执行，直到条件表达式返回`false`时自动退出循环

#### 无线循环
```go
for {
	循环体语句
}
```
for循环可以通过 `break`、`goto`、`return`、`panic` 语句强制退出循环

#### for range（键值循环）
Go语言中使用`for range`遍历数组、切片、字符串、map、通道（channel），通过`for range`遍历的返回值有以下规律：  
0. 数组、切片、字符串 返回索引和值
1. map 返回键和值
2. 通道（channel）只返回通道内的值

#### switch case
使用switch-case语句可以方便地对大量的值进行条件判断
```go
package main

import "fmt"

func main() {
	age := 19

	if age < 18 { // if age := 19; age < 18 作用域不一样，这里的age是局部变量
		fmt.Println("未成年！")
	} else if age == 18 {
		fmt.Println("步入成年行列！")
	} else {
		fmt.Println("已经成年！")
	}

	var sum int = 0
	for i := 0; i < 100; i++ {
		sum += i
	}
	fmt.Println("sum = ", sum)

	// 无限循环
	/*for {
		fmt.Println("我正在思考 ......")
	}*/

	// for range
	s := "Let's go! 冲啊"
	for i, v := range s {
		fmt.Printf("index = %d value = %c\n", i, v)
	}

	// 跳出循环
	for i := 0; ; i++ {
		if i < 5 {
			continue
		} else {
			break
		}
	}

	// switch-case
	// 在go语言的switch-case语句中，不需要使用break
	var n = 5
	switch n {
	case 1, 3, 5, 7, 9: // 这里可以是多个值，也可以是单个值或条件语句
		fmt.Println("奇数")
	case 2, 4, 6, 8, 10:
		fmt.Println("偶数")
	default:
		fmt.Println("不在计算范围中")
	}

	// fallthrough 以后不鼓励写
	var s1 string = "a"
	switch {
	case s1 == "a":
		fmt.Println("a")
		fallthrough // 该关键字是为了兼容C语言的某一些特性，以后不写
	case s1 == "b":
		fmt.Println("b")
		fallthrough
	case s1 == "c":
		fmt.Println("c")
	default:
		fmt.Println("...")
	} // 此语句块结果为打印 a b

	// goto 语句  不建议使用，尽量少用
	for j := 0; ; j++ {
		if j == 10 {
			goto end // 会跳到 end: 标签处，即下方
		}
	}
end: // break 和 continue 也可以打标签
	fmt.Println("over!")
}
```

## 运算符
Go语言内置的运算符有五种：
0. 算术运算符
1. 关系运算符
2. 逻辑运算符
3. 位运算符
4. 赋值运算符

#### 算术运算符
|  运算符  |  描述  |
|  ----  |  ----  |
|  + - * /  |  加 减 乘 除  |
|  %  |  求余  |
注意：`++`（自增）和`--`（自减）在Go语言中是独立的语句，并不是运算符

#### 关系运算符
|  运算符  |  描述  |
|  ----  |  ----  |
|  ==  |  检查两个值是否相等，如果成立返回true，否则返回false  |
|  !=  |  检查两个值是否不相等，如果成立返回true，否则返回false  |
|  >  |  检查左边的值是否大于右边的值，如果成立返回true，否则返回false  |
|  >=  |  检查左边的值是否大于等于右边的值，如果成立返回true，否则返回false  |
|  <  |  检查左边的值是否小于右边的值，如果成立返回true，否则返回false  |
|  <=  |  检查左边的值是否小于等于右边的值，如果成立返回true，否则返回false  |

#### 逻辑运算符
|  运算符  |  描述  |
|  ----  |  ----  |
|  &&  |  逻辑AND运算符，如果两边的操作数都是true，则为true，否则为false  |
|  ||  |逻辑OR运算符，如果两边的操作数中有一个为true，则为true，否则为false  |
|  !  |  逻辑NOT运算符，如果条件为true，则结果为false，否则为true|

#### 位运算符
|  运算符  |  描述  |
|  ----  |  ----  |
|  &  |  参与运算的两个数各对应的二进制位相与（两位均为1则为1）|
|  |  |  参与运算的两个数各对应的二进制位相或（两位有一位为1则为1）|
|  ^  |  参与运算的两个数各对应的二进制位相异或（两位不一样则为1）|
|  <<  |  左移n位，就是乘以2的n次方  |
|  >>  |  右移n位，就是除以2的n次方  |

#### 赋值运算符
|  运算符  |  描述  |
|  ----  |  ----  |
|  =  |  将一个右值或表达式赋给一个左值  |
|  +=  |  相加后再赋值  |
|  -=  |  相减后再赋值  |
|  \*=  |  相乘后再赋值  |
|  /=  |  相除后再赋值  |
|  %=  |  相余后再赋值  |
|  <<=  |  左移后再赋值  |
|  >>=  |  右移后再赋值  |
|  &=  |  按位与后再赋值  |
|  |=  |  按位或后再赋值  |
|  ^=  |  按位异或后再赋值  |

## 数组
数组是同一数据类型元素的集合，在Go语言中，数组从声明时就确定，使用时可以修改数组成员，但是数组大小不可以变化。

  - 基本语法
var 变量名 [长度]数据类型  
var b1 [4]bool、var b2 [5]bool 是不同的类型

 - 初始化
如果不进行初始化，默认元素都是零值（布尔值：false、整型和浮点型：0、字符串：""）
```go
b := [3]bool { true, true, true }
a := [...]int { 0, 1, 2, 3, 4, 5 } // ... 根据初始值自动推断长度
c := [5]int {0: 1, 1: 2} // 表示按照下标赋值c[0] = 1， c[1] = 2
```

 - 遍历
```go
citys := [...]string { "重庆", "北京", "上海", "深圳" }

for i := 0; i < len(citys); i++ {
	fmt.Println(city[i])
}

for i, v := range citys {
	fmt.Println(i, v)
}
```

 - 多维数组
```go
package main

import "fmt"

func main() {

	// 定义与赋值
	var a [3][2]int
	a = [3][2]int{
		[2]int{1, 2},
		[2]int{3, 4},
		[2]int{5, 6},
	}
	fmt.Println(a)

	b := [3][2]int{{1, 2}, {3, 4}, {5, 6}}
	fmt.Println(b)

	c := b
	fmt.Println(c)

	// 遍历
	for i, v := range b {
		fmt.Println(i, v)
	}

	for _, v1 := range b {
		fmt.Println(v1)
		for _, v2 := range v1 {
			fmt.Println(v2)
		}
	}
}
```

#### 附加
0. 数组是值类型
1. 数组支持 "=="、"!=" 操作符，因为内存总是被初始化过的
2. [n]\*T表示指针数组，\*[n]T表示数组指针  // T为数据类型  
因为数组的长度是固定的并且数组长度属于类型的一部分，所以数组有很多局限性。例如：
```go
func arraySum(x [3]int) int {
	sum := 0
	for _, v := range x {
		sum += v
	}
	return sum
}
// 这个求和函数只能接受`[3]int`类型，其它的都不支持，再比如：
a := [3]int{1, 2, 3}
// 数组a中已经有三个元素了，我们不能继续往数组a中添加新元素了。
```

## 切片
0. `切片（Slice）`是一个拥有相同类型元素可变长度的序列
1. 它是基于数组类型做的一层封装
2. 它非常灵活，支持自动扩容
3. 切片一般用于快速地操作一块数据集合
 - 切片是一个`引用`类型，它的内部结构包含`地址`、`长度`和`容量`

#### 定义
 - 声明
```go
var name []T
```
其中，name 表示变量名，T 表示切片中的元素类型

#### 切片的长度和容量
长度：内置的len()函数  
容量：内置的cap()函数  
注：切片的容量是直接初始化即为初始化的长度，否则容量是底层数组从切片的第一个元素到最后的容量值（切数组的时候）

#### 使用make()函数构造切片
上面都是基于数组来创建的切片，如果需要动态的创建一个切片，我们就需要使用内置的`make()`函数，格式如下：
```
make([]T, size, cap))
```
其中，T为切片的元素类型、size为切片中元素的数量、cap为切片的容量，若不指定cap，默认cap = size

#### 切片的本质
0. 切片就是一块连续的内存
1. 切片属于引用类型，真正的数据都是保存在底层数组里的

#### 切片不能直接比较
0. 切片直接不能比较，不能使用`==`操作符来判断两个切片是否含有全部相等元素
1. 切片唯一合法的比较操作是和`nil`比较
2. 一个`nil`值得切片是没有底层数组的，而且长度和容量都是0，但不能说一个长度和容量都是0的切片一定是`nil`
3. 判断一个切片是否为空，应该用`len(s) == 0`，而不是`s == nil`
```go
var s0 []int // len(s0) = 0; cap(s0) = 0; s0 == nil
s1 := []int{} // len(s1) = 0; cap(s1) = 0; s1 != nil
s2 := make([]int, 0) // len(s2) = 0; cap(s2) = 0; s2 != nil
s2 := make([]int, 5, 10) // len(s2) = 5; cap(s2) = 10; s2 != nil
```

#### 切片的赋值拷贝
下面代码中演示了拷贝前后两个变量共享底层数组，对一个切片的修改会影响另一个切片的内容，这点需要特别注意。

#### 遍历切片
方法同数组

#### append() 为切片追加元素
0. Go语言的内建函数`append()`可以为切片动态添加元素
1. 每个切片会指向一个底层数组，这个数组能容纳一定数量的元素，当底层数组不能容纳新增的元素时，切片就会自动按照一定的策略对齐进行“扩容”，切片指向的底层数组就会更换
2. “扩容”操作往往发生在调用`append()`函数时
##### append() 附加重要属性
0. 如果新申请的容量（cap）大于旧容量（old.cap）的2倍，最终容量（newcap）就是新申请的容量（cap）
1. 如果旧切片长度小于1024，则最终容量（newcap）增加至旧容量（old.cap）的两倍，即 newcap = 2 * old.cap
2. 如果旧切片长度大于等于1024，则最终容量（newcap）从旧容量（old.cap）开始循环增加原来的1/4，即（newcap = old.cap, for { newcap += newcap / 4 }）直到最终容量（newcap）大于等于新申请的容量（cap），即 newcap >= cap
3. 如果最终容量（cap）计算值溢出，则最终容量（cap）就是新申请容量（cap）  
##### 源码
可以通过查看 `$GOROOT/src/runtime/slice.go` 源码，其中扩容相关代码如下：
```go
newcap := old.cap
doublecap := newcap + newcap
if cap > doublecap {
	newcap = cap
} else {
	if old.len < 1024 {
		newcap = doublecap
	} else {
		// Check 0 < newcap to detect overflow
		// and prevent an infinite loop.
		for 0 < newcap && newcap < cap {
			newcap += newcap / 4
		}
		// Set newcap to the requested cap when
		// the newcap calculation overflowed.
		if newcap <= 0 {
			newcap = cap
		}
	}
}
```
注：切片扩容还会根据切片中元素的类型不同而做不同的处理，比如`int`和`string`类型的处理方式就不一样

#### 使用 copy()函数 复制切片
```go
a := []int{1, 2, 3, 4, 5}
b := a
fmt.Println(a) // [1 2 3 4 5]
fmt.Println(b) // [1 2 3 4 5]
b[0] = 100
fmt.Println(a) // [100 2 3 4 5]
fmt.Println(b) // [100 2 3 4 5]
```
0. 由于切片是引用类型，所以a和b其实都指向了同一块内存地址，修改b的同时a的值也会发生变化
1. Go语言内建函数`copy()`可以迅速地将一个切片的数据复制到另一个切片空间中，`copy()`函数的使用格式如下：
```go
copy(destSlice, srcSlice []T)
```
其中：srcSlice是数据来源切片、destSlice是目标切片

#### 从切片中删除元素
Go语言中没有删除切片元素的专用方法，但我们可以使用切片本身的特性来删除元素
```go
a := []int{1, 2, 3, 4, 5}
// 删除索引为2的元素
a = append(a[:2], a[3:]...)
fmt.Println(a) // [1, 2, 4, 5]
```
总结：从切片a中删除索引为index的元素，操作方法就是`a = append(a[:index], a[index+1:]...)`

#### 对切片进行排序
```go
var a = [...]int{7, 8, 6, 3, 1, 5}
sort.Ints(a[:])
fmt.Println(a)
```

#### 陷阱
```go
var x = make([]int, 5, 10) // 创建切片，长度为5，容量为10
	fmt.Println(x)
	for i := 0; i < 10; i++ {
		x = append(x, i)
	}
	fmt.Println(x) // [0 0 0 0 0 1 2 3 4 5 6 7 8 9] 且容量变为20
```

#### 总结
0. 切片不保存具体的值
1. 切片对应一个底层数组
2. 底层数组都是占用一块连续的内存
```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	// 定义一个存放int类型的切片
	var s1 []int
	var s2 []string
	fmt.Printf("%T %T\n", s1, s2)
	fmt.Println(s1, s2)    // [] [] 空在go相当于没有开辟内存空间
	fmt.Println(s1 == nil) // true
	fmt.Println(s2 == nil) // true

	// 初始化
	s1 = []int{1, 2, 3}
	s2 = []string{"Let's go!", "I love you", "我爱你"}
	fmt.Println(s1, s2)

	// 长度和容量
	fmt.Printf("len(s1): %d cap(s1): %d\n", len(s1), cap(s1))
	fmt.Printf("len(s2): %d cap(s2): %d\n", len(s2), cap(s2))

	// 由数组得到切片，求长度和容量
	a := [...]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	s3 := a[0:4]    // [1, 2, 3, 4] 基于数组切割，左闭右开（左包含，右不包含）
	fmt.Println(s3) // [1 2 3 4]
	fmt.Printf("len(s3): %d cap(s3): %d\n", len(s3), cap(s3)) // 4 10
	a[0] = 10
	a[1] = 20
	a[2] = 30
	a[9] = 100
	s4 := a[1:6]
	fmt.Println(s4) // [20 30 4 5 6]
	fmt.Printf("len(s4): %d cap(s4): %d\n", len(s4), cap(s4)) // 5 9
	s5 := a[:5] // 从开始到下标为5-1处
	fmt.Println(s5)
	fmt.Printf("len(s5): %d cap(s5): %d\n", len(s5), cap(s5)) // 5 10
	s6 := a[3:]    // 从下标为3处到结尾
	fmt.Println(s6)
	fmt.Printf("len(s6): %d cap(s6): %d\n", len(s6), cap(s6)) // 7 7
	s7 := a[:]    // 从头到尾
	fmt.Println(s7)
	fmt.Printf("len(s7): %d cap(s7): %d\n", len(s7), cap(s7)) // 10 10

	// 切片再切片
	s8 := s6[3:]
	fmt.Println(s6)  // [4 5 6 7 8 9 100]
	fmt.Println(s8)  // [7 8 9 100]
	fmt.Printf("len(s8): %d cap(s8): %d\n", len(s8), cap(s8)) // 4 4

	// make()构造切片
	s9 := make([]int, 5, 10) // 初始化默认填0
	fmt.Printf("s9: %v len(s9): %d cap(s9): %d\n", s9, len(s9), cap(s9))

	s0 := make([]int, 0, 10)
	fmt.Println(s0 == nil) // false
	fmt.Printf("s0: %v len(s0): %d cap(s0): %d\n", s0, len(s0), cap(s0))

	// 遍历切片
	for i := 0; i < len(s8); i++ {
		fmt.Println(s8[i])
	}
	for _, v := range s8 {
		fmt.Println(v)
	}

	// append()函数 追加元素
	s10 := [...]string{"上海", "深圳", "北京", "重庆"}
	s11 := s10[:]
	// s10[4] = "广州" // 错误写法导致编译出错
	fmt.Println(s11)
	s11 = append(s11, "广州")
	fmt.Println("s10: ", s10) // 原先数组并未改变，切片指向了另外一个地址
	fmt.Println(s11)
	// 容量扩大为原来的两倍
	fmt.Printf("s11: %v len(s11): %d cap(s11): %d\n", s11, len(s11), cap(s11))
	s12 := []string{"武汉", "西安", "苏州", "杭州"}
	s11 = append(s11, s12...) // ...表示拆开，一个一个装进s12拆开加入s11
	fmt.Printf("s11: %v len(s11): %d cap(s11): %d\n", s11, len(s11), cap(s11))

	// copy()函数 复制切片
	m := []int{1, 2, 3, 4, 5}
	n := m
	fmt.Println(m) // [1 2 3 4 5]
	fmt.Println(n) // [1 2 3 4 5]
	n[0] = 100
	fmt.Println(m) // [100 2 3 4 5]
	fmt.Println(n) // [100 2 3 4 5]

	var n1 []int // nil
	n1 = make([]int, len(n))
	copy(n1, n)
	n1[0] = 1000
	fmt.Println(n)  // [100 2 3 4 5]
	fmt.Println(n1) // [1000 2 3 4 5]

	// 从切片中删除元素
	j := [...]int{1, 2, 3, 4, 5}
	o := j[:]
	// 删除索引为2的元素
	o = append(o[:2], o[3:]...) // 修改了底层数组，后面的值一次向前复制
	fmt.Println(j)              // [1, 2, 4, 5, 5]
	fmt.Println(o)              // [1, 2, 4, 5]
	fmt.Printf("o: %v len(o): %d cap(o): %d\n", o, len(o), cap(o))

	// 排序
	var k = [...]int{7, 8, 6, 3, 1, 5}
	sort.Ints(k[:])
	fmt.Println(k)

	// 陷阱
	var x = make([]int, 5, 10) // 创建切片，长度为5，容量为10
	fmt.Println(x)
	for i := 0; i < 10; i++ {
		x = append(x, i)
	}
	fmt.Println(x) // [0 0 0 0 0 1 2 3 4 5 6 7 8 9]
}
```

## 指针
Go语言中不存在指针操作，只需要记住两个符号：
0. `&` 取地址
1. `*` 根据地址取值

变量、指针地址、指针变量、取地址、取值的相互关系和特性：
0. 对变量进行取地址（&）操作，可以获得这个变量的指针变量
1. 指针变量的值是指针地址
2. 对指针变量进行取值（ * ）操作，可以获得指针变量指向的原来变量的值

##### new和make分配内存
```go
var p *int // nil，无地址，不能直接赋右值，则需要申请内存
fmt.Println(p) // <nil>
```
#### new
```go
var p *int
p = new(int)
*p = 10
```
#### make
make也是用于内存分配的，区别于new，它只用于slice、map的以及chan的内存创建，而且它返回的类型就是这三个基本类型本身，而不是他们的指针类型，因为这三种类型就是引用类型，所以就没有必要返回它们的指针了  
make函数 的函数签名如下：
```go
func make(t Type, size ...IntegerType) Type
```
make函数是无可替代的，我们在使用slice、map以及channel的时候，都需要使用make进行初始化，然后才可以对它们进行操作

#### new和make的异同
0. make和new都是用于申请内存的
1. new很少使用，一般用于给基本数据类型申请内存，例如：string、int ...，返回指向对应类型的指针
2. make是用来给slice、map、chan申请内存的，返回对应类型本身，而非指针
```go
package main

import "fmt"

func main() {
	n := 10
	p := &n // var p *int = &n
	fmt.Printf("p: T(%T) V(%v) *p(%d)\n", p, p, *p)

	var p1 *int
	p1 = &n
	fmt.Println(p1)

	p2 := *p1 // p2为int类型
	fmt.Printf("%T %v \n", p2, p2)

	// new
	var p3 *int
	p3 = new(int)
	*p3 = 10
	fmt.Println(*p3)
}
```

## map
 - `map`是go语言提供的一种映射关系容器，其内部使用`散列表（hash）`实现
 - `map`是一种无序的基于`key-value`的数据结构，go语言中的map是`引用`类型，必须初始化才能使用
 - 语法：
```go
map[keyType]valueType
```
其中，KeyType表示键的类型，valueType表示值的类型  
`map`类型变量的默认初始值是nil，需要使用make()函数分配内存，语法为：  
```go
make(map[KeyType]ValueType, [cap])
```
其中cap表示map的容量，该参数不是必须的，但是我们应该在初始化map的时候就为其指定一个合适的容量  

#### 判断某个键值是否存在
```go
value, ok := m1["dream"] // 约定成俗用ok接受返回的布尔值
if !ok {
	fmt.Println("No!")
} else {
	fmt.Println(value, "Yes!") // 18 Yes!
}
// 若不使用上面方式打印值，则打印一个键值不存在的map类型的值通常返回0值
// 即：
fmt.Println(m1["nohappy"]) // 0
```

#### map的遍历
```go
// 遍历key和value
for k, v := range mapVar {
	... ...
}

// 只遍历key
for k := range mapVar {
	... ...
}

// 只遍历value
for _, v := range mapVar {
	... ...
}
```
 - 遍历map时的元素顺序与添加键值对的顺序无关

#### 使用delete()函数删除键值对
使用内建函数`delete()`从map中删除一组键值对，格式如下：  
```go
delete(map, key)
```
其中，map表示要删除键值对的map，key表示要删除的键值对的键

#### 元素为map类型的切片，值为切片的map

#### 代码
```go
package main

import "fmt"

func main() {
	var m1 map[string]int

	fmt.Println(m1) // nil

	m1 = make(map[string]int, 1) // 可以自动扩容，但要避免，会降低性能
	m1["dream"] = 18
	m1["happy"] = 19
	m1["comfortable"] = 35
	fmt.Printf("m1: %T, %v\n", m1, m1)

	// 判断某个键值是否存在
	value, ok := m1["dream"] // 约定成俗用ok接受返回的布尔值
	if !ok {
		fmt.Println("No!")
	} else {
		fmt.Println(value, "Yes!") // 18 Yes!
	}
	// 若不使用上面方式打印值，则打印一个键值不存在的map类型的值通常返回0值
	// 即：
	fmt.Println(m1["nohappy"]) // 0

	// map的遍历
	for k, v := range m1 {
		fmt.Println(k, v)
	}

	// delete() 删除键值对
	delete(m1, "comfortable") // 若删除不存在的，则什么都不干，也不报错
	delete(m1, "comfortable")
	fmt.Println(m1)

	// 元素为map类型的切片
	s := make([]map[int]string, 10, 10) // 第二个参数表示长度，在这里不可以是0
	// 没有对内部的map做初始化
	s[0] = make(map[int]string, 1)
	s[0][100] = "aaa"
	fmt.Println(s)

	// 值为切片的map类型
	m2 := make(map[string][]int, 10)
	m2["happy"] = []int{10, 20, 30}
	s1 := make([]int, 3)
	s1[0] = 1
	s1[1] = 2
	s1[2] = 3
	m2["dream"] = s1
	fmt.Println(m2)
}
```

## 函数
Go语言中定义函数使用`func`关键字，具体格式如下：
```go
func 函数名(参数)(返回值) { // 多个返回值需要用()包裹
	函数体
}
```
Go语言中没有`默认参数`这个概念  
Go语言中的函数传参传递的都是值（复制、拷贝）

### 函数进阶

#### 变量作用域
 - 全局变量：从定义开始直到程序结束都有效
 - 局部作用域：从定义开始到定义该变量的局部消亡，变量也随即消亡，不复存在

#### 函数类型和变量
##### 定义函数类型
Go语言中可以使用`type`关键字来定义一个函数类型，格式：
```go
type calculation func(int, int) int
```

 - 函数也可以作为参数的类型进行使用
 - 函数还可以作为返回值

#### 匿名函数
即没有名字的函数，一般用于函数内部
```go
// 匿名函数用作全局函数，一般不这样用
var f = func(x, y int) {
	fmt.Println(x + y)
}

// 用于函数内部，只调用一次、立即执行 函数
package main

import "fmt"

func main() {
	f := func(x, y int) {
	fmt.Println(x + y)
	} // 不会立即执行，需要调用，如下，一般不这样用
	f(100, 200)

	func(x, y int) {
		fmt.Println("x:", x)
		fmt.Println("y:", y)
	}(100, 200) // 只调用一次、函数定义完后加()立即执行
}
```

### 递归
递归，就是在运行过程中调用自己  
Go语言支持递归，但我们在使用递归时，开发者需要设置退出条件，否则递归将陷入无限循环中  
递归对于解决数学上的问题是非常有用的，例如：阶乘计算、生成斐波那契数列等
```go
func recursion() {
	recursion() // 函数调用自身
}

func main() {
	recursion()
}
```
 - 上台阶 面试题
n个台阶，一次可以走1步，也可以走2步，有多少种走法？
```go
func f(n uint64) uint64 {
	if n == 1 {
		// 如果只有一个台阶就一种走法
		return 1
	}
	
	if n == 2 {
		return 2
	}
	
	return f(n-1) + f(n-2)
}

func main() {
	ret := f(n) // n 为具体的数值
	fmt.Println(ret)
}
```

#### 闭包
闭包指的是一个函数和与其相关的引用环境组合而成的实体  
简单来说，闭包 = `函数 + 引用环境`，如下例子：
```go
package main

import "fmt"

// 示例一：
func adder(x int) func(int) int {
	return func(y int) int {
		x += y
		return x
	}
}

// 示例二：
func f1(f func()) { // 调用函数
	fmt.Println("This is f1")
	f()
}

func f2(x, y int) { // 被调用函数
	fmt.Println("This is f2")
	fmt.Println("x + y =", x+y)
}

func closure(f func(int, int), x, y int) func() { // 闭包函数
	tmp := func() {
		f(x, y)
	}
	return tmp
}

func main() {
	// 示例一：
	var f = adder(100) 	// f 为 func(int) int 类型
	ret := f(10) 		// ret 为 int 类型
	fmt.Println(f(10)) 	// 110
	
	// 示例二：
	closuref := closure(f2, 10, 20) // 把原来需要传递两个int类型改写成为不需要传参的函数
	closuref()                       // 执行函数
	f1(closuref)                     // 将闭包返回函数传递给调用函数
	// 语句 closuref() 输出：
	// This is f2
	// x + y = 30
	// 语句 f1(closuref) 输出：
	// This is f1
	// This is f2
	// x + y = 30
}
```
总结：闭包是一个函数，这个函数包含了它外部作用域的一个变量  
底层的原来：函数可以作为返回值、函数内部查找变量的顺序，先在自己内部找，找不到往外层找

#### 内置函数
|  内置函数  |  介绍  |
|  ----  |  ----  |
|  close  |  只要用于关闭channel  |
|  len  |  求长度，比如string、array、slice、map、channel  |
|  new  |  用于分配内存，主要用于分配值类型，比如int、struct，返回指针  |
|  make  |  用于分配内存，主要用于分配引用类型，比如chan、map、slice  |
|  append  |  用于追加元素到slice中  |
|  panic和recover  |  用于做错误处理  |

##### panic/recover
Go语言中没有错误机制，使用`panic/recover`模式来处理错误  
`panic`可以在任何地方发生，`recover`只有在`defer`调用的函数中有效
```go
func funcA() {
	fmt.Println("a")
}

func funcB() {
	panic("出现错误 ...... ") // 程序崩溃退出
	fmt.Println("b")
}

func funcC() {
	fmt.Println("c")
}

func main() {
	funcA()
	funcB()
	funcC()
}
// a 程序崩溃退出

func funcA() {
	fmt.Println("a")
}

func funcB() {
	defer func() {
		err := recover()
		fmt.Println(err)
		fmt.Println("继续执行...")
	}()
	panic("出现错误 ...... ") // 程序崩溃退出
	fmt.Println("b") // 虽处理了错误，该语句不会被执行
}

func funcC() {
	fmt.Println("c")
}

func main() {
	funcA()
	funcB()
	funcC()
}
// a 出现错误 ...... 继续执行... c
```
总结：
0. 在实际开发中，其实很少使用 panic/recover
1. recover()必须搭配defer使用
2. defer一定要在可能引发panic的语句之前定义

### defer语句
`defer`语句会将其后面跟随的语句进行延迟处理，在`defer`归属的`函数即将返回`时，将延迟处理的语句按`defer`定义的逆序进行执行，也就说，先被`defer`的语句最后执行，最后被`defer`的语句最先执行  
多用于函数结束之前释放资源（文件句柄、数据库连接、socket连接）  
相当于`栈`操作
```go
package main

import "fmt"

func calc(index string, a, b int) int {
	ret := a + b
	fmt.Println(index, a, b, ret)
	return ret
}

func main() {
	fmt.Println("start")
	defer fmt.Println("hhh")
	defer fmt.Println("eee")
	defer fmt.Println("xxx")
	defer fmt.Println("vvv")
	fmt.Println("end")
	
	a := 1
	b := 2
	defer calc("1", a, calc("10", a, b))
	a = 0
	defer calc("2", a, calc("20", a, b))
	b = 1
}
// 结果如下：
// start
// end
// "10" 1 2 3
// "20" 0 2 2

// "2" 0 2 2
// "1" 1 3 4
// vvv
// xxx
// eee
// hhh
```

#### defer执行时机
在go语言中，函数最后的`return`语句在底层并不是原子操作，它分为两步：
0. 给返回值赋值
1. 执行RET指令  
而`defer`语句执行的时机就在返回值赋值操作后，RET指令执行前
![alt defer执行时机](./0.png)
```go
package main

import "fmt"

// 函数返回的底层执行步骤：
// 第一步：返回值赋值
// defer  若存在
// 第二步：真正的RET返回

func f1() int {
	x := 5
	defer func() {
		x++
	}()
	return x
}

func f2() (x int) {
	defer func() {
		x++
	}()
	return 5 // 给返回值x赋值5 -> x++ -> 真正返回6
}

func f3() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x
}

func f4() (x int) {
	defer func(x int) {
		x++
	}(x) // 这儿把返回值当作参数传进函数，defer操作的是x的副本
	return 5
}

func main() {
	fmt.Println("start")
	defer fmt.Println("hhh")
	defer fmt.Println("eee")
	defer fmt.Println("xxx")
	defer fmt.Println("vvv")
	fmt.Println("end")

	x := f1()
	fmt.Println(x) // 5

	y := f2()
	fmt.Println(y) // 6

	z := f3()
	fmt.Println(z) // 5

	a := f4()
	fmt.Println(a) // 5
}
```

### 函数总结（代码）：
```go
package main

import "fmt"

// 函数返回的底层执行步骤：
// 第一步：返回值赋值
// defer  若存在
// 第二步：真正的RET返回

func f1() int {
	x := 5
	defer func() {
		x++
	}()
	return x
}

func f2() (x int) {
	defer func() {
		x++
	}()
	return 5 // 给返回值x赋值5 -> x++ -> 真正返回6
}

func f3() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x
}

func f4() (x int) {
	defer func(x int) {
		x++
	}(x) // 这儿把返回值当作参数传进函数，defer操作的是x的副本
	return 5
}

func f5() {
	fmt.Println("Let's go!")
}

func f6() int {
	return 10
}

// 函数作为参数
func f7(x func() int) (s int) {
	s = x()
	return
}

// 函数作为返回值
func f8(x func() int) (y func() int) {
	y = x
	return
}

// 用于 闭包 的函数 f9、f10、closure()
func f9(f func()) { // 调用函数
	fmt.Println("This is f9")
	f()
}

func f10(x, y int) { // 被调用函数
	fmt.Println("This is f10")
	fmt.Println("x + y =", x+y)
}

func closure(f func(int, int), x, y int) func() { // 闭包函数
	tmp := func() {
		f(x, y)
	}
	return tmp
}

// panic/recover
func f11() {
	fmt.Println("a")
}

func f12() {
	defer func() {
		err := recover()
		fmt.Println(err)
		fmt.Println("继续执行...")
	}()
	panic("出现错误 ...... ") // 程序崩溃退出
}

func f13() {
	fmt.Println("c")
}

func main() {
	fmt.Println("start")
	defer fmt.Println("hhh")
	defer fmt.Println("eee")
	defer fmt.Println("xxx")
	defer fmt.Println("vvv")
	fmt.Println("end")

	x := f1()
	fmt.Println(x) // 5

	y := f2()
	fmt.Println(y) // 6

	z := f3()
	fmt.Println(z) // 5

	a := f4()
	fmt.Println(a) // 5

	// 函数类型及传参
	b := f5
	b()                      // Let's go!
	fmt.Printf("b: %T\n", b) // b: func()

	c := f6
	d := c()
	fmt.Printf("c: %T d = %d\n", c, d) // c: func() int d = 10

	// 函数当作参数
	var e func(func() int) int
	e = f7
	f := e(f6)
	fmt.Printf("e: %T f = %d\n", e, f) // e: func(func() int) int f = 10

	// 函数当作参数、返回值
	var g func(func() int) func() int
	g = f8
	h := g(f6)
	i := h()
	fmt.Printf("g: %T h: %T i: %d\n", g, h, i) // g: func(func() int) func() int h: func() int i: 10

	// 匿名函数：用于 只调用一次、立即执行 函数
	func(x, y int) {
		fmt.Println("x:", x)
		fmt.Println("y:", y)
	}(100, 200)

	// 闭包
	closuref := closure(f10, 10, 20) // 把原来需要传递两个int类型改写成为不需要传参的函数
	closuref()                       // 执行函数
	f9(closuref)                     // 将闭包返回函数传递给调用函数

	// 内置函数panic/recover
	f11()
	f12()
	f13()
}
```

## fmt
fmt包实现类似C语言printf和scanf的格式化IO，主要分为向外输出内容和获取输入内容两部分。  

#### 向外输出
`fmt`提供的相关函数如下：

##### Print
`Print`系列函数会将内容输出到系统的标准输出  
区别：  
0. `Print`函数直接输出内容，不加换行
1. `Printf`函数支持格式化输出字符串，不加换行
2. `Println`函数会在输出内容的结尾自动添加一个换行符
```go
func Print(a ...interface{}) (n int, err error)
func Printf(format string, a ...interface{}) (n int, err error)
func Println(a ...interface{}) (n int, err error)
// 格式化占位符见下方
```

##### Fprint
`Fprint`系列函数会将内容输出到一个`io.Writer`接口类型的变量`w`中，我们通常用这个函数往文件中写入内容
```go
func Fprint(w io.Writer, a ...interface{}) (n int, err error)
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```

#### Sprint
`Sprint`系列函数会把传入的数据生成并返回一个字符串，拼接
```go
func Sprint(a ...interface{}) string
func Sprintf(format string, a ...interface{}) string
func Sprintln(a ...interface{}) string
```

#### Errorf
`Errorf`函数根据format参数生成格式化字符串并返回一个包含该字符串的错误
```go
func Errorf(format string, a ...interface{}) error
```

#### 获取输入
Go语言`fmt`包下有`fmt.Scan`、`fmt.Scanf`、`fmt.Scanln`三个函数，可以在程序运行过程中从标准输入获取用户的输入

##### fmt.Scan
```go
func Scan(a ...interface{})(n int, err error)
```
0. Scan从标准输入扫描文本，读取由空白字符分隔的值保存到传递给本函数的参数中，换行符视为空白符
1. 本函数返回成功扫描的数据个数和遇到的任何错误，如果读取的数据个数比提供的参数少，会返回一个错误报告原因
```go
var s string
fmt.Scan(&s)
fmt.Println(s)
// Nice
// Nice 
```

##### fmt.Scanf
可以使用占位符
```go
var (
	name string
	age int
	class string
)
fmt.Scanf("%s %d %s\n", &name, &age, &class)
fmt.Println(name, age, class)
// happy 12 class8
// happy 12 class8
// 
```

##### fmt.Scanln
扫描到换行
```go
var (
	name string
	age int
	calss string
)
fmt.Scanln(&name, &age, &calss)
fmt.Println(name, age, calss)
// happy 12 class8
// happy 12 class8
// 
```

#### 格式化占位符
`*printf`系列函数都支持format格式化参数

##### 通用占位符
|  占位符  |  说明  |
|  ----  |  ----  |
|  %v  |  值的默认格式表示  |
|  %+v  |  类似%v，但在输出结构体时会添加字段名  |
|  %#v  |  值的Go语法表示（输出更加详细，包括类型、""等）  |
|  %T  |  值的类型  |
|  %%  |  百分号  |

##### 布尔型
|  占位符  |  说明  |
|  ----  |  ----  |
|  %t  |  true或false  |

##### 整型
|  占位符  |  说明  |
|  ----  |  ----  |
|  %b  |  二进制值  |
|  %o  |  八进制值  |
|  %d  |  十进制值  |
|  %x  |  十六进制值，使用a-f  |
|  %X  |  十六进制值，使用A-F  |
|  %c  |  该值对应的unicode码值  |
|  %U  |  表示为Unicode格式：U+1234，等价于"U+%O4X"  |
|  %q  |  表示单引号括起来的字符值（类似ASCII码），必要时会采用安全的转义表示  |

##### 浮点数与复数
|  占位符  |  说明  |
|  ----  |  ----  |
|  %b  |  无小数部分，二进制指数的科学计数法，如-123456p-78  |
|  %e  |  科学计数法，如-1234.456e+78  |
|  %E  |  科学计数法，如-1234.456E+78  |
|  %f  |  有小数部分但无指数部分，如123.456  |
|  %F  |  等价于 %f  |
|  %g  |  根据实际情况采用%e或%f格式（以获得更简洁、准确的输出）  |
|  %G  |  根据实际情况采用%E或%F格式（以获得更简洁、准确的输出）  |

##### 字符串和 []byte
|  占位符  |  说明  |
|  ----  |  ----  |
|  %s  |  直接输出字符串或者[]byte  |
|  %q  |  该值对应的双引号括起来的go语法字符串字面值，必要时会采用安全的转义表示  |
|  %x  |  每个字节用两字符十六进制数表示（使用a-f）  |
|  %X  |  每个字节用两字符十六进制数表示（使用A-F）  |

##### 指针
|  占位符  |  说明  |
|  ----  |  ----  |
|  %p  |  表示为十六进制，并加上前导的0x  |

##### 宽度标识符
`宽度`通过一个紧跟在百分号后面的十进制数指定，如果未指定宽度，则表示值时除必需之外不作填充  
`精度`通过`宽度+.`后跟的十进制数指定，如未指定精度，会使用默认精度，如果`.`后没有跟数字，则表示精度为0，比如：
|  占位符  |  说明  |
|  ----  |  ----  |
|  %f  |  默认宽度，默认精度  |
|  %9f  |  宽度9，默认精度  |
|  %.2f  |  默认宽度，精度2  |
|  %9.f  |  宽度9，精度0  |
|  %9.2f  |  宽度9，精度2  |

##### 其它
|  占位符  |  说明  |
|  ----  |  ----  |
|  '+'  |  总是输出数值的正负号；对%q（%+q）会生成全部是ASCII字符的输出（通过转义）  |
|  ' '  |  对数值，正数前加空格而负数前加符号；对字符串采用%x或%X时（% x或% X）会给各打印的字节之间加空格  |
|  '-'  |  在输出右边填充空白而不是默认的左边（即从默认的右对齐切换为左对齐）  |
|  '#'  |八进制数前加o（%#o），十六进制前加0x（%#x或%#X），指针去掉前面的0x（%#p），对于%q和%U（%#q、%#U）会输出空格和单引号括起来的go字面值  |
|  '0'  |  使用0而不是空格填充，对于数值类型会把填充的0放在正负号后面  |

## 自定义类型和类型别名

#### 自定义类型
使用`type`关键字来定义自定义类型  
```go
// 将MyInt定义为int类型
type myInt int
```
通过`type`关键字的定义，`MyInt`就是一种新的类型，它具有`int`的特性

#### 类型别名
目的为使代码更具有可读性
```go
type myInt = int
```
`rune`和`byte`都是类型别名，type rune = int32，type byte = uint8

##### 示例
```go
package main

import "fmt"

type myInt int     // 自定义类型
type yourInt = int // 类型别名

func main() {
	// 自定义类型
	var n myInt
	n = 100
	fmt.Printf("%T\n", n) // main.myInt
	
	// 类型别名
	var m yourInt
	m = 100
	fmt.Printf("%T\n", m) // int
}
```

## 结构体
Go语言种没有“类”的概念，也不支持“类”的继承等面向对象，但是通过结构体的内嵌再配合接口比面向对象具有更高的扩展性和灵活性  
Go语言中通过`struct`来实现面向对象

#### 结构体的定义
使用`type`和`struct`关键字定义结构体，格式如下：
```go
type 类型名 struct {
	字段名 字段类型
	字段名 字段类型
	... ...
}
```
其中，  
`类型名`标识自定义结构体的名称，在同一个包内不能重复；  
`字段名`表示结构体字段名，结构体中的字段名必须唯一
`字段类型`表示结构体字段的基本类型

#### 匿名结构体
在定义一些临时数据结构的场景下还可以使用匿名结构体
```go
var s struct {
	name string
	age  int
}
s.name = "Bruce Cheung"
s.age = 23

fmt.Println(s) // {Bruce Cheung 23}
```

#### 结构体初始化
0. 使用`变量名.字段`方法初始化
1. 键值对初始化，最常使用
2. 列表初始化，必须初始化结构体所有字段且顺序与定义一致
```go
// 第一种方式
var varStruct myStruct
varStruct.字段名 = 值
varStruct.字段名 = 值
... ...

// 第二种方式
varStruct := myStruct {
	字段名: 值,
	字段名: 值,
	... ...,
}

// 第三种方式
varStruct := myStruct {
	值1,
	值2,
	... ..., // 必须对全部字段设定值
}
```

#### 创建指针类型结构体
我们可以通过`new`关键字对结构体进行实例化，得到结构体变量的地址
```go
var p = new(person)
fmt.Printf("%T\n", p)      // *main.person
fmt.Printf("p = %#v\n", p) // p2 = &main.person{name:"", city:"", age:0}
```
注意：在Go语言中支持对结构体指针直接使用`.`来访问结构体的成员

##### 结构体占用一块连续的内存空间

#### 构造函数
Go语言中的构造函数就是单独开辟一个函数来创造结构体变量，且约定成俗该函数以new开头  
```go
type 结构体名 struct {
	字段1 类型1
	字段2 类型2
	... ...
}

// 构造函数
func new结构体名(字段1 类型1, 字段2 类型2, ... ...) *结构体名 {
	return &结构体名 {
		// 初始化字段
	}
}
// 返回的结构体是值或指针
// 当结构体比较大的时候尽量使用结构体指针，减少程序的内存开销

// 示例：
type person struct {
	name   string
	age    int
	gender string
	hobby  []string
	setAge func(int) int
}

func newPerson(name string, age int, gender string, hobby []string, setAge func(int) int) *person {
	return &person{
		name:   name,
		age:    age,
		gender: gender,
		hobby:  hobby,
		setAge: setAge,
	}
}

p := newPerson("Joker", 35, "男", []string{"大笑", "镜子"}, setAgee)
fmt.Printf("%T\n", p)
fmt.Println(*p)
```

#### 方法和接收者
Go语言中的`方法（Method）`是一种作用于特定类型变量的函数，这种特定类型变量叫做`接收者（Receiver）`  
接收者就类似于其它语言中的`this`或`self`  
方法定义格式如下：
```go
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
	函数体
}
```
其中，  
0. 接收者变量：接收者中的参数变量名在命名时，官方建议使用接收者类型名的第一个小写字母，而不是self、this之类的命名，例如：Person类型的接收者变量应该命名为p，Connector类型的接收者变量应该命名为c ... ...
1. 接收者类型：接收者类型和参数类型可以是指针或非指针类型
2. 方法名、参数列表、返回参数：具体格式与函数定义相同
```go
type person struct {
	name   string
	age    int
	gender string
	hobby  []string
	setAge func(int) int
}

func newPerson(name string, age int, gender string, hobby []string, setAge func(int) int) *person {
	return &person{
		name:   name,
		age:    age,
		gender: gender,
		hobby:  hobby,
		setAge: setAge,
	}
}

func (p person)walk() {
	fmt.Printf("My name is %s, 我会走路！", p.name)
	// ... ...
}

// 值传递
func (p person) year() {
	p.age++
}

// 引用传递
func (p *person) year1() {
	p.age++
}

// 声明
p := newPerson("Joker", 35, "男", []string{"大笑", "镜子"}, setAgee)
fmt.Printf("%T\n", p)
fmt.Println(*p)

// 调用方法
p.walk()

p.year() // 35，未进行改变
fmt.Println(p2)
p.year1() // 36，已改变
fmt.Println(p2)
```

##### 只能给自定义类型设置方法，也不能为别的包里面的类型添加方法
```go
package main

import "fmt"

type myInt int

func (m myInt) hello() { // 但是不能直接为int构造方法
	fmt.Println("我是一个int\n")
}

func main() {
	m := myInt(100)
	m.hello()
}
```

#### 什么时候使用指针类型接收者
0. 需要修改接收者中的值
1. 接收者是拷贝代价比较大的对象
2. `保证一致性`，如果某个方法使用了指针接收者，那么其它方法也应该使用指针接收者

##### 标识符：变量名、类型名、方法名、函数名
##### Go语言中如果标识符首字母是大写的，就表示对外部包可见（公有的（public））
```go
// Person 这里是相关描述，这是必需的，否则会有警告
type Person struct { // Person首字母大写，表示被别的包导入时可见，可使用
	// ... ...
}
```

#### 匿名字段
结构体允许其成员字段在声明时没有字段而只有类型，这种没有名字的字段就称为匿名字段  
适用于字段少也比较简单的场景，不常用
```go
type Person struct {
	string
	int
}

func main() {
	p := Person {
		"小王子",
		18,
	}
	fmt.Printf("%#v\n", p)       // main.Person{string:"小王子", int:18}
	fmt.Println(p.string, p.int) // 小王子 18
}
```

#### 嵌套结构体
```go
type address struct {
	name string
	city string
}

type company struct {
	name     string
	province string
	city     string
	addr     address // 嵌套
}

type company1 struct {
	name string
	address 		// 匿名嵌套
}

c := company{
	name:     "staff",
	province: "cq",
	city:     "cq",
	addr: address{
		name: "address",
		city: "cq",
	},
}
fmt.Println(c, c.city) // 无匿名嵌套结构体，只在本结构体中寻找city字段

c1 := company1{
	name:     "staff",
	addr: address{
		name: "address",
		city: "cq",
	},
}
fmt.Println(c1.city) // 先在自己结构体查找city字段，找不到再到匿名嵌套结构体中查找
// 但是如果有多个匿名嵌套结构体且有重复的字段名，需要写完整路径查找字段
```

#### 结构体的”继承“
Go语言中使用结构体可以实现其它编程语言中面向对象的继承
```go
type animal struct {
	name string
}

func (a animal) move() {
	fmt.Printf("%s会移动！\n", a.name)
}

type cat struct {
	feet uint8
	animal
}

func (c cat) mia() {
	fmt.Printf("%s: mia~ ~ ~\n", c.name)
}

c := cat{
	animal: animal{name: "blue"},
	feet:   4,
}
fmt.Println(c) // {4 {blue}}
c.move() // blue会移动！
c.mia()  // blue: mia~ ~ ~
```

#### 结构体与JSON
0. 把go语言中的结构体变量 -> json格式的字符串，这个过程就叫做序列号
1. json格式字符串 -> go语言中能够识别的结构体变量，反序列化
```go
package main

import (
	"fmt",
	"encoding/json"

type person struct {
	Name string // 没有大写的话 json包不能进行访问
	Age int
}

type person struct {
	Name string `json:"name" db:"name" ini:"name"`
	Age int		`json:"age"` // 实现前端必须使用小写开头的需求
}

func main() {
	p := person{
		Name: "Dream",
		Age:  9999,
	}
	
	// 序列化
	b, err := json.Marshal(p)
	if err != nil {
		fmt.Printf("marshal failed, err: %v", err)
		return
	}
	fmt.Println(string(b)) // {"name":"Dream","age":9999}
	
	// 反序列化
	str := `{"name":"Dream","age":9999}`
	var p4 person1
	json.Unmarshal([]byte(str), &p4) // 注意：函数传参永远传递的是副本，传指针为了能够进行修改
	fmt.Println(p4) // {Dream 9999}
}
```

#### 结构体总结（代码）
```go
package main

import (
	"encoding/json"
	"fmt"
)

type person struct {
	name   string
	age    int
	gender string
	hobby  []string
	setAge func(int) int
}

func setAgee(a int) (age int) {
	age = a
	return
}

// 构造函数
func newPerson(name string, age int, gender string, hobby []string, setAge func(int) int) *person {
	return &person{
		name:   name,
		age:    age,
		gender: gender,
		hobby:  hobby,
		setAge: setAge,
	}
}

// 方法 作用于特定类型的函数
func (p person) walk() {
	fmt.Printf("My name is %s, 我会走路！\n", p.name)
}

func (p person) year() {
	p.age++
}

func (p *person) year1() {
	p.age++
}

// 匿名字段
type dog struct {
	string
	int
}

// 嵌套结构体
type address struct {
	name string
	city string
}

type company struct {
	name     string
	province string
	city     string
	addr     address
}

type company1 struct {
	name     string
	province string
	address
}

// 继承
type animal struct {
	name string
}

func (a animal) move() {
	fmt.Printf("%s会移动！\n", a.name)
}

type cat struct {
	feet uint8
	animal
}

func (c cat) mia() {
	fmt.Printf("%s: mia~ ~ ~\n", c.name)
}

// 结构体与json
type person1 struct {
	Name string `json:"name"` // 没有大写的话 json包不能进行访问，就无法转换
	Age  int    `json:"age"`
}

func main() {
	var oubc person
	oubc.name = "oubc97"
	oubc.age = 21
	oubc.gender = "男"
	oubc.hobby = []string{"足球", "编程", "C/C++"}

	fmt.Println(oubc) // {oubc97 21 男 [足球 编程 C/C++] <nil>}

	oubc.setAge = setAgee
	fmt.Println(oubc.setAge(22)) // 22

	// 匿名结构体，多用于临时场景
	var s struct {
		name string
		age  int
	}
	s.name = "Bruce Cheung"
	s.age = 23

	fmt.Println(s) // {Bruce Cheung 23}

	// 声明且初始化，通常使用此初始化方式
	var s1 = person{
		name:   "Crysatl",
		age:    25,
		gender: "女",
		hobby:  []string{"画画", "摄影"},
	}
	fmt.Println(s1) // {Crysatl 25 女 [画画 摄影] <nil>}

	// 使用值列表的形式初始化，顺序必须与定义一致
	s2 := person{
		"小王子",
		21,
		"男",
		[]string{"游戏", "Linux"},
		setAgee,
	}
	fmt.Println(s2) // {小王子 21 男 [游戏 Linux] 0x49f8f0}

	// 结构体指针
	var p *person
	p = &oubc
	fmt.Printf("T: %T, p = %#v\n", p, p) // T: *main.person

	var p1 = new(person)
	p1.name = "James"
	p1.age = 24
	p1.gender = "女"
	p1.hobby = []string{"游泳", "JS"}
	fmt.Println(*p1) // {James 24 女 [游泳 JS] <nil>}

	// 使用构造函数构造一个对象
	p2 := newPerson("Joker", 35, "男", []string{"大笑", "镜子"}, setAgee)
	fmt.Printf("%T\n", p2)
	fmt.Println(*p2)

	// 调用方法
	p2.walk()
	p2.year() // 35，未进行改变
	fmt.Println(p2)
	p2.year1() // 36，已改变
	fmt.Println(p2)

	// 匿名字段使用
	d := dog{
		"旺财",
		2,
	}
	fmt.Printf("%T\n", d)
	fmt.Println(d.string, d.int)

	// 嵌套结构体
	c := company{
		name:     "staff",
		province: "cq",
		city:     "cq",
		addr: address{
			name: "address",
			city: "addrcq",
		},
	}
	fmt.Println(c, c.city) // cq

	// 匿名嵌套结构体
	c1 := company1{
		name:     "staff",
		province: "cq",
		address: address{
			name: "address",
			city: "addrcq",
		},
	}
	fmt.Println(c1.city) // addrcq

	// 继承
	c2 := cat{
		animal: animal{name: "blue"},
		feet:   4,
	}

	fmt.Println(c2)
	c2.move()
	c2.mia()

	// 结构体与json
	// 序列化
	p3 := person1{
		Name: "Dream",
		Age:  9999,
	}

	b, err := json.Marshal(p3)
	if err != nil {
		fmt.Printf("marshal failed, err: %v", err)
		return
	}
	fmt.Println(string(b))

	// 反序列化
	str := `{"name":"Dream","age":9999}`
	var p4 person1
	json.Unmarshal([]byte(str), &p4) // 注意：函数传参永远传递的是副本，传指针为了能够进行修改
	fmt.Println(p4) // {Dream 9999}
}
```















