# Sort

## 实现Go提供的sort接口，来进行自定义排序

> 只需要实现下列三个接口，就可以进行排序
``` go
package sort
type Interface interface {
    Len() int            // 获取元素数量
    Less(i, j int) bool // i，j是序列元素的指数。
    Swap(i, j int)        // 交换元素
}
```

**自己模拟了一个最简单的示例**
```
package main

import (
	"fmt"
	"sort"
)

func main() {

	var mysort MySort
	var a SortType
	a.id = 5
	a.Name = "a"
	var b SortType
	b.id = 51
	b.Name = "b"
	var c SortType
	c.id = 6
	c.Name = "c"
	var d SortType
	d.id = 1
	d.Name = "d"
	mysort = []SortType{a, b, c, d}
	sort.Sort(mysort)
	fmt.Println(mysort)
}

type MySort []SortType

type SortType struct {
	Name string
	id   int
}

func (a MySort) Len() int           { return len(a) }
func (a MySort) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a MySort) Less(i, j int) bool { return a[i].id < a[j].id }

```


``` 
//输出结果为
[{d 1} {a 5} {c 6} {b 51}]
```

>接口实现不受限于结构体，任何类型都可以实现接口。但是系统自定义的类型是无法实现的，因为GO语言自己实现了这些[]int ,[]string 的接口。如果要自己实现自定义的排序的话，可以添加别名类型。


----
[Back](go-summary.md)
--