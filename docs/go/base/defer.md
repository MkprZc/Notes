## defer 介绍

> + 栈的规则：先进后出
> + 多个 defer 执行顺序：先进后出

```go
func main() {
	//defer 延迟调用 将函数加载到内存中并没有执行 而是在函数出栈销毁时在执行
	defer fmt.Println("1")
	defer fmt.Println("2")
	defer fmt.Println("3")
}

// 输出
// 3
// 2
// 1
```

```go
func main() {
	var whatever [5]struct{}

	for i := range whatever {
		defer fmt.Println(i)
	}
}

// 输出
// 4
// 3
// 2
// 1
// 0
```

```go
// defer 碰上闭包
func main() {
	var whatever [5]struct{}
	for i := range whatever {
		defer func() { fmt.Println(i) }()
	}
}
// 由于闭包用到的变量 i 在执行的时候已经变成4,所以输出全都是4
```

```go
// 多个 defer 注册，按 FILO 次序执行 ( 先进后出 )。哪怕函数或某个延迟调用发生错误，这些调用依旧会被执行
func test(x int) {
	defer println("a")
	defer println("b")

	defer func() {
		println(100 / x) // div0 异常未被捕获，逐步往外传递，最终终止进程。
	}()

	defer println("c")
}

func main() {
	test(0)
}

// 输出
// c
// b
// a
// panic: runtime error: integer divide by zero
```

```go
// 延迟调用参数在注册时求值或复制，可用指针或闭包 “延迟” 读取
func test() {
    x, y := 10, 20

    defer func(i int) {
        println("defer:", i, y) // y 闭包引用
    }(x) // x 被复制

    x += 10
    y += 100
    println("x =", x, "y =", y)
}

func main() {
    test()
}

// 输出
// x = 20 y = 120
// defer: 10 120
```

## defer 陷阱

```go
func foo(a, b int) (i int, err error) {
    defer fmt.Printf("first defer err %v\n", err)
    defer func(err error) { fmt.Printf("second defer err %v\n", err) }(err)
    defer func() { fmt.Printf("third defer err %v\n", err) }()
    if b == 0 {
        err = errors.New("divided by zero!")
        return
    }

    i = a / b
    return
}

func main() {
    foo(2, 0)
}

// 输出
// third defer err divided by zero!
// second defer err <nil>
// first defer err <nil>
```

```go
func foo() (i int) {

    i = 0
    defer func() {
        fmt.Println(i)
    }()

    return 2
}

func main() {
    foo()
}

// 输出
// 2
```

```go
// 记得检查返回值的错误（多做错误判断）
func do() (err error) {
    f, err := os.Open("book.txt")
    if err != nil {
        return err
    }

    if f != nil {
        defer func() {
            if ferr := f.Close(); ferr != nil {
                err = ferr
            }
        }()
    }

    // ..code...

    return nil
}

func main() {
    do()
}
```