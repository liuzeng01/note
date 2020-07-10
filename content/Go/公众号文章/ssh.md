# SSH

---
###利用Go语言提供的ssh包实现一个ssh工具


``` go 
package main

import (
	"fmt"
	"log"
	"net"
	"os"
	"time"

	"golang.org/x/crypto/ssh"
)

func main() {
	session, err := connect("root", "abc@123A", "10.45.11.115", 22)
	if err != nil {
		log.Fatal(err)
	}
	defer session.Close()

	fmt.Printf("---->")
	session.Stdout = os.Stdout
	session.Stderr = os.Stderr
	session.Stdin = os.Stdin
	session.Run("sh")

}
func connect(user, password, host string, port int) (*ssh.Session, error) {
	var (
		addr         string
		clientConfig *ssh.ClientConfig
		client       *ssh.Client
		session      *ssh.Session
		err          error
	)

	clientConfig = &ssh.ClientConfig{
		User: user,
		Auth: []ssh.AuthMethod{
			ssh.Password(password),
		},
		Timeout: 30 * time.Second,
		HostKeyCallback: func(hostname string, remote net.Addr, key ssh.PublicKey) error {
			return nil
		},
	}

	// connet to ssh
	addr = fmt.Sprintf("%s:%d", host, port)

	if client, err = ssh.Dial("tcp", addr, clientConfig); err != nil {
		return nil, err
	}

	// create session
	if session, err = client.NewSession(); err != nil {
		return nil, err
	}

	return session, nil

}



```
>需要补充的：包装下os.Stdin接口，加入点显示什么的
>ssh用来执行远程批量任务比较好。这个先写下来，回头实现下。
>解决下windows下os.Stdin结束的行尾存在'\r\n'问题



---
[Back](summary.md)
--