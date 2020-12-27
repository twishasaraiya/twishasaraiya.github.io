---
name: Goroutines and Channels
layout: single
---

### Getting started with goroutines and channels as beginners

Go routines are essentially like thread. If I can say it in a simple way, it is a concurrently running task. We have one `main` go routine that runs by default and executes all the tasks, like the exercises that we have been doing so far


To create a new goroutine we can simply do

```go
go functionName()

// or we can create an anyonymours function inline

go func(){
    // do something
}()

// the () invokes the function immediately after the declaration

```

So in a normal scenario, lets say we do something like

```go
package main

import (
	"fmt"
)


type Resp struct {
	idx int
}

func functionName(idx int) (Resp, error){
	fmt.Println("API CALL for idx = ", idx)
	return Resp{idx: idx}, nil ;
}

func main() {
	data, _ := functionName(0)
	
	fmt.Print(data)
	
}

// the output is
/**
API CALL for idx =  0
{0}
*/
```

Now to make it concurrent, simply doing, wont work. **Why?** Lets see
```go
package main

import (
	"fmt"
)


type Resp struct {
	idx int
}

func functionName(idx int) (Resp, error){
	fmt.Println("API CALL for idx = ", idx)
	return Resp{idx: idx}, nil ;
}

func main() {
	var data Resp
	go functionName(1)
	fmt.Print(data)
	
}

// output is 
// {0}
```

Few points to note here are
1. Cannot directly assign the goroutine to a variable like this `data := go functionName()` it results in error
2. If you look at the output it shows the default values, whereas we passed idx as 1 and also the message "API call for idx" was not printed


This happens because as soon as we start new goroutine, the function call moves to another thread and the main goroutine continues with the next task. In this case it printed data and exited. As per the note, I mentioned earlier, once the main goroutine ends, all other executing goroutines are terminated as well. 

Now we need the resp from the function call, just as before, right? This is where **Channnels** come into the picture


### Channels

Channel as the name suggests, it is a path for communication between goroutines. Go has a specific type `chan` for creating a channel. Read more about it in [official doc](https://golang.org/ref/spec#Channel_types). A channel provides two way communication between the goroutines ie it can be used for both sending and receiving the data


To create a new channel

```go
// create a channel for int data values
ch := make(chan int)
```

For the above usecase we rewrite or modify the code slightly

```go
package main

import (
	"fmt"
)

// create a type that will be passed between go routines
type Api struct {
	resp Resp
	err error
}

// define a type that will be the API response type
type Resp struct {
	idx int
}

func functionName(idx int, ch chan Api) (Resp, error){
	fmt.Println("API CALL for idx = ", idx)
	ch <- Api{ resp: Resp{idx: idx}, err: nil }
	return Resp{idx: idx}, nil
}

func main() {
	var data Api
	ch := make(chan Api)  // create a channel
	go functionName(1, ch) // pass the channel as argument
	data = <-ch //  receive the data from channel once it sent from their
	fmt.Print(data.resp, data.err)
	
}

// output
// API CALL for idx =  1
// {1} <nil>
```


Few points to note
1. The line `data = <-ch` is a blocking operation, ie it will wait until a response is received from the channel. So lets say the API response is returned after 5 secs, this line will keep on waiting.
2. Closing the channel. As a beginner in Go and from my reading at this point in time, you should close channels from the sender side and not from the receiver side. The `chan` type has a method `close(channelName)` to close the channel. In our scenario close will be called once we have sent the data in `functionName()` and will not be sending anymore 

[Try it out](https://play.golang.org/)


> **Note: All the goroutines are terminated automatically once the main goroutine ends.Irrespective of whether the goroutine is completed or not, it will be terminated**


### Resources
- [Go By Example : Goroutines](https://gobyexample.com/goroutines)
- [A tour of Go : Concurrency](#)
- 