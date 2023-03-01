
```go
func main() {
	//defer 延迟调用 将函数加载到内存中并没有执行 而是在函数出栈销毁时在执行
	defer fmt.Println("1")
	defer fmt.Println("2")
	defer fmt.Println("3")
}

// 输出
// 3
// 2
// 1
```