## 结构体介绍

> 结构体本身也是一种类型
> 只有当结构体实例化时，才会真正地分配内存。也就是必须实例化后才能使用结构体的字段 (例如：var 结构体实例 结构体类型)
> 结构体中字段大写开头表示可公开访问，小写表示私有（仅在定义当前结构体的包中可访问）

```go
// 结构体所占的字节大小与成员的顺序有关（内存对齐）
type student struct {
	name  string  // 字符串属性的成员在结构体中保存的是 数据指针和长度
	age   int
	sex   string
	score int
	addr  string  // 结构体成员在定义时不能赋值
}

func main() {
	//定义结构体变量
	stu := student{"兵哥哥", 18, "男", 100, "山东聊城"}
	//stu.sex = "女"
	//fmt.Println(stu)
	fmt.Printf("%p\n", &stu)
	fmt.Println(&stu.name)
	fmt.Println(&stu.age)
	fmt.Println(&stu.sex)
	fmt.Println(&stu.score)
	fmt.Println(&stu.addr)
}

// 输出
// 0xc000042040
// 0xc000042040
// 0xc000042050
// 0xc000042058
// 0xc000042068
// 0xc000042070
```


## 创建指针类型结构体

```go
var p2 = new(person) // 等价于 p2 := &person{} (使用&对结构体进行取地址操作相当于对该结构体类型进行了一次new实例化操作)
p2.name = "测试"
p2.age = 18
p2.city = "北京"
fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"测试", city:"北京", age:18}

// p2.name = "测试"其实在底层是(*p2).name = "测试"，这是Go语言帮我们实现的语法糖
```


## 方法和接收者

方法的定义格式如下：

    func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
        函数体
    }


> 什么时候应该使用指针类型接收者

    1.需要修改接收者中的值
    2.接收者是拷贝代价比较大的大对象
    3.保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者。


```go
type Student struct {
	name string
	age  int
}

func main() {
	m := make(map[int]Student)
	s1 := Student{"兵哥", 18}
	s2 := Student{"法师", 33}
	m[1001] = s1

	//m[1001].name = "法师" //写操作 err
	m[1001] = s2 //如果想修改结构体  必须整体操作
	fmt.Println(m)
	fmt.Println(m[1001].name) //读操作 OK
}
```


## 匿名字段

> 匿名字段默认采用类型名作为字段名，结构体要求字段名称必须唯一，因此一个结构体中同种类型的匿名字段只能有一个

```go 
type Person struct {
    string
    int
}
```

```go
//子类和父类有共同的成员名
type Perple1 struct {
	Name string
	Age  int
}

type Student1 struct {
	//P Perple1
	//匿名字段 将结构体作为另外一个结构体成员
	Perple1
	Name  string
	Score int
}

func main() {
	stu := Student1{Perple1{"兵哥", 18}, "兵哥哥", 100}
	//stu.Name = "法师"
	//stu.P.Name = "法师"

	//采用就近原则 修改本结构体信息
	//stu.Name="法师"
	stu.Perple1.Name="法师"
	fmt.Println(stu)
}
```


## 结构体的 "继承"

```go
//Animal 动物
type Animal struct {
    name string
}

func (a *Animal) move() {
    fmt.Printf("%s会动！\n", a.name)
}

//Dog 狗
type Dog struct {
    Feet    int8
    *Animal //通过嵌套匿名结构体实现继承
}

func (d *Dog) wang() {
    fmt.Printf("%s会汪汪汪~\n", d.name)
}

func main() {
    d1 := &Dog{
        Feet: 4,
        Animal: &Animal{ //注意嵌套的是结构体指针
            name: "乐乐",
        },
    }
    d1.wang() //乐乐会汪汪汪~
    d1.move() //乐乐会动！
}
```


## 参考

+ <https://www.topgoer.cn/docs/golang/chapter03-15>