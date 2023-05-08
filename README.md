
> An awesome project.

golang mysql 死锁解决 linux

## Go

context

数据类型的转换
字符串到 整型，之类的
整型 到 字符串


字符串的高性能拼接

字符串常用的 包 strings


go 的list 包


函数的参数是值传递，go中都是值传递

函数的可变参数： ...

函数闭包

func aa() func() int {
	local := 0
	return func() int {
		local += 1
		return local
	}
}


defer 会在return 之前执行

按照指定顺序遍历map


指针的内存图未画

switch 

排序算法（算法库）

引用类型 map、channel 是指针包装，range 复制值是什么

斐波那契数列（递归）

go 常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型

结构体的内存对齐



没有延迟的mysql集群,机器重启数据自动同步多点读写,对代码要求很高,要求支持多点读写

go中函数是"一等公民"
1，函数本身可以当做变量2.匿名函数 闭包3，函数可以满足接口




go语言限制了指针的运算，在c 语言中你得到一个指针之后可以进行+1 在g语言中不行 不能参加运算
//go的指针是一个阉制版，unsafe 包里面，所以说一段我们不会使用unsafe 包，但是当你要用到的时候是可以使用


go vet 命令 检测代码

go 文件操作


bug 记录 c 调用 go，传字符串，在 go 的协程中多次调用会乱码的问题，
是空指针的问题，需要在 go 这边对这个字符串进行深拷贝


mysql 
软删除（就是用一个字段去标记是否已经删除）
硬删除（就是 delete）

go Prometheus、Grafana
(https://cloud.tencent.com/developer/article/1769920)
