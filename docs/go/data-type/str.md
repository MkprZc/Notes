## 字符串转义符

|转义   |含义|
| ---  | --- |
|\r	   |回车符（返回行首）|
|\n	   |换行符（直接跳到下一行的同列位置）|
|\t	   |制表符|
|'	   |单引号|
|"	   |双引号|
|\	   |反斜杠|

```go
	// 双引号，会识别转义字符
	a := "123\nabc"
	fmt.Println(a) // 输出时会换行

	// 反引号，以字符串的原生形式输出，包括换行和特殊字符，可以实现防止攻击、输出源代码等效果
    // 多行字符串也是使用 反引号
	b := `123\nabc` // 输出时原样输出，不会转义
	fmt.Println(b) // 123\nabc
```

```go
	str := " hel lo "
	// 去除空格
	str = strings.Replace(str, " ", "", -1) // hello
```


## 字符串的常用操作

|方法	                                |介绍|
| ---                                   | --- |
|len(str)	                            |求长度|
|+或fmt.Sprintf	                        |拼接字符串|
|strings.Split	                        |分割|
|strings.Contains	                    |判断是否包含|
|strings.HasPrefix,strings.HasSuffix	|前缀/后缀判断|
|strings.Index(),strings.LastIndex()	|子串出现的位置|
|strings.Join(a[]string, sep string)	|join操作|


## 参考
+ <https://www.topgoer.cn/docs/golang/chapter03-8>