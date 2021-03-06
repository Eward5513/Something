# 第一章
* Go语言就是为了解决当下编程语言对并发支持不友好，编译速度慢，编程复杂这三个问题而诞生的
* 函数以func开头，函数体开头的"{"必须在函数头所在行尾部，不能单独起一行
* Go的标识符构成规则时：开头一个字符必须是字母或者下划线，后面跟任意多个字符数字或者下划线，并且区分大小写
* 内置20个预声明数据类型标识符
    1. 整型（16个）：byte int uint8 int16 int32 int64 uint uint8 uint16 uint32 uint64 uintprt
    2. 浮点型（2个）：float32 float64
    3. 复数型（2个）：complex64 complex32
    4. 字符和字符串型（2个）：string rune
    5. 接口型（1个）：error
    6. 布尔型（1个）：bool
* Go是一种强类型静态编译语言，在声明初始化内置类型变量时，Go可以自动地进行类型推导
* 常量标识符（4个）：true false iota nil 空白标识符（1个）：_
* 操作符
    1. 算数运算符（5个）：+ - * / %
    2. 位运算符（6个）：& |  ^ &^ >> << 。&^是与非操作，a&^b会将a中对应b的每一位如果为1则置0，即b==5时会将a的最低位和第三位置0，因为b为101
    3. 赋值和赋值复核运算符（13个）：:= = += -= *= /= %= &= |= ^= &^= >>= <<=
    4. 比较运算符（6个）：> >= < <= == !=
    5. 括号（6个）：（）{} []
    6. 逻辑运算符（3个）：&& || !
    7. 自增自减运算符（2个）：++ -- Go中的自增自减操作符是语句而不是表达式
    8. 其它运算符（6个）： : , ; . ... <-
* Go不支持用户定义的字面量
* 浮点型字面量支持两种格式：标准的数学记录法和科学计数法
* Go的变量声明后就会立即为其分配空间，如果不指定初始值则将该变量初始化为类型的零值
* :=声明只能出现在函数内（包括方法内）
* Go语言提供自动类型管理，不需要特别关注变量的生存期和存放位置，编译器使用栈逃逸技术能够自动为变量分配空间，可能在栈上也可能在堆上
* Go内部使用统一的命名空间对变量进行管理，每个变量都有一个唯一的名字，包名是这个名字的前缀
* Go中常量分为布尔型，字符串型和数值型常量，常量存储在程序的只读段里
* iota用在常量声明中，初始值为0，一组多个常量同时声明时其值逐行增加，分开的const语句，iota每次都从0开始
* 比较表达式和逻辑表达式的结果都是布尔类型数据，if和for语句的条件部分一定是布尔类型的值或者表达式
* byte是uint8的别名，不同类型的整型必须进行强制类型转换
* 浮点数字面量被自动类型推断为float64类型，两个浮点数之间不应该使用==或者!=进行比较操作，高精度科学计算应该使用math标准库
* 两种复数类型 complex64和complex128，complex64由两个float32组成，complex128由两个float64组成，有三个内置函数处理复数
```go
var v = complex(2.1,3)//构造一个复数
a := real(v)//返回实部
b := image(v)//返回虚部
```
* 字符串是常量，可以通过类似数组的索引访问其字节单元，但是不能修改某个字节的值
* 字符串转化为切片[]byte时每转换一次都需复制内容
* 基于字符串创建的切片和原字符串指向相同的底层字符数组，一样不能修改，对字符串的切片操作返回的子串仍然是string，而非slice
* 字符串可以转换为字节数组，也可以转换为Unicode的字数组
```go
a := "hello"
b :=[]byte(a)
c :=[]rune(a)
```
* Go内置两种字符类型，一种是byte字节类类型，byte是uint8的别名。另一种是表示Unicode编码的字符rune, rune在Go内部是int32类型的别名，占用4个字节，Go语言默认的字符编码就是UTF-8类型的
* Go语言基本的复合数据类型有指针，数组，切片，字典，通道，结构和接口
* Go不支持指针运算，因为要支持垃圾回收
* 函数允许返回局部变量的地址
* 数组的初始化
```go
a :=[...]int{1,2,3}//不指定长度，但是后面的初始化列表数量来确定其长度
a :=[3]int{1:1,2:2}//指定总长度，并通过索引值进行初始化
a :=[...]int{1:1,2:2}//不指定总长度，通过索引值初始化
```
* 关于数组
    1. 数组创建完长度就固定了，不可再追加元素
    2. 数组是值类型的，数组赋值或作为函数参数都是值拷贝
    3. 数组长度是数组类型的组成部分，[10]int和[20]int表示不同的类型
    4. 可以根据数组创建切片
