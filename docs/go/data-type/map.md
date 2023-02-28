## map 介绍

> map 是一种 `无序` 的基于 key-value 的数据结构，Go 语言中的 map 是引用类型，必须初始化才能使用


## map 定义

```go
	//map定义
	//var 字典名 map[键类型]值类型
	//var m map[int]string = make(map[int]string, 10)
	//m[1001] = "法师"
	//m[8888] = "兵哥"
	//
	//fmt.Println(m)

	// map是一个无序的的集合
	// map 默认并发不安全，多个 goroutine 写同一个 map，引发竞态错误
	// map 对象即使删除了全部的 key，但不会缩容空间
	m := map[int]string{1001: "法师", 8888: "兵哥", 1234: "兴臣", 3333: "孟跃平"}
	//fmt.Println(m)

	//k key 键 v value 值
	for k, v := range m {
		fmt.Println(k, v) // 每次打印的数据顺序可能不一样
	}

	// 元素为map类型的切片
	var mapSlice = make([]map[string]string, 3)

	// 值为切片类型的map
	var sliceMap = make(map[string][]string, 3)
```


## 删除和判断 key

```go
	//map中的key必须支持== ！=运算符操作  不能是切片 map 函数
	m := make(map[int]string)
	m[1001] = "法师"
	m[1005] = "兵哥"

	//通过key判断value是否存在
	if v, ok := m[1001]; ok {
		fmt.Println(v)
	} else {
		fmt.Println("不存在key")
	}

	fmt.Println(len(m))
	//删除map中的数据
	delete(m, 1001)
	fmt.Println(len(m))

	fmt.Println(m)

	// 返回的是底层 map 结构体指针的字节大小
	// 64位操作系统：8  32位操作系统：4
	fmt.Println(unsafe.Sizeof(m))
```

