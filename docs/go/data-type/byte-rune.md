## byte

> + Golang中没有专门的字符类型，如果要存储单个字符(字母)，一般使用 `byte` 来保存
> + 字符只能被单引号包裹，不能用双引号，双引号包裹的是字符串
> + 字符使用 UTF-8 编码，英文字母占一个字符，汉字占 3或4个字符

```go
	// byte 字符类型, 底层： type byte = uint8, 代表了ASCII码的一个字符
	// 实际为 uint8 会按照 uint8 格式打印
	// ASCII 码
	// 0-31 控制字符 转义字符
	// 32-126 默认字符 48 = '0' 65 = 'A' 97 = 'a'
	// 127 Delete 键

	var ch byte = 'a' // 取值范围小，不支持中文
	fmt.Println(ch) // 97
	fmt.Printf("%T\n", ch) // uint8

	// %c 按照字符打印输出
	fmt.Printf("%c\n", ch) // a
```


## rune

```go
	// rune 字符类型，底层：type rune = int32, 代表一个 UTF-8字符
	// 实际为 int32 会按照 int32 格式打印
	var r rune = '中' // 支持 utf-8 中文格式
	fmt.Println(r) // 20013
	fmt.Printf("%T\n", r) // int32

	fmt.Printf("%c\n", r) // 中
```