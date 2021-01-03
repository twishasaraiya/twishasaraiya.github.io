---
name: How to build a URL Shorterner in Go - Part 2
date: 22-12-2020
layout: single
---

## In Part 2, We explore how to accept different file formats(YAML, JSON) and parse them

This is part 2 of the two series blog post. In this blog post I am going to talk about things I learned while implementing the bonus section of the URL shorterner excercise from [Gophercises](https://courses.calhoun.io/lessons/les_goph_04)

- [In Part 2, We explore how to accept different file formats(YAML, JSON) and parse them](#in-part-2-we-explore-how-to-accept-different-file-formatsyaml-json-and-parse-them)
  - [YAML Parser](#yaml-parser)
  - [JSON Parser](#json-parser)
  - [Command line flags](#command-line-flags)

### YAML Parser

- Package: [yaml.v2](https://godoc.org/gopkg.in/yaml.v2)
- The YAML package is not present by default in $GOROOT/$GOPATH. So we need to fetch and download the dependencies locally
  ```go
    go get gopkg.in/yaml.v2
  ```
- Under the hood, since our repository is module aware(we have a `go.mod` file), these dependencies are installed at module level instead of the legacy GOPATH level
- You can list all the modules 
  ```go
    go list all
    // or check all the modules at repo level
    go list -m all
  ```
- Read more about [go commands](https://golang.org/cmd/go/#hdr-Legacy_GOPATH_go_get)

- The `YAMLHandler()` takes a array of bytes as input. So while calling the function we convert string to byte
  ```go
  []byte(yaml string)
  ```
- The bytes need to parsed(ie. to be converted to a standard structure) and then build the map
- We define a struct like so that we can convert any input type(be it yaml, json or any other) in the future

```go
type Path struct{
    Path string
    Url string
}
```
- Read more about creating and accessing Structs type at [go by example](https://gobyexample.com/structs)
- To parse the YAML we use `Unmarshal` method
- The unmarshal method takes bytes as input(thats y we converted the string to yaml earlier) and second paramter is a reference. As per docs it can be maps or pointers. So inorder to pass a pointer, we need to create a variable of type `Path` that we created above
```go
// parseJSON()
path := make([]Path, 0)
err := json.Unmarshal(data, &path)
if err != nil {
	return nil, err
}
return path,nil

```
- Now url-shorterner-part-IIthat we have the data from YAML file into our standard defined structure we convert it to a map using `buildMap()`. This method would be a generic one that would always take a the `Path` data type and build a `map[string]string` from it. This method is not concerned with where the original data comes from be it YAML, json or simple string. So we have decoupled the responsibilities
```go
// buildMap
m := make(map[string]string, len(parsedData))
for _, each := range parsedData {
	m[each.Path] = each.Url
} 
return m
```
- Thats it, now we just need to call these functions in `YAMLHandler`
```go
parsedYAML, err := parseYAML(yml)
if err != nil {
	return nil,err
}
pathMap := buildMap(parsedYAML) 
fmt.Print(pathMap)
return MapHandler(pathMap, fallback), nil
```

### JSON Parser

- Similar to YAML handler, we can also have a requirement to accept JSON files as well 
- We need to just write 2 more functions
`JSONHandler` which is very similar to the `YAMLHandler` and a `parseJSON` method. 
- To parse the input JSON and convert it to `Path` type we use Package **[encoding/json](https://golang.org/pkg/encoding/json/)** as shown below 
- Read more about working with JSON [here](https://blog.golang.org/json)
```go
path := make([]Path, 0)
err := json.Unmarshal(data, &path)
if err != nil {
	return nil, err
}
return path,nil
```
- Now you can just change the input in `main` module and pass the jsonHandler to the server

### Command line flags

- Additionally you can also take the file as input through the command line 
- As seen in previous [blog posts](2020-12-20-quiz-game.md), we use the **flag** package
```go
flag.String("yml", "handler.yml", "a yaml file in the format of path and url to add routes")
flag.Parse()
```
- Since so far our input were static variables defined in the files, but now the data is coming from files right.
- So we need to use **io/ioutil** package to read the data from these files
- In earlier post, we had seen `os.Open` method to read files but here we will use `ioutil.ReadFile` . Why ? because the `Open` method returns the data as [File](https://golang.org/pkg/os/#File) type whereas `ReadFile` returns data in byte. 
- There is nothing wrong with using Open but its just that we would need to extra method to convert the type or change our functions. So at this point I decided to use `ReadFile` only