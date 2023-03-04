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


## 参考
+ <https://www.topgoer.cn/docs/golang/chapter04-4>