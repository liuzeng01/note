# Json

- `func Marshal(v interface{}) ([]byte, error) // json编码`

- `func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error) // json编码带缩进`
  

- `func Unmarshal(data []byte, v interface{}) error // json解码`



``` go 
package main

import (
    "fmt"
    // "reflect"
    "encoding/json"
)

type User struct{
    // Name string `json:"-,"`  // 这个导出才是  {"-":"张三","age":20}
    // Name string `json:"-"`   // always omitted
    // Name string `json:",omitempty"`     // 如果为零值导出为空. 然而不想重命名.
    Name string `json:"name,omitempty"`  // 如果为零值导出为空.
    // Age int `json:"age,string"`         // 这个会把int变成json string...
    // 上面那个很有用.  在 A map => string => B map 的时候可能会被弄成科学计数法..
    // 用字符串表示时间戳这种可以避免这个...
    Age int `json:"age"`
    sex int `json:"sex"`
}

func main() {
    u := User{"张三", 20, 1}
    uu, _ := json.Marshal(u)
    // 只会导出公有的, 即首字母大写的
    fmt.Println(string(uu))
}
```
---
[Back](summary.md)
--