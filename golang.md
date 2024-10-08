### golang应用
- 服务器端数据处理，大并发
- 游戏软件数据通道，即数据同步
- 分布式文件系统，云计算
### 为什么需要golang
- 计算机硬件更新频繁，现有编程语言不能合理利用多核多CPU优势进行开发
- 缺乏一个计算能力强能处理大并发且足够简洁高效的编程语言
- c语言运行快但编译太慢，且有内存泄漏问题
### golang的特点
- 既有静态编译语言的安全和性能，又有动态语言开发维护的高效率
- go语言的一个文件都要归属于一个包
- 垃圾回收机制，内存自动回收
- 天然并发，使用goroutine轻量级线程，基于CPS并发模型实现
- 吸收管道通信机制，实现不同的goroute之间相互通信
### 程序开发过程
- goproject->src->go_code->project1->main
- **go build**(-o 可以用来指定生成可执行文件的文件名) 命令对.go文件进行编译，生成.exe文件，再去执行文件（可以用 **go run**命令直接执行.go文件,二者区别如下）
- - 编译生成的可执行文件在没有go开发环境的机器上仍然可以运行，但go run go源代码 则不行
- - 编译器会将程序依赖的库文件包含在可执行文件中，可执行文件会大很多
- go语言定义的变量或者import的包没有用到，代码编译会出错
### Dos基本命令

> cd /d 绝对或相对路径
> md 目录文件
> rd /q/s 目录文件
> echo hello >d:\test.txt
> copy abc.txt d:\test100
> copy abc.txt d:\test100\ok.txt
> move abc.txt f:\
> del abc.txt
> cls 清屏
### 变量数据类型

 1. **基本数据类型：**
 数值型：int uint(根据OS位数不同默认不同位数，64位)  float32 float64(浮点型默认声明为float64)
 字符型：没有专门的字符类型,如果要存储单个字符,一般用byte(= uint8)，rune(= int32)用来存储单个汉字字符. golang中, 字符的本质就是整数, 字符以整数形式存储
 布尔型：bool(取值true false,,不能用0或非零来表示, 占用一个字节)
 **字符串型**：string, golang的字符串不是由字符组成, 而是由**字节**组成. 可以使用双引号"" 或者反引号`` 来表示字符串。字符串在底层就是一个**byte数组**
 2. **复杂数据类型**：指针、数组、结构体、管道、函数、切片、接口、map
 ```go
 // 引入多个包的格式
 import(
	 "fmt"
	 _ "unsafe" // 对于暂时不用的包可以使用下划线
)

 // 查看某个变量的占用字节大小和类型
// fmt.Printf("var1 的类型是 %T var占用的字节数是 %d",var1, unsafe.Sizeof(var1)
 
 var c1 byte = 'a'
 var c2 byte = '0'
 var c3 int = '国'
 var c4 int = 22269  // '国'->22269
 // 当我们直接输出字符byte值,就是输出了对应字符的ASCII码值
 fmt.Println("c1=", c1)
 // 我们希望输出对应字符,需要使用格式化输出
 fmt.Printf("c2=%c", c2)
 // 能将数字转换为字符输出
fmt.Printf("c4=%c", c4)

//基本类型转string类型
fmt.Sprintf
 ```
 3. **基本类型转string类型**
（1）使用第一种 fmt.Sprintf方法
 ```go
var num1 int = 99
var num2 float64 = 23.456
var b bool = true
var myChar byte = 'h'
var str string // 空str
// 如果把 %v改为 %q，则输出是带有双引号的字符串
str = fmt.Sprintf("%d", num1)
fmt.Printf("str type %T str = %v\n", str, str)

str = fmt.Sprintf("%f", num2)
fmt.Printf("str type %T str = %v\n", str, str)

str = fmt.Sprintf("%t", b) // 布尔值 %t
fmt.Printf("str type %T str = %v", str, str)

str = fmt.Sprintf("%c", myChar)
fmt.Printf("str type %T str = %q", str, str)
```
（2）使用strconv包
```go
import "strconv"
var num3 int = 99
var num4 float64 = 23.456
var b2 bool = true
// 将int64型的整数以十进制方式输出为字符串
str = strconv.FormatInt(int64(num3), 10)
// 将int型整数输出为字符串
str = strconv.Itoa(num3)
// 'f'表示一种格式 10表示小数位保留10位 64表示这个小数是float64型
str = strconv.FormatFloat(num4, 'f', 10, 64)

str = strconv.FormatBool(b2)
```
4. **string类型转基本类型**
```go
// string数据类型转基本数据类型
var str string = "true"
var b bool
// ParseBool方法会返回两个值(value bool, err error)
b, _ = strconv.ParseBool(str)

var str2 string = "123456"
var n1 int64
n1, _ = strconv.ParseInt(str2, 10, 64)

var str3 string = "123.456"
var f1 float64
f1, _ = strconv.ParseFloat(str3, 10, 64)
```
说明：ParseXx方法返回的一般是int64和float64
- 要确保String类型能转成有效的数据类型，若是无效类型就会返回默认值（0、false）
- 若要判断字符串0是否转换成功，可以查看返回的err信息，err为NULL则转换成功
5. **指针**
```go 
a := 10
var ptr *int = a  // 错误写法
var ptr *int = &a // 正确写法
```
6. **值类型与引用类型**

