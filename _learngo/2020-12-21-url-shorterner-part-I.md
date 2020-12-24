---
name: How to build a URL Shorterner in Go - Part 1
date: 21-12-2020
layout: learngo
---

## URL Shorterner

> The goal of this exercise is to create an http.Handler that will look at the path of any incoming web request and determine if it should redirect the user to a new page, much like URL shortener would.

- [Problem Statement on Gophercises](https://courses.calhoun.io/lessons/les_goph_04)
- [URL Shorterner](#url-shorterner)
  - [Create a new module](#create-a-new-module)
  - [Understanding Map](#understanding-map)
  - [Create a http server](#create-a-http-server)

### Create a new module

- Create a new file `handler.go` at root level. The name of the new module is defined at the first line
```go
package urlshort
....

```

- When importing the package in `main.go`, you will face an error like 
`cannot find package "urlshort" in any of:
	/usr/local/go/src/urlshort (from $GOROOT)
	$HOME/src/urlshort (from $GOPATH`

What this means is, go is looking for the package name in $GOROOT and $GOPATH location but it cannot find it there. Obviously since we have created a new package in our new folder it would be present in the location that is go searching for. How do we tell golang where to look?

```go
go mod init urlshort
// in general
// go mod init package-name
```

This will create a `go.mod` file at the root level. The file defines the path of the module, which is also the import path used while importing the custom package

Now we see a new Error something like

```go
found packages urlshort (handler.go) and main (main.go) in /home/twisha/Desktop/go/url-shorterner
```

What this is ? To understand it, lets first see **what a module is?** A module is a collection of related packages. **What is a package?** A package is a directory in the go project consisting of multiple go files. 

In our current scenarios we have go found 2 module at the root level. One is urlshort and other is main. Typically, a go project contains only one module at the top level. To resolve we can do 2 things

**One**: Change the package name in handler.go. Now both go source files belong to the same main module
```
package main
```

**Two**: Create a folder structure and keep urlshort module at the top level

```
main/main.go
go.mod
handler.go
```

> **Note: Doing either one of them will resolve the issue**


- Aliasing imported packages
```go
import (
    us "urlshort"
)
```

- Read more about [How to write go code](https://golang.org/doc/code.html)


### Understanding Map

- As seen in other languages, map is a dictonary to store key-value pairs
- Maps cheatsheet

```go
// create a new map[keyType]valueType

var m map[string]string

// m is nil currently
// writing m["root"] will result in error

// so we need to initialize the map

m = make(map[string]string)

// now the map acts as an empty object and allows us to do 

val := m["root"]
// the value of the key that does not exist will be empty string ""

// add/update the key
m["root"] = "root"

// check if key exists
// returns two values
// if exists
// val = "root", ok = true
// else
// val = "", ok = false
val, ok := m["root"]



// get length of map
n := len(m)

// delete key in map
// delete(map instance, key name)
delete(m, "root")

```

- Play around with maps [here](https://play.golang.org/)
- Read more about [Map](https://blog.golang.org/maps)


### Create a http server

- Package: [net/http]()
- Create a route handler and start a server. The handler takes two parameters, first the response object that we can modify and send back and second is the request that was received.
- The properties and methods in type `Request` are [here](https://golang.org/pkg/net/http/#Request) and `Response` are [here](https://golang.org/pkg/net/http/#ResponseWriter)

```go
func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hi there, I love %s!", r.URL.Path[1:])
}

http.HandleFunc("/", handler)
//http.ListenAndServe(PORT string, nil)
http.ListenAndServe(":8080", nil)
```
