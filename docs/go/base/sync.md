## Pool 临时对象池

```go
package main

import (
	"fmt"
	"sync"
)

//定义函数赋值给pool.New
func Demo() interface{} {
	return 6.66
}

func main() {
	var pool sync.Pool
	//put 将数据存储在临时对象池
	pool.Put(1)
	pool.Put("hello")
	pool.Put(true)
	pool.Put(3.14)

	//get 将数据从临时对象池取出
	value := pool.Get()
	fmt.Println("-----", value)
	//临时对象池第一个数据在最前，后续的数据采用先进后出的原则
	value = pool.Get()
	fmt.Println("+++++", value)

	//循环获取所有对象池内容
	for {
		value := pool.Get()
		if value == nil {
			break
		}
		fmt.Println(value)
	}
	//需要函数类型变量
	pool.New = Demo

	fmt.Println(pool.New())
}

// 输出
// ----- 1
// +++++ 3.14
// true
// hello
// 6.66

```


## 等待组(WaitGroup)和互斥锁(Mutex)

```go
package main

import (
	"fmt"
	"sync"
)

//不建议全局变量在协程中使用  如果使用 需要加锁
var num = 0
var wg sync.WaitGroup
var mu sync.Mutex

func Count() {
	mu.Lock()
	defer mu.Unlock()
	num++
	wg.Done()
}
func main() {
	wg.Add(10000)
	for i := 0; i < 10000; i++ {
		go Count()
	}

	wg.Wait()

	fmt.Println(num) // 10000
}

```


## Cond

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	wg := sync.WaitGroup{}

	cond := sync.NewCond(new(sync.Mutex))

	for i := 0; i < 3; i++ {
		go func(i int) {
			wg.Add(1)
			defer wg.Done()
			fmt.Println("协程启动：", i)
			cond.L.Lock() //加锁
			fmt.Println("协程：", i, "加锁")
			cond.Wait()  // 等待信号过来

			cond.L.Unlock() //解锁
			fmt.Println("协程：", i, "解锁")

		}(i)
	}

	time.Sleep(time.Second * 2)
	//发送信号
	cond.Signal()
	fmt.Println("主协程发送信号")

	time.Sleep(time.Second * 2)
	//发送信号
	cond.Signal()
	fmt.Println("主协程发送信号")

	time.Sleep(time.Second * 2)
	//发送信号
	cond.Signal()
	fmt.Println("主协程发送信号")

	wg.Wait()
}

```


## 单次执行

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func Test() {
	fmt.Println("----test--------")
}
func main() {
	var once sync.Once
	for i := 0; i < 10; i++ {
		//多次在协程调用  但只执行一次
		go func() {
			once.Do(Test) //函数回调
		}()
	}

	time.Sleep(time.Second * 3)
}

```


## sync.Map

> Map就像Go中的map[interface{}]interface{}，但对于多个goroutine的并发使用是安全的，不需要额外的锁定或协调。加载、存储和删除在摊销后的恒定时间内运行

```go
package main

import (
	"fmt"
	"sync"
)

func Print(key, value interface{}) bool {
	fmt.Println("键：", key, "值：", value)
	return true
}
func main() {
	//var m map[interface{}]interface{}//err map必须初始化才可以使用 make
	//m["法师"] = "帅哥"
	//fmt.Println(m)
	var smap sync.Map
	//将键值对存储在sync.map
	smap.Store("法师", 33)
	smap.Store("兵哥", 18)
	smap.Store("木子", 22)

	//从map中获取数据
	//value, ok := smap.Load("老子")
	//if ok {
	//	fmt.Println(value)
	//} else {
	//	fmt.Println("未找到数据")
	//}

	//从map中删除数据
	smap.Delete("法师")

	//遍历数据(函数回调  将函数作为函数参数)
	smap.Range(Print)
}

```