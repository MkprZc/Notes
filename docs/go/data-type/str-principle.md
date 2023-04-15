
```go
func main() {
	str := "hello world"
	// 返回的是底层字符串结构体的字节大小
	fmt.Println(unsafe.Sizeof(str)) // 16
	//字符串下标读取字符
	fmt.Println(str[2])         // 108  ASCII 码
	fmt.Println(string(str[2])) // l
	//字符串下标修改字符
	//str[2]='M'//err

	//创建切片
	slice := []byte(str)
	//修改切片中的元素的值
	slice[2] = 'M'
	fmt.Println(string(slice)) // heMlo world
}
```


```go
func main() {
	slice := []byte{'h', 'e', 'l', 'l', 'o'}
	str := string(slice) //如果出现赋值会发生内存拷贝

	//str[2]='M'//err 字符串常量区不允许修改

	//修改slice 不会影响str
	slice[2] = 'M'
	fmt.Println(str)
	fmt.Println(string(slice)) //不会内存拷贝

	//硬编码常量

	//fmt.Println("helloworld")
	str1 := "helloworld"
	fmt.Println(str1 == "helloworl")
}
```


```go
// string和[]byte都可以表示字符串，但因数据结构不同，其衍生出来的方法也不同，要跟据实际应
// 用场景来选择。

// string 擅长的场景：

// 需要字符串比较的场景；
// 不需要nil字符串的场景；

// []byte擅长的场景：

// 修改字符串的场景，尤其是修改粒度为1个字节；
// 函数返回值，需要用nil表示含义的场景；
// 需要切片操作的场景；

// 虽然看起来string适用的场景不如[]byte多，但因为string直观，在实际应用中还是大量存在，在
// 偏底层的实现中[]byte使用更多
```