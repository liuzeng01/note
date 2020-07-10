# Scanner

## 循环读取用户输入

``` go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	scanner()
}

func scanner() {
	fmt.Printf("---->")
	scanner := bufio.NewScanner(os.Stdin)
	for scanner.Scan() {

		fmt.Println(scanner.Text())
		fmt.Printf("---->")
	}
}

```

----
[Back](summary.md)
--