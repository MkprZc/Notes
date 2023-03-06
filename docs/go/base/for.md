## for 语法

    for init; condition; post { }
    for condition { }
    for { }
    init： 一般为赋值表达式，给控制变量赋初值；
    condition： 关系表达式或逻辑表达式，循环控制条件；
    post： 一般为赋值表达式，给控制变量增量或减量。
    for语句执行过程如下：
    ①先对表达式 init 赋初值；
    ②判别赋值表达式 init 是否满足给定 condition 条件，若其值为真，满足循环条件，则执行循环体内语句，然后执行 post，进入第二次循环，再判别 condition；否则判断 condition 的值为假，不满足条件，就终止for循环，执行循环体外语句。

```go
package main

func length(s string) int {
    println("call length.")
    return len(s)
}

func main() {
    s := "abcd"

    for i, n := 0, length(s); i < n; i++ {     // 避免多次调用 length 函数。
        println(i, s[i])
    } 
}  

// 输出
// call length.
// 0 97
// 1 98
// 2 99
// 3 100
```


## range 语法

```go
for key := range slice {

}
```

> + `range` 会复制对象

```go
    a := [3]int{0, 1, 2}

    for i, v := range a { // index、value 都是从复制品中取出。

        if i == 0 { // 在修改前，我们先修改原数组。
            a[1], a[2] = 999, 999
            fmt.Println(a) // 确认修改有效，输出 [0, 999, 999]。
        }

        a[i] = v + 100 // 使用复制品中取出的 value 修改原数组。

    }

    fmt.Println(a) // 输出 [100, 101, 102]。
```

引用类型，其底层数据不会被复制
```go
    s := []int{1, 2, 3, 4, 5}

    for i, v := range s { // 复制 struct slice { pointer, len, cap }。

        if i == 0 {
            s = s[:3]  // 对 slice 的修改，不会影响 range。
            s[2] = 100 // 对底层数据的修改。
        }

        println(i, v)
    }

    // 输出
    // 0 1
    // 1 2
    // 2 100
    // 3 4
    // 4 5
```


## 参考
+ <https://www.topgoer.cn/docs/golang/chapter04-4>
+ <https://www.topgoer.cn/docs/golang/chapter04-5>