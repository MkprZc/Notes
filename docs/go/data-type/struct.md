
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