
```go
	//如果定义多个空结构体都指向同一个内存地址 (全局区)
	//null := NULL{}
	null := struct{}{}
	fmt.Println(unsafe.Sizeof(null)) // 0
	fmt.Printf("%p\n", &null) // 0x8572f8

	null1:=struct{}{}
	fmt.Printf("%p\n", &null1) // 0x8572f8
```