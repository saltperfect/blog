---
title: "Docker Experiments"
date: 2022-02-05T13:01:23+05:30
draft: true
---

# Mapping current directory to container directory for realtime updates.

It's a Sunday afternoon and you decided to hack something up but when you started to install those creepy deps, you get ERRORS!!. To solve this you spend a lot of time on stackoverflow. Maybe you find some resolutions they want you to run some unknown commands you don't want to mess up you computer. This happens with the best of us and instead of hacking away you end up in dependency hell.

Sure you could write your code and install dependencies in docker and copy over you code to run there but that would take a painful amount of time. But wait, there's more... 

What if I told you could get a environment using docker and can map your personal directory with one of the directory in the docker container. This way you will get real time updates of your computer's directory.

Sound Fun, Let me show you how.

Make a simple docker File in empty directory

``` Dockerfile
FROM golang:1.10

```

In the same directory make main.go

``` go
package main
func main() {
    fmt.Printf("hello world!\n")
}
```
From the current directory run `docker build -t simple-image .`

This would create an image with name simple-image

From the current directory run `docker run --rm -it -v $(pwd):/go/src/app simple-image`

1. --rm to remove the image after running
2. -it to get inside the container
3. -v is for mapping you current directory $(pwd) to /go/src/app directory which would be created if not present in docker container

This will log you in to the container and if you `run cat src/app/main.go`
you will be able to see your main.go file which you just created in your computer.

```
root@8d0ff4bde24b:/go# cat src/app/main.go 
package main

import "fmt"

func main() {
        fmt.Printf("hello world!\n")
}

```

Now you can change you main.go and build it from inside the container with the required dependencies that you want.
Moreover you could also get the outputs from the container back to you current directory as well.

Please share if you liked the article.


