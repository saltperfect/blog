---
title: "Context"
date: 2022-02-03T12:42:52+05:30
draft: true
---

The context package in go provides a path to cancel and propagate cancelling information around the methods and even across http servers.

Consider you request your server with some information but want to cancel that request halfway, or maybe somehow the connection get drops. The ability to propagate that information to server and trigger cancellation of that request will save you lot of computations.

The below code is in example implementation of exactly this.

# Server Code

``` go
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"
)

func main() {

	http.HandleFunc("/", rootHandler)

	log.Fatal(http.ListenAndServe("localhost:8080", nil))
}

func rootHandler(w http.ResponseWriter, r *http.Request) {
	log.Printf("handler started")
	defer log.Printf("handler ended")

	// access context here
	ctx := r.Context()
	// selecting if our work got done of context was cancelled
	select {
	case <-time.After(5 * time.Second):
		fmt.Fprintln(w, "hello")
	case <-ctx.Done():
		log.Printf("context reached, %s", ctx.Err().Error())
		http.Error(w, ctx.Err().Error(), http.StatusInternalServerError)
	}

}
```

Now let's look how we can actually cancel the context

# Client Code
``` go
package main

import (
	"context"
	"io"
	"log"
	"net/http"
	"os"
	"time"
)

func main() {

    // create context and add cancel context to it, calling cancel will trigger ctx
	// which can be interpreted on server side
	ctx := context.Background()
	ctx, cancel := context.WithCancel(ctx)
    time.AfterFunc(time.Second, cancel)
    // creating request to attach ctx to it
	req, err := http.NewRequest(http.MethodGet, "http://localhost:8080", nil)
	if err != nil {
		log.Printf("error: request made: %v", err)
	}
	req = req.WithContext(ctx)
	// executing the request
    resp, err := http.DefaultClient.Do(req)
	if err != nil {
		log.Fatalf("error while getting, %v", err)
	}

	if resp.StatusCode != http.StatusOK {
		log.Fatalf("error: response, %v", resp.Body)
	}
	defer resp.Body.Close()
	io.Copy(os.Stdout, resp.Body)
}
```

By calling cancel we quickly getting the response back from server without the 5 second delay.


# Passing Values in Ctx

Context package gives the ability to pass values in the context as well.

Rule of thumb while passing values, the value should have the impact on how the program behaves. A good example could be passing uuid's, request'ids.
