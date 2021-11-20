
### 变量和作用域

可以使用短声明替代原有声明，且可作用于原先不可使用var声明的地方

var定义再func外，属于package级别的作用域变量，且短声明不可用于此处

声明浮点型时默认为`float64`

Golang每个类型都有一个默认值，被称作零值

#### %f格式化动词

由两部分组成

- 宽度：显示出最少的字符个数，不足补空格/零（包含小数点和小数）
- 精度，小数点后面的位数

浮点类型精度： 为了尽量最小化舍入误差，建议先做乘法后做除法

使用%T, 可以打出数据类型

从整数环绕角度理解补码有点意思：`255+1 => 0 uint8`; `127+1 => -128 int8`

整型最大值和最小值：`math.MaxInt16`;`math.MinInt64`


一般对`var distance = 24e18`，默认为`float64`类型

大数存储`big package`:`big.Int`, `big.Float`类型，及一些函数如`newInt`, `Div`等；一般可使用字符串进行初始化，并可传入进制

Go语言里常量可以是`untyped`无类型类型的，如`untyped int`


### 多语言文本

unicode可用`rune`类型表示，其时`int32`的一个类型别名
byte时`uint8`的类型别名，可表示ASCII
自定义语法如`type rune = int32`