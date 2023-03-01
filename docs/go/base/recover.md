
```go
/*
编辑时异常
编译时异常
运行时异常
*/
// recover只有在defer调用的函数中有效
// 服务器开发，错误信息，recover 日志记录时间，参数，函数名等
func demo(i int) {
	//recover错误拦截  要在出现在panic错误之前设置
	defer func() {
		err := recover()
		if err != nil {
			fmt.Println(err)
		}
	}()
	var arr [10]int
	//数组下标越界
	arr[i] = 123 //err 出现错误，后面的代码不会执行
	fmt.Println(arr)
}
func main() {

	demo(10)
	fmt.Println("程序继续执行...") // demo 函数已经将错误拦截，所以这段代码会执行
}
```

```go
//一般性错误处理，已在代码层处理，不需要用 recover
//还有就是有的业务出现错误后必须崩溃，使用 recover 拦截反而会出现更大问题
func test(a int, b int) (v int, err error) {
	if b == 0 {
		err = errors.New("除数不能为0")
		return
	}
	v = a / b
	return
}
func main() {
	test(10, 0)
}
```