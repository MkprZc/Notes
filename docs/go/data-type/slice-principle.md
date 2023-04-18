
```go
func Test(slice []int) []int {
	slice = append(slice, 1, 2, 3, 4, 5) // append 在操作切片时有内存拷贝，会在新开辟的内存上进行操作

	//fmt.Println("demo函数：", slice)
	//slice[0] = 666
	//slice[1] = 777
	//slice[2] = 888
	//slice[3] = 999
	//slice[4] = 000
	return slice
}
func main() {
	slice := []int{1, 2, 3, 4, 5}

	slice = Test(slice)

	fmt.Println(slice) // [1 2 3 4 5 1 2 3 4 5]
}
```

```go
func Demo(slice *[]int) {
	*slice = append(*slice, 1, 2, 3, 4, 5)
}
func main() {
	/*
		//所有内存地址 是一个无符号十六进制整型数据表示的
		type slice struct {
			array unsafe.Pointer	//指针 8字节
			len   int				//长度 8字节
			cap   int				//容量 8字节
		}
	*/
	slice := []int{1, 2, 3, 4, 5}
	fmt.Println(unsafe.Sizeof(slice)) // 24
	Demo(&slice)
	fmt.Println(unsafe.Sizeof(slice)) // 24

	fmt.Println(slice) // [1 2 3 4 5 1 2 3 4 5]
}
```

```go
func main() {
	slice := []int{1, 2, 3, 4, 5} //len==cap
	fmt.Printf("%p\n", slice)     // 0xc0000cc030

	slice = append(slice, 6)  //len=7 cap=10
	fmt.Printf("%p\n", slice) // 0xc0000b00f0

	slice = append(slice, 6)  //len=8 cap=10
	fmt.Printf("%p\n", slice) // 0xc0000b00f0

	fmt.Printf("%p\n", &slice) // 获取的是底层切片结构体的内存地址，在同一个函数内是一样的

}
```