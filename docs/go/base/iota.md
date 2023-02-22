## iota

> + iota 是 go 语言的常量计数器，只能在常量的表达式中使用。
> + iota 在 `const` 关键字出现时将被重置为 `0`。 `const` 中每新增一行常量声明将使 `iota` 计数一次( `iota` 可理解为 `const` 语句块中的行索引)。 使用 `iota` 能简化定义，在定义枚举时很有用

```go
const (
            n1 = iota //0
            n2        //1
            n3        //2
            n4        //3
        )
```

> 使用 `_` 跳过某些值

```go
const (
            n1 = iota //0
            n2        //1
            _
            n4        //3
        )
```

> `iota` 声明中间插队

```go
const (
            n1 = iota //0
            n2 = 100  //100
            n3 = iota //2
            n4        //3
        )
const n5 = iota //0
```

> 多个 `iota` 定义在一行

```go
const (
            a, b = iota + 1, iota + 2 //1,2
            c, d                      //2,3
            e, f                      //3,4
        )
```


## 参考
+ <https://www.topgoer.cn/docs/golang/chapter03-7>