* Go为切片维护三个元素，指向底层数组的指针，切片的元素数量和底层数组的容量
* 切片可由数组创建，也可通过内置函数make创建切片，由make创建的切片各元素被默认初始化为切片元素类型的零值
* 切片支持len()返回切片长度 cap()返回切片底层数组容量 append()对切片追加元素 copy()用于复制一个切片
* map也是一种引用类型，可由字面量创建，也可用内置的make函数创建，可用range遍历map，但是不保证每次迭代元素的顺序，可用len()返回map中键值对的数量
* map不是并发安全的，不要直接修改value中某个元素的值，修改完成后需要整体赋值
* struct结构中的类型可以是任何类型，struct的存储空间是连续的
* switch后面可以跟一个可选的简单初始化语句，条件表达式的值可以是任意支持相等比较运算的类型变量，可以通过fallthrough语句来强行执行下一个case语句，不用判断下一个case自居的条件是否满足
* switch中的default语句可以放到任何位置
```go
switch{
case score >=90:
        grade = 'A'
case score >=80:
        grade = 'B'
default:
        grade = 'F'
}
```
* Go支持三种循环语句
```go
for init;condition;post{}
for condition{}
for {}
```
* break用于函数内跳出for,switch,select语句的执行
# 第二章
* 函数是一种类型，函数类型变量可以向其他类型变量一样使用，可以作为其他函数的参数或返回值，也可以直接调用执行，支持多值返回，支持闭包，支持可变参数
* 函数名遵循标识符的命名规则，首字母的大小写决定该函数在其他包的可见性，大写时其他包可见，小写时只有相同的包可以访问
* 函数支持有名的返回值，参数名就相当于函数体内最外层的局部变量，命名返回值会被初始化为类型零值，最后的return可以不带参数名直接返回
* 如果多值返回有错误类型，则一般将错误类型作为最后一个返回值
* Go函数实参到形参的传递永远是值拷贝
* 不定参数
    1. 所有的不定参数类型必须是相同的
    2. 不定参数必须是函数的最后一个参数
    3. 不定参数在函数体内相当于切片，对切片的操作同样适合对不定参数的操作
    4. 切片可以作为参数传递给不定参数，切片名后要加上"..."
    5. 形参为不定参数的函数和形参为切片的函数类型不同
* 函数类型即函数签名，一个函数的类型就是函数定义首行去掉函数名，参数名和{，可以使用fmt.Printf("%T")打印函数的类型
* 两个函数类型相同的条件是拥有相同的形参列表和返回值列表，列表的次序个数和类型都相同
* 可以使用type定义函数类型，函数类型变量可以作为函数的参数或者返回值
* 有名函数可以看做函数类型的常量，可以直接使用函数名调用参数，也可以直接赋值给函数类型变量
* Go有两种函数，有名函数和匿名函数。匿名函数可以看做函数字面量，所有直接使用函数类型变量的地方都可以由匿名函数代替。匿名函数可以直接赋值给函数变量，可以当做实参也可作为返回值
* defer可以注册多个延迟调用，这些调用以先进后出的顺序在函数返回前被执行
* defer后面必须是函数或方法调用，defer函数的实参在注册时通过值拷贝传递进去，注意是在注册时
* defer语句必须先注册后才能执行，如果defer位于return之后，则defer因为没有注册，不会执行
* 主动调用os.Exit(int)退出进程时，defer将不再被执行
* 一般defer语句放在错误检查语句之后，尽量不要放在循环语句里面
* 闭包=函数+引用环境
    1. 闭包对闭包外的环境引入是直接引用
    2. 多次调用函数，返回的多个闭包所引用的外部变量是多个副本，原因是每次调用函数都会为局部变量分配内存
    3. 用一个闭包函数多次，如果该闭包修改了其引用的外部变量，则每一次调用该闭包对该外部变量都有影响，因为闭包函数共享外部引用
    4. 如果一个函数调用返回的闭包引用修改了全局变量，则每次调用都会影响全局变量
    5. 对象是附有行为的数据，而闭包是附有数据的行为
* panic和recover
    1. 发生panic之后，程序会从调用panic的函数位置或发生panic的地方立即返回，逐层向上执行函数的defer语句，然后逐层打印函数调用堆栈，直到被recover捕获或运行到最外层函数而退出
    2. panic的参数是一个空接口类型interface{}，所以任意类型的变量都可以传递给panic。调用panic的方法非常简单,panic(xxxx)
    3. panic不但可以在函数正常流程中抛出，在defer逻辑里也可以再次调用panic或抛出panic，defer里面的panic能够被后续执行的defer捕获
    4. recover()只有在defer后面的函数体内被直接调用才能捕获panic终止异常，否则返回nil，异常继续向外传递。注意是函数体内直接调用，defer recover()不行 defer func(){func(){recover()}()}()也不行 
    5. 可以有连续多个panic被抛出，连续多个panic的场景只能出现在延迟调用里面，否则不会出现多个panic被抛出的场景。但只有最后一次panic能被捕获
    6. 包中init函数引发的panic只能在init函数中捕获，在main中无法被捕获，原因是init函数先于main执行，函数并不能捕获内部新启动的goroutine所抛出的panic
* Go提供了两种处理错误的方式，一种是借助panic和recover的抛出捕获机制，另一种是使用error错误类型
* 函数的多值返回是主调函数预先分配好空间来存放返回值，被调函数执行时将返回值复制到该返回位置来实现的
