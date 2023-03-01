
```go
//闭包 将匿名函数作为函数的返回值

func seq() func() int {
	i := 0
	return func() int {
		//fmt.Println("hello")
		i++
		return i
	}
}
func main() {
	//可以读取函数内部的变量
	//可以将函数始终加载到内存中
	f := seq()
	value := f()
	fmt.Println(value)
	value = f()
	fmt.Println(value)
	value = f()
	fmt.Println(value)

	f1 := seq()
	value1 := f1()
	fmt.Println(value1)
	value1 = f1()
	fmt.Println(value1)

	// 手动删除，释放内存（栈空间）
	f = nil
	f1 = nil
}

// 输出：
// 1
// 2
// 3
// 1
// 2
```