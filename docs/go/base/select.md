## select 语句

> select 是Go中的一个控制结构，类似于用于通信的switch语句。每个case必须是一个通信操作，要么是发送要么是接收
> select 随机执行一个可运行的case。如果没有case可运行，它将阻塞，直到有case可运行。一个默认的子句应该总是可运行的


```go
package main

import (
	"fmt"
	"time"
)

func main() {
	//指定CPU核数
	//fmt.Println(runtime.GOMAXPROCS(0))
	// runtime.GOMAXPROCS(1)
	intch := make(chan int, 1)
	strch := make(chan string, 1)
	intch <- 1
	strch <- "hello"
	/*
		1、select case如果未准备好 会阻塞
		2、如果多个case都满足条件会随机选择一个执行
		3、case 必须为IO操作
		4、可以做超时和退出判断
		5、select  如果对应的是异步处理 需要在for循环中使用
		6、select 可以监控chan数据流向
	*/
	select {
	case value := <-intch:
		fmt.Println(value)
	case value := <-strch:
		fmt.Println(value)
	case <-time.After(time.Second * 5): //超时处理
		fmt.Println("超时")
		//default:阻塞操作
	}
}

```


### 退出

```go
//主线程（协程）中如下：
var shouldQuit=make(chan struct{})
fun main(){
    {
        //loop
    }
    //...out of the loop
    select {
        case <-c.shouldQuit:
            cleanUp()
            return
        default:
        }
    //...
}

//再另外一个协程中，如果运行遇到非法操作或不可处理的错误，就向shouldQuit发送数据通知程序停止运行
close(shouldQuit)
```