---
name: How to build a HTML Parser
layout: single
---

## We explore html parsing methods, go functions and methods, lexical scope

- Build a package that extracts all links from the provided HTML page
- Things Learned
  - [Problem Statement](#problem-statement)
  - [Parsing Html](#html-parser)
  - [Functions vs Methods](#functions-and-methods)
  - [Lexical Scope](#lexical-scope-variables)
  - [String methods](#various-string-methods)
  - [Go Constants](#constants-in-go)


### Problem Statement

Read more about the problem statement [here](https://gophercises.com/)

### Parsing HTML

Lets start with a html as string for simplicity purpose

```go
doc := `<html>
<body>
  <h1>Hello!</h1>
  <a href="/other-page">A link to another page</a>
</body>
</html>`
```

Now we need to convert this html into DOM tree format. For this we use the package [html](https://godoc.org/golang.org/x/net/html). In this package which focus on the method `Parse()` that returns [Node](https://godoc.org/golang.org/x/net/html#Node) and `err`. So the Node contains the `html` ElementNode as the root of the DOM tree. And as per the documentation as part of `Node` type we get children of the node as well.

The package can be used this way. The Parse method requires a `io.Reader` as input parameter

```go
import "golang.org/x/net/html"

func main(){
  // doc is initialized here
  //....

	// create a new IO reader
	r := strings.NewReader(doc)
	// parse HTML, which returns doc and err
	nodes, err := html.Parse(r)
	if err != nil {
		panic(err)
  }
  
  //...
}

```

- `Attr` field contains the HTML properties like `href`, `src` etc
- `Type` of the node can be ElementNode(like `a`, `div`, `p` etc), TextNode(that contains the data within elementNodes) and CommentNode(which contains all the comments in your HTML)


### Functions and Methods
<!-- 
1. difference btw fucntions and methods with examples 
2. return types in function
3. Reseacrh first class functions just like in javascript - func can be passed around 
-->

Functions and methods maybe used in the same context in other languages but in Go, they have different meanings and usecases

**Definition**

- Functions
```go

```

- Methods
```go

```

**Functions Cheatsheet**

| Description| Syntax |
| ----------- | ------------------------------- |
| Input Parameters - The data that is passed as input to the function. Syntax: <variableName> <Type>. Incase multiple variable have same type it can also be defined as <var1>, <var2> <Type>                       | ```go func name(abc string)```                |
| Return Type - The type of data that is returned from the function. The return Type is defined after input paramerter. In case of multiple returns values it can be specified as follows : `(string, int, <type>)` | ```go func name(abc string) string { ... }``` |


Not having used methods much I will not diving deep into it in this blog. Maybe a separate blog in future

### Lexical Scope variables

### Various string methods
<!-- compare them to javascript -->

For this excercise, we need use various string methods for comparsion, checking the prefix and stuff. Coming from javascript background, I will try to put across equivalent names in both of them for reference

Package : [strings]()

| Javascript method                        | Go Function                             |
| ---------------------------------------- | --------------------------------------- |
| `<originalString>`.startsWith(`<subString>`) | `<originalString>`.hasPrefix(`<substring>`) |
| `<originalString>`.endsWith(`<subString>`) | `<originalString>`.hasSuffix(`<substring>`) |


### String ' vs ""

### Exported vs UnExported
<!-- if u dont want to export can start with small case 
but if u want to export and use it elsewhere start with a upper case as a best practice
-->


### Algorithms in Go
<!--DFS/BFS, if required try to explain it or find our about data structures in Go-->

Coming back to the problem, we had the DOM tree right? Now we need to get a `a` links and all the nested text within those links. We want to ignore the commented code, the nested `ElementNodes` and nested `a` tags. We just need the `text` within those anchor tags joined together 

### Constants in Go
<!-- https://tour.golang.org/basics/15 -->

Just like we have `const` in Javascript. Once the constants are defined they cannot be modified. Similar in Go we have the `const` keyword

Read more about it [here](https://tour.golang.org/basics/15)