1）值类型：都有对应的指针类型，包括基本数据类型、数组、结构体
变量直接存储值，内存**通常**在栈中分配（取决于go的逃逸分析）
2）引用类型：指针、切片、map、管道、interface等，必须通过make分配存储空间后才能使用
引用类型**通常**在堆区分配空间，当没有引用这个变量所存储的地址时，该地址所对应的数据空间成为了一个垃圾，由GC来回收

7. **命名**

包名：保持package的名字与目录保持一致，简短又有意义但不能与标准库冲突
变量名、函数名、常量名：驼峰命名法
如果变量名、函数名、常量名首字母大写，则可以被其他的包访问，如果首字母小写，只能在本包中使用
**go mod init 命令用于导入自己编写的其他包**

8. **25个保留关键字和36个预定标识符**
### 运算符
```go
10 / 4  // 2
10.0 / 4 // 2.5
a % b = a - a / b * b
&& // 逻辑与
|| // 逻辑或
! // 逻辑非
```
### 常量const
常量在定义时必须初始化，常量不能修改，只能修饰bool、数值、string
```go
const (
	a = iota  // a 值为0
	b  // 值为1
	c  // 值为2
)
```

### 键盘输入语句
```go
fmt.Scanln() // 以行为单位输入数据
fmt.Scanf()  // 可以按照指定格式输入
fmt.Scanf("%s %d %f %t", &name, &age, &sal, &isPase) // 输入一行空格隔开的数据
```
### 分支控制
```go
if age := 20; age > 18{ // 允许在if后面实行赋值操作，而且不加()在条件判断上
	fmt.Println("年龄大于18")
}else{  // 强制格式
代码块2
}
if ...{
}else if ...{
}else{
}
switch 表达式{ // 不需要写break。switch后面可以不跟表达式
	case 表达式1,表达式2,...,: // 可以有多个表达式(包括常量、变量、函数、条件表达式)
		语句块1
		fallthrough  // 默认只能穿透下一个case语句
	case 表达式3,表达式4,...,: // case后面的表达式如果是常量值则要求不能重复，变量值重复检测不出来
		语句块2
	default:  // default非必须语句
		语句块
}
```
### 循环结构
```go
for i:=0; i < 10; i++{
	循环体
}

i := 0
for i <= 10{
	循环体
	i++
}

for{ // 是一个死循环，通常配合break使用
}

str = "abc"
for index, val := range str{
	循环体
}

break 可以通过标签指明跳出哪一层
label1:
for{
	label2:
	for{
		break label1
	}
}
```
go语言没有while和do while，可以使用for循环到达这个效果
### 随机数的生成
```go
import (
	"math/rand"
	"time"
)
rand.Seed(time.Now().Unix())
n := rand.Intn(100) // 生成[0,100)的随机整数
```
## 函数和方法
func 函数名(形式参数) (返回值列表){
...
}
### 包
- 本质就是创建不同的文件夹，可以用来引入别的包中的函数
- go的每一个文件都是属于一个包，go以包的形式来管理文件和项目目录结构
- 区分相同名字的函数变量、很好管理文件、控制函数变量的访问范围
- 在引入一个自己写的包的时候，引入包的路径应该写src开始的相对路径（go mod init 命令用于导入自己编写的其他包, go.mod文件应当建立在src路径下，之后会产生pkg文件夹，里面有自己编写的包对应的库文件）
- 在import包时，GOROOT没有找到时，编译器会自动从环境变量中GOPATH路径下src开始寻找
### 函数调用机制
- 栈区：基本数据类型一般说分配在这里
- 堆区：引用数据类型一般说分配在这里
- 代码区：代码存放在这里

1.数组和基本数据类型都是值传递。使用指针等才能实现引用传递。
2.但无论是值传递还是引用传递都是传递变量的副本（值副本、地址副本），一般来说引用传递效率高
3.go不支持传统的函数重载（即函数名一样但形参不一样）
```go
// go中函数也是一种数据类型，可以赋值给一个变量，可以通过该变量调用函数
func sum(n1 int,n2 int) int {
	return ni + n2
}
type myKind func(int, int) int // 给函数的数据类型重新命名
// 函数可以作为形参被调用
func myFun(myvar myKind, num1 int, num2 int) int{
	return myvar(num1, num2)
}

// go支持可变参数,表示有0到多个参数
func function(args...int) int{
	return int
}
func main(){
	a := sum
	res := a(19, 90)  //等价于sum(19, 90)
	b := myFun(sum, 10, 20) // 等价于sum(10, 20)	
}
```
### init函数
- 每一个源文件都可以包含一个init函数，该函数在main函数之前被调用，来完成一些初始化工作
- 一个文件同时含有全局变量定义、init、main，执行顺序从前到后
- 若在main.go中引入别的包中的内容，则执行顺序为：别包中的全局变量的定义->别包中init函数->main.go中的全局变量定义、init、main函数

