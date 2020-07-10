# interface 

## io.Reader 接口
``` go
type Reader interface {
    Read(p []byte)(n int, err error) 
}
```

## io.Writer 接口
``` go 
type Writer interface{
    Writer(p []byte)(n int,err error)
}
```

## io.ReaderWriter接口
``` go
type ReaderWriter interface{
    Reader 
    Writer
}

```



```
os.File 同时实现了 io.Reader 和 io.Writer
strings.Reader 实现了 io.Reader
bufio.Reader/Writer 分别实现了 io.Reader 和 io.Writer
bytes.Buffer 同时实现了 io.Reader 和 io.Writer
bytes.Reader 实现了 io.Reader
compress/gzip.Reader/Writer 分别实现了 io.Reader 和 io.Writer
crypto/cipher.StreamReader/StreamWriter 分别实现了 io.Reader 和 io.Writer
crypto/tls.Conn 同时实现了 io.Reader 和 io.Writer
encoding/csv.Reader/Writer 分别实现了 io.Reader 和 io.Writer
mime/multipart.Part 实现了 io.Reader
net/conn 分别实现了 io.Reader 和 io.Writer(Conn接口定义了Read/Write)
```
    除此之外，io 包本身也有这两个接口的实现类型。如：
    1. 实现了Reader的类型：LimitedReader、PipeReader、SectionReader
    2.  实现了Writer的类型：PipeWriter以上类型中，常用的类型有：os.File、strings.Reader、bufio.Reader/Writer、bytes.Buffer、bytes.Reader类型





----
[Back](go-summary.md)
--
