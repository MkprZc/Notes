## 闭包介绍

> + 闭包是由函数及其相关引用环境组合而成的实体(即：闭包=函数+引用环境)
> + 维基百科讲，闭包（Closure），是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例

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