**全局变量不能使用 := 的方式赋值，即不能使用类型推导赋值**
### 匿名函数
```go
// 求两个数的和 使用匿名函数方式实现
ans := func (n1 int, n2 int) int{
return n1 + n2
}(10, 20) // ans = 30

a := func (n1 int, n2 int) int{
return n1 - n2
}
res := a(20, 10) // res = 10, 且a可以被反复使用

// 还有全局匿名函数，... 脱裤子放屁。。
```
### 闭包
就是一个函数和与其相关的引用环境组合成的一个整体
```go
// 累加器
func AddUpper() func (int) int{ //AddUpper是一个函数，返回值的数据类型是func(int) int
	var n int = 10
	return func (x int) int{ // 返回的是一个匿名函数，但是这个匿名函数引用到了函数外的n
//因此这个匿名函数和n就构成了一个整体，构成闭包
		n = n + x
		return n
	}
}
// 要搞清楚闭包的关键就是要分析出返回的函数它引用到哪些变量
func main(){
	f := AddUpper()
	fmt.Println(f(1)) // 输出11
	fmt.Println(f(2)) // 输出13
	fmt.Println(f(3)) // 输出16
}
```
### 函数中的defer
defer在函数执行完毕后，及时地释放函数创建的资源
 - 当执行到defer语句时，暂时不执行，而是将defer后面的语句压入‘defer栈’
 - 当defer所在的函数执行完毕后，再从defer栈，按照先进后出的方式将语句出栈
 - defer语句入栈时，也会将相关的值拷贝同时入栈（值在defer外被修改不影响defer语句）
 - 一般用于关闭文件资源和释放数据库资源
