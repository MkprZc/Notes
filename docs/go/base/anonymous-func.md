## 匿名函数介绍

> 匿名函数是指不需要定义函数名的一种函数实现方式
> 匿名函数的优越性在于可以直接使用函数内的变量

```go
//定义函数类型
type FUNCTYPE func(int, int)

func main() {
	//匿名函数
	//func(a int, b int) {
	//	fmt.Println(a + b)
	//}(10,20)

	//定义函数类型变量f
	//var f FUNCTYPE
	//var f func(int,int)
	f := func(a int, b int) {
		fmt.Println(a + b)
	}

	fmt.Printf("%T\n",f) // func(int, int)
	//函数在代码区的内存地址
	//fmt.Println(test)
	f(10, 20)
	//fmt.Println(f)
}
```