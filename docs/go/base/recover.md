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

## 参考
+ <https://www.topgoer.cn/docs/golang/chapter05-7>