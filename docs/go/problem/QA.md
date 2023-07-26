```go
func main() {
	var wg sync.WaitGroup
	ctx, cancel := context.WithCancel(context.Background())

	wg.Add(1)
	go func() {
		defer wg.Done()
		select {
		case <-ctx.Done(): // 如果context取消则直接跳出
			fmt.Println("协程被取消")
			return
		default:
			fmt.Println("协程执行中")
			time.Sleep(10 * time.Second)
		}
	}()

	go func() {
		time.Sleep(2 * time.Second)
		cancel() // 取消协程
	}()

	wg.Wait()
	fmt.Println("主程序退出")
}

// 2 秒取消后，为什么要 10 秒后才打印 主程序退出，而不是 2 秒后打印
// 协程执行到某一种状态了


// 解决方案
func main() {
	var wg sync.WaitGroup
	ctx, cancel := context.WithCancel(context.Background())

	wg.Add(1)
	go func() {
		defer wg.Done()

		ch := make(chan struct{})
		go func() {
			defer close(ch)
			a := 10
			fmt.Println("子协程执行中")
			time.Sleep(10 * time.Second)
			fmt.Println(a)
		}()

		select {
		case <-ctx.Done(): // 如果context取消则直接跳出
			fmt.Println("协程被取消")
			return
		case <-ch:
			return
		}
	}()

	go func() {
		time.Sleep(2 * time.Second)
		cancel() // 取消协程
	}()

	wg.Wait()
	fmt.Println("主程序退出")
}

// 存在问题：
// 主协程取消退出导致协程也被取消退出了，如果子协程里面有很多的资源，那这些资源会被 gc 回收掉嘛

// chatgpt 回答：
go 主协程没有等待子协程执行完就退出导致子协程也跟着退出了，如果子协程里面有很多的资源，那这些资源会被 gc 回收掉嘛

对于你的问题，主协程退出并不会等待子协程执行完毕，因此子协程可能会被强制终止，而不管其中是否存在资源。当子协程被终止时，其中的资源将不会按照正常的流程释放，直到垃圾收集器（GC）运行。

在Go语言中，垃圾收集是自动进行的，由垃圾收集器负责扫描并回收不再被程序使用的内存。当子协程提前终止时，其中的资源没有引用时，这些资源将会被垃圾收集器标记为不再使用，并在垃圾收集时释放。

但是需要注意的是，垃圾收集并不是实时进行的，而是根据运行时系统的策略进行触发。因此，尽管资源最终会被垃圾收集器回收，但在终止子协程和资源真正被回收之间的时间段内，这些资源可能会一直占用内存。如果你希望及时释放这些资源，可以在主协程退出之前确保子协程执行完毕，或者显式地进行资源的释放和清理操作。


go 主协程退出导致子协程被强制终止，那子协程的所有资源会被释放回收嘛

对于你的问题，确实是这样的。当主协程退出时，所有的子协程会被强制终止，不会等待它们执行完毕。在子协程终止的过程中，其中的资源会被释放和回收。

在Go语言中，垃圾收集器（GC）负责自动管理内存。当子协程被强制终止时，子协程中的资源会被标记为不再被引用，这意味着它们可以被垃圾收集器回收。垃圾收集器在运行时会定期触发，它会清理和释放不再被程序使用的内存。

因此，尽管子协程被强制终止，其中的资源会在垃圾收集器运行时被回收，并最终释放掉所占用的内存。这是Go语言的垃圾回收机制，帮助开发者管理和回收不再使用的资源，防止内存泄漏。

```