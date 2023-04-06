
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    //chan定义方式
    // 变量名 chan=make(chan 数据类型,大小)
    //ch := make(chan int, 10)//双向带缓冲区
    //fmt.Println(unsafe.Sizeof(ch))

    //单向chan
    //ch:=make(chan<- int) //接收
    //ch := make(<-chan int) //发送

    //有缓冲和无缓冲区别
    //无缓冲 要求发送和接收必须同时准备好 才可以完成  属于同步操作 会发生阻塞等待
    //有缓冲 可以接收N个值 属于异步操作
    ch := make(chan int)

    go func() {
        value := <-ch
        fmt.Println(value)
    }()
    ch <- 1
    time.Sleep(time.Second * 2) // 不等待两秒的话，有可能子协程来不及打印，主协程就退出了
}

```


## for 循环中，协程按顺序一个一个的执行

```go
func main() {
    ch := make(chan struct{})
    for i := 0; i < 100; i++ {
        go func() {
            fmt.Println("Start Goroutine", i)
            // time.Sleep(time.Second)
            // fmt.Println("End Goroutine", i)
            ch <- struct{}{}
        }()

        <-ch // 等待上一个协程执行完毕
    }
    close(ch)
}
```