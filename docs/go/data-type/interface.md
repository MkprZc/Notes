## 接口

> Go语言中接口（interface）是一种类型，一种抽象的类型 (其实在 type 后面的都是类型)
> Go语言提倡面向接口编程

```go
/*
type 接口名 interface{
	方法声明
	方法名（参数）返回值
}
*/

type Humaner interface {
	SayHi()
}

type Person2 struct {
	Name string
	Age  int
}

type Teacher struct {
	Person2
	Subject string
}
type Student2 struct {
	Person2
	Score int
}

func (t Teacher) SayHi() {
	fmt.Printf("大家好，我是%s，我今年%d岁，我教%s\n", t.Name, t.Age, t.Subject)
}
func (s *Student2) SayHi() {
	fmt.Printf("大家好，我是%s，我今年%d岁，我的成绩%d\n", s.Name, s.Age, s.Score)
}

func main0401() {
	//tea := Teacher{Person2{"法师", 33}, "go语言开发"}
	stu := Student2{Person2{"木子", 18}, 100}
	//创建接口类型变量
	var h Humaner
	//将对象的地址赋值给接口类型
	h = &stu
	//通过接口调用
	h.SayHi()
}

//多态 将接口类型作为函数参数
func SayHi(h Humaner) {
	h.SayHi()
}
func main() {
	//初始化对象
	//tea := Teacher{Person2{"法师", 33}, "go语言开发"}
	stu := Student2{Person2{"木子", 18}, 100}
	// SayHi(&tea)
	// SayHi(tea)  
	SayHi(&stu)
	// SayHi(stu) // 报错，因为是指针接受者，只能接受指针
}
```

> 一个接口的方法，不一定需要由一个类型完全实现，接口的方法可以通过在类型中嵌入其他类型或者结构体来实现

```go
// WashingMachine 洗衣机
type WashingMachine interface {
	wash()
	dry()
}

// 甩干器
type dryer struct{}

// 实现WashingMachine接口的dry()方法
func (d dryer) dry() {
	fmt.Println("甩一甩")
}

// 海尔洗衣机
type haier struct {
	dryer //嵌入甩干器
}

// 实现WashingMachine接口的wash()方法
func (h haier) wash() {
	fmt.Println("洗刷刷")
}

func main() {
	var w WashingMachine = haier{}
	w.dry() // 甩一甩
}
```


## 空接口

> 空接口是指没有定义任何方法的接口。因此任何类型都实现了空接口
> 空接口类型的变量可以存储任意类型的变量


### 接口值

> 一个接口的值（简称接口值）是由一个具体类型和具体类型的值两部分组成的。这两部分分别称为接口的动态类型和动态值


## 类型断言

```go
func main() {
    var x interface{}
    x = "pprof.cn"
    v, ok := x.(string)
    if ok {
        fmt.Println(v)
    } else {
        fmt.Println("类型断言失败")
    }
}

// 直接用if v,ok := ins.(Type);ok {}的方式做类型探测在探测类型数量多时不是很方便，需要重复写if结构
// Golang提供了switch...case结构用于做多种类型的探测，所以这种结构也称为type-switch
```

