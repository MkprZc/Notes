## recover 与 panic

```go
/*
编辑时异常
编译时异常
运行时异常
*/
// recover只有在defer调用的函数中有效
// 服务器开发，错误信息，recover 日志记录时间，参数，函数名等
func demo(i int) {
	//recover错误拦截  要在出现在panic错误之前设置
	defer func() {
		err := recover()
		if err != nil {
			fmt.Println(err)
		}
	}()
	var arr [10]int
	//数组下标越界
	arr[i] = 123 //err 出现错误，后面的代码不会执行
	fmt.Println(arr)
}
func main() {

	demo(10)
	fmt.Println("程序继续执行...") // demo 函数已经将错误拦截，所以这段代码会执行
}
```

## panic 介绍

> panic会导致程序的退出，平时开发中不要随便用，一般我们在哪里用到:我们一个服务启动的过程中，比如我的服务想要启动，必须有些依赖服务准备好，日志文件存在、MySQL能链接通、比如配置文件没有问题，这个时候服务才能启动的时候
> 如果我们的服务启动检查中发现了这些任何一个不满足那就调用panic 主动调用
> 但是你的服务一旦启动了，这个时候你的某行代码中不小心panic 那么不好意思你的程序挂了，这是重大事故
> 但是架不住有些地方我们的代码写的不小心会导致被动触发panic

```go
//一般性错误处理，已在代码层处理，不需要用 recover
//还有就是有的业务出现错误后必须崩溃，使用 recover 拦截反而会出现更大问题
func test(a int, b int) (v int, err error) {
	if b == 0 {
		err = errors.New("除数不能为0")
		return
	}
	v = a / b
	return
}
func main() {
	test(10, 0)
}
```

```go
// 延迟调用中引发的错误，可被后续延迟调用捕获，但仅最后一个错误可被捕获
func test() {
	defer func() {
		fmt.Println(recover())
	}()

	defer func() {
		panic("defer panic")
	}()

	panic("test panic")
}

func main() {
	test()

	fmt.Println("99")
}

// 输出
// defer panic
// 99
```

```go
// 捕获函数 recover 只有在延迟调用内直接调用才会终止错误，否则总是返回 nil。任何未捕获的错误都会沿调用堆栈向外传递
func test() {
    defer func() {
        fmt.Println(recover()) //有效
    }()
    defer recover()              //无效！
    defer fmt.Println(recover()) //无效！
    defer func() {
        func() {
            println("defer inner")
            recover() //无效！
        }()
    }()

    panic("test panic")
}

func main() {
    test()
}

// 输出
// defer inner
// <nil>
// test panic
```

```go
// 使用延迟匿名函数或下面这样都是有效的
func except() {
    fmt.Println(recover())
}

func test() {
    defer except()
    panic("test panic")
}

func main() {
    test()
}

// 输出
// test panic
```

```go
func test(x, y int) {
	var z int

	func() {
		defer func() {
			if recover() != nil {
				fmt.Println("----recover--------")
				z = 10
			}
		}()
		panic("test panic")
		z = x / y
		return
	}()

	fmt.Printf("x / y = %d\n", z)
}

func main() {
	test(2, 1)
}

// 输出
// ----recover--------
// x / y = 10
```

```go
// Go实现类似 try catch 的异常处理
func Try(fun func(), handler func(interface{})) {
    defer func() {
        if err := recover(); err != nil {
            handler(err)
        }
    }()
    fun()
}

func main() {
    Try(func() {
        panic("test panic")
    }, func(err interface{}) {
        fmt.Println(err)
    })
} 
```

## 捕获 panic 的位置

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	ch := make(chan struct{})

	test(ch)

	<-ch
}

func test(ch chan struct{}) {
	go func(ch chan struct{}) {

		defer func() {
			ch <- struct{}{}
		}()

		defer func() {
			if err := recover(); err != nil {
				fmt.Println("-----recover-------", err)

				for i := 0; i < 9; i++ {
					
					// pc 是uintptr这个返回的是函数指针
      				// file 是函数所在文件名目录
      				// line 所在行号
     				// ok 是否可以获取到信息

					pc, file, line, ok := runtime.Caller(i)
					pcName := runtime.FuncForPC(pc).Name() //获取函数名
					fmt.Println(fmt.Sprintf("%v   %s   %d   %t   %s", pc, file, line, ok, pcName))
				}

			}
		}()

		fn(10)

	}(ch)
}

func fn(i int) {
	var d [3]int
	d[i] = 10
}

// 输出
-----recover------- runtime error: index out of range [10] with length 3
3984658   C:/Users/Administrator/Desktop/1.go   28   true   main.test.func1.2
3614854   D:/Program Files/Go/src/runtime/panic.go   838   true   runtime.gopanic
3607998   D:/Program Files/Go/src/runtime/panic.go   89   true   runtime.goPanicIndex
3985157   C:/Users/Administrator/Desktop/1.go   43   true   main.fn
3985142   C:/Users/Administrator/Desktop/1.go   36   true   main.test.func1
3784032   D:/Program Files/Go/src/runtime/asm_amd64.s   1571   true   runtime.goexit
0      0   false
0      0   false
0      0   false
```

## 参考
+ <https://www.topgoer.cn/docs/golang/chapter05-7>