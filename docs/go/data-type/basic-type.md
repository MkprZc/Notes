## 基本类型介绍

|类型	        |长度(字节)	|默认值	|说明 |
| :---          | ---       | ---   | --- |
|bool	        |1	        |false	||
|byte	        |1	        |0	    |uint8|
|rune	        |4	        |0	    |Unicode Code Point, int32|
|int, uint	    |4或8	    |0	    |32 或 64 位：int和uint的大小和系统有关，32位系统表示int32和uint32，如果是64位系统则表示 int64和uint64|
|int8, uint8	|1	        |0	    |-128 ~ 127, 0 ~ 255，byte是uint8 的别名|
|int16, uint16	|2	        |0	    |-32768 ~ 32767, 0 ~ 65535|
|int32, uint32	|4	        |0	    |-21亿~ 21亿, 0 ~ 42亿，rune是int32 的别名|
|int64, uint64	|8	        |0	    ||
|float32	    |4	        |0.0    |小数位精确到7位|
|float64	    |8	        |0.0    |小数位精确到15位|
|complex64	    |8		    |       ||
|complex128	    |16		    |       ||
|uintptr	    |4或8		|       |以存储指针的 uint32 或 uint64 整数|
|array		    |	        |       |**值类型**|
|struct		    |	        |       |**值类型**|
|string		    |           |""	    |UTF-8 字符串|
|slice		    |           |nil	|**引用类型**|
|map		    |           |nil	|**引用类型**|
|channel	    |	        |nil	|**引用类型**|
|interface	    |	        |nil	|接口|
|function	    |	        |nil	|函数|


## 布尔值

> 1. 布尔类型数据只有 `true` 和 `false` 两个值
> 2. Go 语言中不允许将整型强制转换为布尔型
> 3. 布尔型无法参与数值运算，也无法与其他类型进行转换


## 整型-基于进制表示

```go
	// 整型 基于进制 十进制 八进制 十六进制
	// 十进制
	var a int = 123
	// 八进制 以0开头 表示字符串中的字符
	var b int = 0123
	// 十六进制 以0x开头 使用场景：内存地址 位运算
	var c int = 0x123

	// 默认以十进制打印
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)

	// 十六进制
	var d int = 0xabc

	// %d 按照十进制打印数据
	fmt.Printf("%d\n", a)
	// %o 按照八进制打印数据
	fmt.Printf("%o\n", b) // 输出 123
	// %x %X 按照十六进制打印数据
	fmt.Printf("%x\n", d) // 输出 abc
	fmt.Printf("%X\n", d) // 输出 ABC
```


## 类型转换

> 将浮点型转成整型 可能丢失数据精度

```go
// 这个叫取别名
type int1 int
type int2 int

func main() {
	//var a int8 = 123
	//var b int = 234
	//fmt.Println(int(a) + b)
	//type rune = int32  两个数据类型可以计算(两个类型相同)
	//ch := 'A' //rune ==int32
	//var b int32 = 100
	//fmt.Println(ch + b)
	var a int1 = 123
	var b int2 = 123

	fmt.Println(a + int1(b))
}
```

