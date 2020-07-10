# Pprof

## Go语言pprof工具的使用
### 1. 使用方式
**暂时先以应用的pprof工具使用为例**
```
package main

import (
	
	"runtime/pprof"

	"os"
	"fmt"

)

func main() {

	file, err := os.Create("./cpu.pprof")
	if err != nil {
		fmt.Printf("create cpu pprof failed, err:%v\n", err)
		return
	}
	pprof.StartCPUProfile(file)
	defer pprof.StopCPUProfile()

	Func(){
        .......
    }



}
```
编译之后进行运行，程序会自动生成一个cpu.pprof的文件，这个使用可以使用`go tool pprof cpu.pprof`来进行分析
```
Type: cpu
Time: Jul 4, 2020 at 5:58pm (CST)
Duration: 28.99s, Total samples = 37.83s (130.51%)
Entering interactive mode (type "help" for commands, "o" for options)
(pprof) web
(pprof) top
Showing nodes accounting for 19550ms, 51.68% of 37830ms total
Dropped 280 nodes (cum <= 189.15ms)
Showing top 10 nodes out of 137
      flat  flat%   sum%        cum   cum%
    6790ms 17.95% 17.95%    11980ms 31.67%  regexp.(*Regexp).tryBacktrack
    2490ms  6.58% 24.53%     2490ms  6.58%  regexp.(*bitState).shouldVisit
    2190ms  5.79% 30.32%     2190ms  5.79%  runtime.stdcall1
    1800ms  4.76% 35.08%     2470ms  6.53%  runtime.heapBitsSetType
    1680ms  4.44% 39.52%     1680ms  4.44%  regexp.(*inputString).step
    1220ms  3.22% 42.74%     6830ms 18.05%  runtime.mallocgc
    1050ms  2.78% 45.52%     1060ms  2.80%  runtime.stdcall3
     870ms  2.30% 47.82%      920ms  2.43%  regexp.(*bitState).push
     750ms  1.98% 49.80%      750ms  1.98%  runtime.memclrNoHeapPointers
     710ms  1.88% 51.68%      710ms  1.88%  runtime.nextFreeFast
(pprof) list logicCode
Total: 37.83s
```
中间使用了一个web的命令，这个需要安装graphviz工具，然后就可以已可视化的方式展现所有程序运行的展示。

![cpupprof](/assets/cpupprof.png)


后续将会继续补充相关使用方法

> [Go pprof 性能调优](https://mp.weixin.qq.com/s?src=11&timestamp=1593855053&ver=2440&signature=7n4pF9vkk9YPsVS3pzSRGfcYNrsdjHCuTX5et7LyUtoZ-edsAHWq9C918uS5lxHqwHUQd4jGDCgwha6DUYVlPZzJUHBT1YDbvq2cqoT9nsuskJ2JE2x*1wXrVvcE49bP&new=1)
> 
> [Go性能调优](https://www.liwenzhou.com/posts/Go/performance_optimisation/)


----
[Back](go-summary.md)
--