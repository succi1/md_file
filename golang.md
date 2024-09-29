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
以下面为例：类型断言表示判断a是否指向Point类型的变量，如果是就转成Point类型并赋值给b，否则报错panic
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
	b ：= a.(Point) // 进行类型断言
	fmt.Println(b)
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA3MDI5MDMyMSwxMDA4MDkzNzE1LDQ0Nj
k2ODM3NCwtMTczODk3NDQ3MCwxMTc3MDAyODI1LC0xMjA4MTE2
OTczLC0xODQyNjgzMzM4LC0xNDU0MDc4NzksMjAwOTk2NjU3My
wtMjA3NjMyMzUwNCwtMTAyMjg5MTI4MSwxMjYwNTgyNDg0LDE3
MjI0NzM3OTAsLTE4NjAwMTk4OTIsMzYyMDA4NjI0LDczODAyND
kxNSwtNzEyOTQ5NzMzLC03NzgzMTYxODgsLTUyOTY0Mjk5Mywy
OTQ2NjMwNThdfQ==
-->