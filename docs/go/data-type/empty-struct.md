
```go
	//如果定义多个空结构体都指向同一个内存地址 (全局区)
	//null := NULL{}
	null := struct{}{}
	fmt.Println(unsafe.Sizeof(null)) // 0
	fmt.Printf("%p\n", &null) // 0x8572f8

	null1:=struct{}{}
	fmt.Printf("%p\n", &null1) // 0x8572f8
```

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	//空结构体不占用内存空间 通知作用
	ch := make(chan struct{})
	go func() {
		time.Sleep(time.Second)
		fmt.Println("协程执行完成...")
		close(ch)
	}()
	fmt.Println("主函数执行")
	<-ch
	fmt.Println("主函数执行完成")
}

```