### 字符串中常用系统函数
[Go语言标准库文档中文版 | Go语言中文网 | Golang中文社区 | Golang中国 (studygolang.com)](https://studygolang.com/pkgdoc)
 - len()  是内置函数
 - r := []rune(str)  // 处理含有中文字符的字符串str
 - 字符串转整数：n, err := strconv.Atoi("12")  // 当没错误时，输出err = nil
 - 整数转字符串：str := strconv.Itoa(12)
 - 字符串转[]byte(字符形式存储在数组中): var chars = []byte("hellp")
 - []byte转字符串：str = string([]byte{32, 43, 45, 54})
 - 10进制转2、8、16进制：str =strconv.FormatInt(123, 2)
 - 查找子串是否在指定的字符串中：b := strings.Contains("seafood", "foo") // 返回true或false
 - 统计一个字符串中有几个指定的字串：num := strings.Count("cheeze", "e") // 返回值是3
 - 不区分大小写的字符串比较：b := strings.EqualFold("abc", "ABC") // 返回True
 - 返回字串在字符串第一次出现的index值： index := strings.Index("NLY.abc", "abc")  // 返回值是4，若没有出现返回-1
 - 返回字串在字符串最后一次出现的index值： index := strings.LastIndex("gogolang", "go")  // 返回值是2，若没有出现返回-1
 - 将指定的字串替换成另外一个字串：str := strings.Replace("gogolanguage", "go", "北京”, 2) // 替换为"北京北京language", n表示替换n次，若为-1全部替换
 - 将字符串按照指定字符拆分成字符串切片：strArr := strings.Split("hello,go,ok", ",") // ["hello", "go", "ok"]
 - 将字符串字母进行大小写转换：str := strings.ToLower("ABC") // strings.ToUpper() 是转成大写
 - 将字符串两边空格去掉：str := strings.TrimSpace("  cd  ")  
 - 将字符串两边指定字符去掉：str := strings.Trim("! hell!o! ", " !")  // 返回 “hell!o”
 - strings.TrimLeft()  strings.TrimRight()
 - 判断字符串是否以指定字符串开头：b := strings.HasPrefix("ftp://", "ftp")   // 返回true
 - 判断字符串是否以指定字符串结尾：b := strings.HasPrefix("ftp.jpg", ".jpg")   // 返回true
 
 ### 时间和日期相关函数
 需要引入time包
 1.time.Time是一个类型，用于表示时间。now := time.Now() 获取当前时间，且now的type是time.Time
 2.now.Year()  now.Month()  now.Day() now.Hour() now.Minute() now.Second()
 3.格式化日期和时间：fmt.Printf()输出格式控制 
 fmt.Printf(now.Format("2006/01/02 15:04:05"))  这里的字符串里面的数字不能改变。。。
 4.休眠：func Sleep(d Duration)
 以下是时间常量
> const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)

5.获取当前时间戳或者纳秒戳：now.Unix()   now.UnixNano()  一般用于获取随机数
### 内置函数
len() delete()
cap()用来计算切片的容量
copy(slice1, slice2)将切片2数据拷贝到切片1
new():用来分配内存，主要用来分配值类型，返回的是指针
make():用来分配内存主要用来分配引用类型内存
### go的错误处理机制
1. defer + recover处理
详情文件位置：E:\goproject\src\go_code\project01\error
好处: 加入预警代码使程序更加健壮
2. 自定义错误并抛出：errors.New("错误说明")返回一个error类型的值， panic()能接收错误信息并打印出来且终止程序

# 数组和切片
数组是值类型，进行值传递
```go
// 四种初始化数组的方式
var numArr01 [3]int = [3]int{1, 2, 3}
var numArr02 = [3]int{1, 2, 3}
var numArr03 = [...]int{1, 2, 3}
var numArr04 = [...]int{1:800, 0:900, 2: 100}

// 数组的遍历
for index, value := range numArr01{
...
}
```
---
切片是引用类型，进行引用传递
```go
var a []int  // 声明方式

// 方式一 切片去引用一个已经创建好的数组
var intArr = [3]int{1, 2, 3}
// intArr[1:3] 表示slice引用到intArr这个数组，区间左闭右开
slice := intArr[1:3]
// 修改slice[0], intArr[1]的值会相应变化

// 第二种方式 make([]type, len, [cap])
var slice []int = make([]int, 4, 10)
// 第三种方式 与方式二类似
var slice []int = []int{4, 5, 6}
```
从底层来说 ，slice是一个结构体，包含三个属性（指针，长度，容量）
方式一和方式二创建slice的区别：方式一直接引用创建好的数组，程序员是可见的；方式二通过make创建切片和数组，这个数组对程序员不可见，只能通过切片访问和维护
方式二指定了切片的最大长度，如果元素数量大于len，那么会发生越界，只能通过append方式去添加元素

---
```go
// 切片添加元素
var slice1 []int = []int{0,  9, 8}
// 第一种方式
slice1 = append(slice1, 7, 6)
// 第二种方式 apend(被添加切片, 切片...)
slice1 = append(slice1, slice1...)
```
使用append之后go底层会创建新的数组，会将原切片的元素拷贝到新数组，新数组对程序员来说是不可见的。因此修改新数组的值原数组不会改变

---
也能获取字符串切片
```go
str := "abcabcabc"
slice := str[3:]  // slice 就等于 “abcabc”

// string是不可变的 str[0] = 'z'是错误的，但也能迂回改
arr1 := []byte(str)
arr1[0] = 'z'
str = string(arr1) // 此时，str = "zbcabcabc"
```
# map
### map声明
var 变量名 map[keytype]valuetype

 - keytype一般是int或string，key键值不能重复
 - valuetype通常为数字、string、map、struct ```map[string]map[string]string```，valuetype为映射时一般这样定义
 - 声明不会分配内存，初始化需要make才能分配内存 ``` a := make(map[string]string, 10)```10表示a的长度,可省略
 - ```hero := map[string]string{"hero1" : "ming", "hero2" : "hong", }```最后一个逗号不能省略
 ### map增删改查
 - map[key] = value 如果key不存在，就增加；如果存在就修改value
 - delete(map, key) key存在则删除，不存在不报错
 - ```value, ok := map(key)```如果key在映射中ok值为true，如果不存在ok值为false
 - map遍历只能用for range来遍历
 ### map切片
 切片的数据类型是map，我们称为map切片, 这样使用使得map的个数可以动态变化
 ```go
 // 1.声明一个map切片
var monster []map[string]string
// 2.给切片申请存储空间
monster = make([]map[string]string, 2)
// 3.增加妖怪信息 注意这里省略了make(map[string]string)，事实上map也需要申请内存
monster[0] = map[string]string{"name" : "monkey", "age" : "500",}
monster[1] = map[string]string{"name" : "redchild", "age" : "100",}

// 因为只给切片申请了2个单位大小，我们想让monster能动态增加 需要使用append
monster = append(monster, map[string]string{"name":"rabit", "age": "400",})
```

### map使用细节
1. map是引用类型，遵循引用传递机制
2. map能自动扩容，即使设置了map的最大个数，超出后也不会产生panic
3. map的value最好使用结构体
4. ```map[string]map[string]string```，map嵌套map，里层的map也要make后才能使用

# 面向"对象" 结构体
### 结构体的声明与赋值
五种方式，后三种为创建结构体指针
```go
var cat1 Cat
cat1.Name = "bai"
cat1.age = 3
cat1.Color = "black"

var cat2 Cat = Cat{Name : "xiao", age : 10, Color : "yellow",}

var cat3 *Cat = new(Cat)
// (*cat3).Name = "doudou"是标准写法，但为了方便
// 等价于 cat3.Name = "doudou"
cat3.Name = "doudou"

var cat4 *Cat = &Cat{Name:"hei",age:3, Color:"white"}

var cat5 = &Cat{"luo", 4, "blue"}
```
### 实际开发序列化结构体 结构体标签使用场景
struct每个字段上可以写上一个tag，该tag可以通过反射机制获取，常见的使用场景就是序列化和反序列化
```go
package main
import (
	"fmt"
	"encoding/json"
)

type Monster struct{
// `json:"name"` 就是struct tag
// 使输出的json字符串字段名是小写
	Name string `json:"name"`
	Age int `json:"age"`
	Skill string `json:"skill"`
}

func main(){
	// 创建monster变量
	monster := Monster{"牛魔王", 500, "芭蕉扇"}
	// 将monster变量序列化为json字符串
	// json.Marshal 函数使用反射 之后介绍
	jsonstr, err := json.Marshal(monster)
	if err == nil{
		fmt.Println("转化后的json串为", string(jsonstr))
	}
}
```
### 方法
1.自定义类型都可以有方法，不只是struct
2.方法的声明与调用
```go
type A struct{
	Num int
}
// 结构体A有一个方法 名为test
// func (a *A) test() {}执行速度更快，无需进行变量的值拷贝
func (a A) test(){
	fmt.Println(a.Num)
}
func main(){
	var b A
	b.Num = 10
	b.test() // 调用方法
```

3.方法只能被绑定类型的变量来调用，不能直接被调用 
4.如果一个类型实现了String()这个方法，**那么fmt.Println默认会调用这个变量的string()进行输出**
### 方法和函数的区别
1.调用方式不一样：函数名(实参列表)；变量.方法名(实参列表)
2.对于函数参数为值类型只能传递值类型，参数为引用类型只能传递引用类型
3.对于方法，被结构体变量调用时，该调用变量既可以是值传递也可以是引用传递，方法是否改变变量内部的值应该看 **当初方法在定义时** **绑定的结构体类型**到底是值类型还是引用类型
### 工厂模式**
结构体名->结构体字段->结构体方法

使用场景：
1.我们定义的结构体名称是小写，但是我们需要在别的包中创建该结构体的实例，可以使用工厂模式**建立函数**解决
```go
// 在定义结构体的包中 设置这样一个工厂函数
func NewStudent(n string, s float64) *student{
	return &student{
		Name : n,
		score : s,
	}
}
```
2.我们定义的结构体名称是小写，其中有的字段也是小写，我们需要在别的包中创建该结构体的实例并访问小写字段，可以使用工厂模式**建立方法**解决
```go
func (s *student) GetScore() float64{
	return (*s).score
}
```
main函数调用如下
```go
func main(){
	var stu = model.NewStudent("Jack", 90.0)
	fmt.Println(stu)
	fmt.Println("name =", stu.Name, "score =", stu.GetScore())
}
```
# 面向对象下
多态 ：通过接口来实现多态
### 封装
- 把抽象的字段和对字段方法封装在一起
- 字段、方法首字母小写，此时要额外设置Setxx或者Getxx方法来获得
- E:\goproject\src\go_code\oop\encapsulate\main 封装的简单示例

### 继承
- 解决代码复用问题，提高代码的扩展性和可维护性
- 通过匿名结构体实现继承特性
- 同一包中 结构体可以使用匿名结构体的所有字段和 属性，无论大小写
- 如果结构体和匿名结构体有相同的字段或者方法时，编译器会采用就近访问原则，如果希望访问匿名结构体里面的字段和方法，可以通过匿名结构体名来访问
- 结构体内部有两个匿名结构体，若两个匿名结构体有相同的字段和方法（而结构体本身没有同名的字段和方法），在访问时就必须指明匿名结构体名，否则报错
- 如果结构体嵌套了一个有名结构体，在访问有名结构体的字段时就必须带上名字。此时，**该关系名为组合关系**
- 匿名结构体可以**以指针的方式被嵌套**。。。
E:\goproject\src\go_code\oop\extends
### 接口——Golangte特色
高内聚，低耦合，多态
实现接口就是实现接口里面所有方法，接口无需显示实现
接口里的所有方法都没有方法体
- 接口本身不能创建实例，但是**可以指向一个实现了该接口的自定义类型的变量**
- 只要是自定义类型都可以实现接口，不一定非得是结构体
- 接口可以继承多个别的接口，必须实现继承的接口和本接口的所有方法才能算实现该接口
- interface默认是一个指针（引用类型）
- 空接口没有方法，所有类型都实现了空接口，即可以**把任何一个变量赋给空接口类型**

### 接口 VS 继承
1. 接口是对继承的补充
2. 接口保障规范性
3. 继承主要是解决代码的复用性和可维护性；接口价值主要在于设计各种规范
4. 接口（like a）比继承（is a）更灵活更松散
5. 接口实现代码解耦
### 多态
多态特征通过接口实现
1. 多态参数：computer函数参数是usb接口；可以接受手机变量和相机变量。详细见：E:\goproject\src\go_code\oop\interface
2. 多态数组：在usb数组中可以同时存放phone结构体和camera结构体
### 类型断言
以下面为例：类型断言表示判断a是否指向Point类型的变量，如果是就转成Point类型并赋值给b，下划线对应位置为true。
若转换失败，因为采用的是安全类型断言，不会抛出panic，下划线对应位置为false
```go
type Point struct{
	x int
	y int
}
func main(){
	var a interface{}
	var point Point = Point{1, 2}
	a = point  // ok
	var b Point
	b, _ := a.(Point) // 进行安全类型断言
	fmt.Println(b)
}
```

> E:\goproject\src\go_code\oop\assert
```go
// 循环判断传入的参数是什么类型
func TypeJudge(items... interface{}){
	for i, v := range items{ 
		switch x.(type){ // 进行类型断言
		case bool:
			fmt.Printf("%v, %v", i, v)
		case float32:
			...
		case float64:
			...
		case int, int32, int64:
			...
		default:
			...
		}
	}
}
```
# 文件操作
### 常用文件操作函数和方法
- 打开关闭文件
```go
import "os" // 引入os包
// 打开文件 file叫文件指针|文件对象|文件句柄
file, err := os.Open("d:/test.txt")
if err != nil{
	fmt.Println(err)
}
// 输出file 发现看到一个地址
fmt.Printf("%v", file)

// 关闭文件 经常打开文件后使用defer
// defer file.Close()
err2 := file.Close()
if err2 != nil{
	fmt.Println(err2)
}	
```
- 读取文件
```go
// 1.读取文件内容并输出在终端（带缓冲区方式）
// 创建一个 *Reader 是带缓冲的
// 默认缓冲区是4096
reader := bufio.NewReader(file)
str, err := reader.ReadString('\n') // 读到换行符停止读

// 2.读取文件内容并输出在终端,使用io.ioutil.ReadFile一次将整个文件读入到内存，适用于文件不太大的情况
// 这时不需要打开和关闭文件 文件的Open和Close都封装在该函数里面
func ReadFile(filename string) ([]byte, error)
content, err3 := ioutil.ReadFile(filename)
fmt.Printf("%v\n", content) // 全是数字，因为content是[]byte
fmt.Printf("%v\n", string(content))
```
- 创建文件并写文件
flag 的常用取值
```go
const (
    O_RDONLY int = syscall.O_RDONLY // 只读模式打开文件
    O_WRONLY int = syscall.O_WRONLY // 只写模式打开文件
    O_RDWR   int = syscall.O_RDWR   // 读写模式打开文件
    O_APPEND int = syscall.O_APPEND // 写操作时将数据附加到文件尾部
    O_CREATE int = syscall.O_CREAT  // 如果不存在将创建一个新文件
    O_EXCL   int = syscall.O_EXCL   // 和O_CREATE配合使用，文件必须不存在
    O_SYNC   int = syscall.O_SYNC   // 打开文件用于同步I/O
    O_TRUNC  int = syscall.O_TRUNC  // 如果可能，打开时清空文件
)
```
E:\goproject\src\go_code\writefile
```go
// FileMode在Windows下不发挥作用
func OpenFile(name string, flag int, permn FileMode) (file *File, err error)

filename := "e:/abc.txt"
file, err := os.OpenFile(filename, os.O_WRONLY|os.O_CREATE, 0666)
if err != nil {
	fmt.Println(err)
	return
}
defer file.Close()
str := "hello, God\n"
// 写入时使用带缓存的 *Writer
write := bufio.NewWriter(file)
for i := 0; i < 5; i++ {
	write.WriteString(str)
}
// 因为write是缓存，并没有真正写入磁盘
// 所以需要Flush方法将缓存内容冲刷到磁盘
write.Flush()
```
- 写覆盖已经存在的文件（将上段代码os.O_CREATE删除）
- 追加内容到原来的文件（os.O_WRONLY | os.O_APPEND）
- ioutil.ReadFile()      ioutil.WriteFile()

---
- **判断文件/目录是否存在** , 使用os.Stat()函数返回的错误值进行判断
```go
func PathExists(path string)(bool, error){
	_, err := os.Stat(path)
	if err == nil{// 返回错误为nil 说明文件/目录存在
		return true, nil
	}else if os.IsNotExist(err){//返回错误类型使用os.IsNotExist()判断为true 说明文件不存在
		return false, nil
	}else{ // 返回为其他类型说明不确定文件是否存在
		return false, err
	}
}
```
- 拷贝文件 io包中 func Copy(dst Writer, src Reader) (written int64, err error)
```go
func CopyFile(srcPath string, dstPath string) (int64, error) {
	srcFile, err01 := os.Open(srcPath)
	if err01 != nil{
		fmt.Printf("err01 is %v\n", err01)
	}
	defer srcFile.Close()
  
	dstFile, err02 := os.Create(dstPath) // 使用OpenFile会不成功
	if err02 != nil{
		fmt.Printf("err02 is %v\n", err02)
	}
	defer dstFile.Close()

	return io.Copy(dstFile, srcFile)
}
``` 
## 命令行参数
### 1. 获取命令行输入的各种参数(原始)
os.Args是一个string切片，用来存储所有命令行参数。Args保管了命令行参数，第一个是程序名。
E:\goproject\src\go_code\project02\argsdemo
```go
--mian.go--
func main() {
	fmt.Println("命令行的参数有", len(os.Args))
	for i, v := range os.Args {
		fmt.Printf("args[%d] = %v\n", i, v)
	}
}
// 程序输出:
// args[0] = main.exe
// args[1] = 参数1
// args[2] = 参数2
// args[3] = ...
```
在命令行 go build -o main.exe main.go
再输入 main.exe 参数1 参数2 ...
### 2.flag包解析命令行参数
flag解析带有指定参数的命令行
仍然需要先生成exe文件
E:\goproject\src\go_code\project02\flagdemo\main.go
```go
// &user 接收用户命令行输入的 -u 后面的参数值
// "u" 指定 -u 参数
// "" 是默认值 如果没有接收到参数user默认为""
// "用户名默认为空字符串" 是说明信息
flag.StringVar(&user, "u", "", "用户名默认为空字符串")
flag.StringVar(&pwd, "pwd", "", "密码默认为空字符串")
flag.StringVar(&host, "h", "localhost", "主机名默认为localhost")
flag.IntVar(&port, "p", 3304, "端口号默认为3304")

flag.Parse() // 必须调用该方法才能从os.Args中取出参数
fmt.Printf("user = %v, pwd = %v, host = %v, port = %v",
user, pwd, host, port)
```
## json
json：轻量级的**数据交换格式**，易于机器解析和生成，提高网络传输效率。
Golang数据 ——**序列化**——> json字符串 ——网络传输——> 程序 ——**反序列化**——> 其他语言
[{"key1" : value1, "key2" : value2}, {"key1" : value1, "key2" : value2}]
**json数据解析：**https://www.json.cn/ 确定json格式是否正确
### 序列化
json序列化：将key-value结构的数据类型序列化为json字符串的操作
**encoding/json包**
 - 结构体序列化
```go
func testStruct() {
	monster := Monster{"牛魔王", 500, "1500-12-12", 10000.0, "开辟"}
	// 将monster序列化 Marshal() data是[]byte
	data, err01 := json.Marshal(&monster)
	if err01 != nil {
		fmt.Println("失败")
	}
	fmt.Printf("monster 序列化后为: %v", string(data))
}
```
 - map序列化E:\goproject\src\go_code\project02\json\serial\main.go
 - 切片序列化E:\goproject\src\go_code\project02\json\serial\main.go
 - 普通数据类型序列化(输出仍是这个数据)
```go
func testInt() {
	a := 10
	// 将monster序列化 Marshal() data是[]byte
	data, err01 := json.Marshal(a)
	if err01 != nil {
		fmt.Println("失败")
	}
	fmt.Printf("monster 序列化后为: %v", string(data))
}
```
### 反序列化
反序列化：将json字符串还原成原先的数据类型
json.Unmarshal([]byte(str), &xx)后一个参数要传入一个指针，无论是变量是否是值类型
E:\goproject\src\go_code\project02\json\unserial\main.go
 - 结构体反序列化
```go
func unMarshalStruct() {
	str := `{"Name":"牛魔王","Age":500,"Birthday":"1500-12-12","Salary":10000,"Skill":"开辟"}`
	var monster Monster
	err := json.Unmarshal([]byte(str), &monster)
	if err != nil {
		fmt.Println("失败")
	}
	fmt.Println(monster)
}
```
 - map反序列化
```go
str := `{"add":"chongqing","age":20,"name":"yellowboy"}`
var m map[string]interface{}
// 反序列化无需make unMarshal底层自动make
err := json.Unmarshal([]byte(str), &m)
if err != nil {
	fmt.Println("失败")
}
fmt.Println(m)
```
 - 切片反序列化
```go
func unMarshalSlice() {
	str := `[{"add":"chongqing","age":20,"name":"redboy"},{"add":"chongqing","age":10,"name":"blueboy"}]`
	var s []map[string]interface{}
// 无需make
	err := json.Unmarshal([]byte(str), &s)
	if err != nil {
		fmt.Println("失败")
	}
	fmt.Println(s)
}
```
## 单元测试
 - go自带一个轻量级测试框架testing，自带的go test 命令能实现单元测试和性能测试。go test -v无论运行正常还是错误都输出日志信息
 - 要编写一个新的测试程序，需要创建一个名称以 _test.go 结尾的文件，该文件包含 `func TestXxx(*testing.T) `函数。 将该文件放在与被测试的包相同的包中。该文件将被排除在正常的程序包之外，但在运行 “go test” 命令时将被包含。
 -  func (*T) Fatalf(string)   func (*T) Logf(string)
 - 测试单个文件：go test -v **xx_test.go xx.go** 
 - 测试单个方法：go test -v **-test.run** 方法名
 
E:\goproject\src\go_code\project02\testingdemo
# goroutine协程和Channel管道
Go主线程(main, 重量级的耗费cpu资源，物理线程)：可以发起上万个协程goroutine（轻量级线程逻辑态）

### goroutine特点：
有独立的栈空间；共享程序堆空间；调度由用户控制；协程是轻量级线程 
### goroutine调度模型MPG
M:操作系统的主线程（物理线程）
P:协程需要的上下文
G:协程
获取当前逻辑CPU个数：runtime.NumCPU()
设置运行该程序的CPU个数：runtime.GOMAXPROCS(num)
**go build -race main.go**编译生成的可执行文件在协程有竞争时能一直执行，并且最后告知存在多少资源竞争问题
### goroutine捕获panic
使用recover捕获goroutine可能出现的panic，使其不会影响主线程和其他协程执行
```go
defer func() {
	if err := recover(); err != nil{
		fmt.Println(err)
	}()
```
### 不同协程间如何通信

 - 共享资源加锁
 ``` go
// 声明一个全局的互斥锁
var lock sync.Mutex
func test(){
	lock.Lock() // 对共享资源m加锁
	m[i] = i
	lock.Unlock() // 解锁
}
```
 - 管道通信
 
 ### 管道
  管道是以队列形式存取数据
  ```go
	// 创建一个可以存放三个int类型数据的管道，3 表示容量，不能超过
	var intChan chan int
	intChan = make(chan int, 3)

	// 向管道内添加数据, 若添加数据超出容量会报告deadlock
	intChan <- 10
	num := 22
	intChan <- num

	// 从管道内取出数据
	// 在没有协程的情况下如果管道内没有数据并且再取就会deadlock
	var num2 int
	num2 = <-intChan
```

```go
	// 设置一个可以存放任意数据类型的管道
	allChan := make(chan interface{}, 3)
	allChan <- 10
	allChan <- "tom"
	cat := Cat{"hei", 12}
	allChan <- cat

	// 取出前两个数据扔掉
	<-allChan
	<-allChan

	// 取出cat赋值给newCat
	// 此时newCat的类型在编译层面是interface{}
	// 但在运行层面打印出newCat的类型是Cat
	newCat := <-allChan
	// newCat.Name 写法错误 会在编译阶段出错
	// 使用类型断言
	a := newCat.(Cat)
	fmt.Println(a.Name)
```
### channel的关闭
使用内置函数close()关闭管道; 关闭管道后只能读不能写；关闭管道后不能再次关闭管道

> v, ok := <- channel

当数据被取完后继续执行该语句，如果管道已经关闭，ok = false
如果管道没有关闭，会抛出deadlock，可以用select解决
```go
for{
	select{ // 管道没有关闭取不到数据也不会死锁，而是到下一个case里面执行，直到default
		case v := <- intChan :
			fmt.Println(v)
		case v := <- stringChan :
			fmt.Println(v)
		default : 
			跳出循环对应代码
	}
}
```
### channel的遍历
使用for range 遍历已关闭的管道，如果不关闭会出现deadlock
### channel的阻塞
 - 编译器发现一个管道只有写，没有读，当管道满后会发生deadlock
 - 编译器发现一个管道只有读，没有写，当管道空后会发生deadlock
 - 如果一个管道有读有写，但读频率低于写频率，无事发生，只是写管道的协程在管道满时会发生阻塞
 - 读频率高于写频率，无事发生，只是读管道的协程在管道空时会发生阻塞
### 管道声明三种类型：
可读可写：var chan1 chan int
只写：var chan2 chan<- int
只读：var chan3 <-chan int
一般只读只写是非主函数调用时管道参数设置，防止对管道误操作
# 反射
**reflect包**实现了运行时反射，允许程序操作任意类型的对象。
反射可以在运行时获取变量的各种信息；如果是结构体变量可以获取到结构体本身的字段方法；通过反射可以修改变量的值

- reflect.TypeOf(变量名)获取变量的类型，返回reflect.Type类型
- reflect.ValueOf(变量名)获取变量的值，返回reflect.Value类型；Value是一个结构体类型，有很多属性和方法

### 变量、interface{}和reflect.Value 是可以相互转换的

> b := interface{}

- 将interface{}转成reflect.Value

> rVal := reflect.ValueOf(b)

- 将reflect.Value转成interface{}

> iVal := rVal.interface()

- 将interface{}转成原变量的类型 使用类型断言

> v := iVal.(变量类型)

总结：变量<---->interface{}<---->reflect.Value
### 反射细节说明
- reflect.Value.Kind()reflect.Type.Kind()可以获取变量类别
- kind是类别 type是类型 type划分更细致一点
- 通过反射修改变量，使用SetXxx()方法时需要通过对应指针类型来完成，同时需要使用reflect.Value.Elem()方法
- Elem()返回v持有的接口保管的值的Value封装，或者v持有的指针指向的值的Value封装。
- Value.NumField() Value.Field(i) Type.Field(i).Tag.Get() Value.NumMethod() Value.Method(i).Call(params)
```go
func TestStruct(b interface{}) {
	typ := reflect.TypeOf(b)
	val := reflect.ValueOf(b)
	kd := val.Kind()
	if kd != reflect.Struct {
		fmt.Println("expect struct")
		return
	}
	// 获取该结构体有几个字段
	num := val.NumField()
	fmt.Println(num)
	// 遍历该结构体的所有字段
	for i := 0; i < num; i++ {
		//  val.Field(i) 返回结构体的第i个字段的值 值仍然是reflect.Value类型
		fmt.Printf("Feild %d: 值为=%v\n", i, val.Field(i))

		// 获取struct标签 需要通过reflect.Type来获取标签的值
		// typ.Field(i) 返回第i个字段的tag标签 标签是reflect.StructField类型
		tagValue := typ.Field(i).Tag.Get("json") // 对于没有json注释返回""
		if tagValue != "" {
			fmt.Printf("Field %d: tag = %v\n", i, tagValue)
		}
	}

	// 获取结构体有几个方法
	numOfMethod := val.NumMethod()
	fmt.Printf("struct have %d methods\n", numOfMethod)

	// 获取到结构体的第二个方法 并且调用执行
	// 方法的排序默认按照函数名的ASCII比较
	// 所以此处调用的方法是Print()
	val.Method(1).Call(nil)

	// 调用GetSum()方法
	var params []reflect.Value
	params = append(params, reflect.ValueOf(10), reflect.ValueOf(20))
	res := val.Method(0).Call(params) // 返回值是[]reflect.Value
	fmt.Println("res = ", res[0].Int())
}
```
```go
// 前提传入该函数的参数是结构体的地址
func TestStruct(b interface{}) {
val := reflect.ValueOf(b)
val.Elem().Field(0).SetString("白象精")
}
// 上述函数完成了对原先结构体第一个字段值的改变
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzODQ2MjcyODIsLTIxMDg1ODI1MjgsOT
gxNDIwODYsMTU3ODM5NDU0NSwyMjcxMTgyMjIsLTE3MTYwMDkw
MTAsLTI0NDc0MTEzMCwxMzc3NzIzNjYxLC0xOTk4MjEzNDg5LC
01NDY1ODk2NzEsLTc1NDc1OTE2MywtNTAxNjYwOTc5LDIwNzEx
MjI4NjksNDgxOTE0OTExLDEyODUyMTQ1MTgsLTE4MTA3MTkxNT
gsMjAzNzY4Mjk3NiwxMTg0NDcyODg5LDEwNTAzMzcyOTgsLTUw
Nzg4MzU2NV19
-->