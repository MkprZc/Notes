
for 循环中，协程按顺序一个一个的执行

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