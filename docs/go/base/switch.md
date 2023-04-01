## switch 介绍

> switch 语句用于基于不同条件执行不同动作，每一个 case 分支都是唯一的，从上直下逐一测试，直到匹配为止。
> 分支表达式可以是任意类型，不限于常量。可省略 break，默认自动终止。

```go
	//switch 表达式{case 值1：代码1 case 值2：代码2} 那个分支满足条件执行那一个

	//输入月份 计算天数
	day := 0
	m := 0
	fmt.Scan(&m)
	switch m {
	//如果有多个值 执行相同代码 可以使用逗号分隔
	case 1, 3, 5, 7, 8, 10, 12:
		day = 31
		//fallthrough //执行完当前分支 继续向下执行(不管是否满足下面的条件)
	case 2:
		day = 29
	case 4, 6, 9, 11:
		day = 30
	default:
		//默认选择
		day = -1
	}
	fmt.Println(day)
```

```go
	//pi := 3.14
	////浮点型因为精度问题可能在switc中认为是同一个值
	//switch pi {
	//case 3.140000000000000000019999999:
	//	fmt.Println(pi)
	//case 3.14:
	//	fmt.Println(pi)
	//}
```


## Type Switch

> switch 语句还可以被用于 type-switch 来判断某个 interface 变量中实际存储的变量类型
> 类型断言的 方便语法

```go
switch x.(type){
    case type:
       statement(s)      
    case type:
       statement(s)
    /* 你可以定义任意个数的case */
    default: /* 可选 */
       statement(s)
} 
```