
```go
/*

type hmap struct {
	// Note: the format of the hmap is also encoded in cmd/compile/internal/gc/reflect.go.
	// Make sure this stays in sync with the compiler's definition.
	count     int // # live cells == size of map.  Must be first (used by len() builtin)
	flags     uint8
	B         uint8  // log_2 of # of buckets (can hold up to loadFactor * 2^B items)
	noverflow uint16 // approximate number of overflow buckets; see incrnoverflow for details
	hash0     uint32 // hash seed

	buckets    unsafe.Pointer // array of 2^B Buckets. may be nil if count==0.
	oldbuckets unsafe.Pointer // previous bucket array of half the size, non-nil only when growing
	nevacuate  uintptr        // progress counter for evacuation (buckets less than this have been evacuated)

	extra *mapextra // optional fields
}
*/

/*
map 数据结构，是由桶构成的链表(桶与桶之间是由链表链接的)，桶里面又有数组，
map 的 key value 对应的存储方式是 key key key value value value
*/
// map 中对应的key hash后总共64位，分为高位，中位，低位

func main() {
	m := make(map[int]string, 10)
	m[1001] = "法师"
	//m[1020] = "兵哥"
	//m[2020] = "木子"
	fmt.Println(unsafe.Sizeof(m)) // 返回的是指针(底层结构 hmap 指针)，所以始终为 8
	fmt.Println(m)
}
```