---
title: "TCP echo server"
date: 2021-10-11T19:11:30+05:30
---

Let go through and try to understand how easy it can be to send and receive messages on tcp socket.

Lets first talk about the server and how to build one. 

``` go
package main

import (
	"fmt"
	"net"
)

func main() {
	listener, err := net.Listen("tcp", ":8080")
	if err != nil {
		panic(err)
	}
	defer listener.Close()
	for {
		conn, err := listener.Accept()
		fmt.Println("request received")
		if err != nil {
			panic(err)
		}
		go handleConnection(conn)
	}
}

func handleConnection(conn net.Conn) {
	for {
		messages := make([]byte, 128)
		n, read_err := conn.Read(messages)
		if read_err != nil {
			fmt.Println("failed:", read_err)
			return
		}
		fmt.Println("Got: ", string(messages[:n]))
		_, write_err := conn.Write(messages[:n])
		if write_err != nil {
			fmt.Println("failed:", write_err)
			return
		}
	}
}
```
`net.Listen` returns a listener which can accept multiple connections

for each connection we can handleConnection in a separate goroutine. If you notice our handleConnection runs in a infinite loop and never exists. This is done to enable listening on the connection indefinitely. 

`conn.Read` is a blocking call and will wait until something is written on the connection.

`conn.Write` will write back to the connection

Now comes the part of client

``` go
func main() {
	conn, err := net.Dial("tcp", "localhost:8080")
	if err != nil {
		log.Fatalf("Failed to dial: %v", err)
	}
	defer conn.Close()

	scanner := bufio.NewScanner(os.Stdin)
	messages := make([]byte, 128)
	for {
		fmt.Println("enter the message to send")
		scanner.Scan()
		message := []byte(scanner.Text())

		_, write_err := conn.Write([]byte(message))
		if write_err != nil {
			fmt.Println("failed:", write_err)
			return
		}

		n, read_err := conn.Read(messages)
		if read_err != nil {
			fmt.Println("failed:", read_err)
			return
		}

		fmt.Printf("read %d bytes, echo: %s\n", n, messages[:n])
	}
}
```

`net.Dial` dials in and tries to create a connection

`scanner.Scan` scans for the input

`conn.Write` writes to the connection

`conn.Read` is a blocking call and will wait until something is present to receive

again our whole logic is wrapped around a infinite loop so we are able to send and receive message until the application crashes.

You can find the complete code [here](https://gist.github.com/saltperfect/fb6d7a313aefc163ba27ac3790a0bbd0.js")
