# github.com/x-mod/routine

>为什么要写这么一个基础库呢？

>每段程序都是从 main 函数开始的，但是，却不会将整个程序的功能实现都放在 main 函数里。而是，通过层层功能的抽象与封装，最终，在 main 函数仅提供功能函数的入口。所以，当我们看很多大型程序时，其实，main 函数是非常简单的。

>但是，即使 main 函数越变越简单，有些必要的功能则是逃不掉的。例如程序启动参数、程序的信号处理，这些通常还是会放在 main 函数进行处理。

>除了以上 main 函数本身的处理以外，我发现将程序中的执行绪，也就是固定的 Go 协程的入口放在 main 函数中进行定义，可以帮助维护者更加快速的理解应用的逻辑实现。

[link](https://www.gitdig.com/post/go-routine/)
----


**使用方法**
--
``` go
import "github.com/x-mod/routine"

//timeout
timeout := routine.Timeout(time.Minute, exec)

//retry
retry := routine.Retry(3, exec)

//repeat
repeat := routine.Repeat(10, time.Second, exec)

//concurrent
concurrent := routine.Concurrent(4, exec)

//schedule executor
crontab := routine.Crontab("* * * * *", exec)

//command
command := routine.Command("echo", routine.ARG("hello routine!"))

//parallel
parallel := routine.Parallel(exec1, exec2, exec3, ...)

//sequence
sequece := routine.Append(exec1, exec2, exec3, ...)
```



> github上提供的示例程序
> ```
``` go 
package main

import (
	"context"
	"log"
	"os"
	"syscall"
	"time"

	_ "net/http/pprof"
	"runtime/trace"

	"github.com/x-mod/routine"
)

func prepare(ctx context.Context) error {
	log.Println("prepare begin")
	defer log.Println("prepare end")
	trace.Logf(ctx, "prepare", "prepare ... ok")
	return nil
}

func cleanup(ctx context.Context) error {
	log.Println("cleanup begin")
	defer log.Println("cleanup end")
	time.Sleep(time.Millisecond * 50)
	trace.Logf(ctx, "cleanup", "cleanup ... ok")
	return nil
}

func foo(ctx context.Context) error {
	log.Println("foo begin")
	defer log.Println("foo end")
	time.Sleep(time.Second * 2)
	trace.Logf(ctx, "foo", "sleeping 2s done")
	return nil
}

func bar(ctx context.Context) error {
	log.Println("bar begin")
	defer log.Println("bar end")
	for i := 0; i < 10; i++ {
		log.Println(i)
		trace.Logf(ctx, "bar", "counting ... %d", i)
	}
	return nil
}

func main() {
	f, err := os.Create("trace.out")
	if err != nil {
		log.Fatalf("failed to create trace output file: %v", err)
	}
	defer func() {
		if err := f.Close(); err != nil {
			log.Fatalf("failed to close trace file: %v", err)
		}
	}()

	if err := routine.Main(
		context.TODO(),
		routine.ExecutorFunc(bar),
		routine.Signal(syscall.SIGINT, routine.SigHandler(func() {
			os.Exit(1)
		})),
		routine.Prepare(routine.ExecutorFunc(prepare)),
		routine.Cleanup(routine.ExecutorFunc(cleanup)),
		routine.Trace(f),
		routine.Go(routine.ExecutorFunc(foo)),
		routine.Go(routine.ExecutorFunc(foo)),
		routine.Go(routine.ExecutorFunc(foo)),
	); err != nil {
		log.Println(err)
	}
}
```



> 接下来是自己的一些模仿实现
``` go
package main

import (
	"context"
	"time"

	"fmt"

	"github.com/x-mod/routine"
)

func main() {
	err := routine.Main(
		context.TODO(),
		routine.ExecutorFunc(bar),                                        //main协程入口
		routine.Prepare(routine.ExecutorFunc(prepare)),                   //启动开始前的准备函数
		routine.Cleanup(routine.ExecutorFunc(exitclean)),                 //启动关闭后的清理函数
		routine.Go(routine.ExecutorFunc(dosomething)),                    //启动并发的协程
		routine.Go(routine.Concurrent(5, routine.ExecutorFunc(conexec))), //启动并发执行同一程序
		//routine.Go(routine.Crontab("*/1 * * * *", routine.ExecutorFunc(crontabexec))), //定时执行任务，配置crontab表达式
		routine.Go(routine.Repeat(3, time.Second, routine.ExecutorFunc(repeatexec))), //定时重复执行一项程序
		//routine.Go(routine.Deadline(time.Now().Add(5*time.Second), routine.ExecutorFunc(deadlinexec))),这个没搞明白
	)
	if err != nil {
		fmt.Println(err)
	}

}

func bar(ctx context.Context) error {
	fmt.Println("bar begin")
	fmt.Println(time.Now())
	defer fmt.Println("bar end")
	return nil
}

func prepare(ctx context.Context) error {
	fmt.Println("Start Prepare !!")
	defer fmt.Println("Prepare finish !!")
	return nil
}
func exitclean(ctx context.Context) error {
	fmt.Println("Exiting !!")
	defer fmt.Println("Exited")
	return nil
}
func dosomething(ctx context.Context) error {
	routine.New(routine.ExecutorFunc(bar)).Execute(ctx) //启动新的协程
	time.Sleep(50 * time.Millisecond)
	fmt.Println(time.Now())
	return nil
}

func conexec(ctx context.Context) error {
	fmt.Println("并发执行中！！！")
	return nil
}

func crontabexec(ctx context.Context) error {
	fmt.Println("定时执行中 ~~~")
	return nil
}

func repeatexec(ctx context.Context) error {
	fmt.Println("重复执行中，每隔一秒执行一次")
	return nil
}

func deadlinexec(ctx context.Context) error {
	for i := 0; i < 10; i++ {
		fmt.Printf("等待deadline。。。,%d 秒\n", i)
		time.Sleep(time.Second)
	}
	return nil
}
```


## 总结
- 这个库包装了所有常见的需要goroutine操作的情景，可以支持并发，串行，重复执行，重试，定时执行，启动新协程等一系列常用的Go并发场景，以后多包装使用这个库，能剩下不少的事情
  
---
[Crontab语法](../../../Linux/Crontab.md)
---
[Back](../../第三方库.md)
--