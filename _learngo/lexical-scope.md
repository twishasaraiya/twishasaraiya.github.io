---
name: Lexical Scope in Go
layout: single
---

## What is Lexical Scope in Go ? 

<!--Explain the conecpt similar to javascript -->

What doesHeamorrage lexical scope mean? To simply answer this, it means that it contains the reference to the parent environment/surrounding state.

For example: In the case below, what would be the output?

```go
package main

import (
	"fmt"
)

func main() {
 	var a int = 1
	fmt.Print(a)
}

// Output
// 1
```

The output was as expected right?
What if we didnt have variable `a` in main function?
For example:

```go

package main

import (
	"fmt"
)

var a string = "hello"

func main() {
	fmt.Print(a)
}

// Output
// hello

```

What happens here is that it did not find `a` in the scope of function main so it went one level up to the parent and checked for global variable with name `a` and outputs its value as hello

What happens if we keep both the variables (at global and function level)? what would be the output in that case?

```go
package main

import (
	"fmt"
)

var a string = "hello"

func main() {

	var a int = 1
	fmt.Print(a)
}

// Output
// 1
```

The language takes the function level variable `a` and prints its value


Lets take an another example. What do you think would be the password

```go
package main

import (
	"fmt"
)

var a string = "hello"

func main() {

	var a int = 1
	fmt.Println(a)
	fmt.Println(b)
}

var b string = "world"

// Output
// 1
// world
```
Here we define the variable `b` after the main function but we access it before it is declared and intialized.We see even if it is declared later, we are still able to access it


> **Note: We cannot have non-declaration statement outside the function** What this means is we cannot have declarations like `x:= 1` at the global scope level

To see this point let's take an example,
```go

package main

import (
	"fmt"
)

var a string = "hello"

var b string = "hi"

func main() {
	fmt.Println(b)
}

b = "world" // this line throws an error

// output
// syntax error: non-declaration statement outside function body
```

Why did we get an error? As mentioned in the error, we cannot have `non-declaration` statement(ie. statements where are just assigning values). 

Can you guess the output of the code snippet below

```go

package main

import (
	"fmt"
)

func main() {

	var x int = 1
	y := 1	
	
	if true {
		x := 2
		y = 2
		fmt.Println(x)
		fmt.Println(y)
	}
	
	fmt.Println(x)
	fmt.Println(y)
}

```

```
Output
2
2
1
2
```

What happenend here ? The if block creates a new variable `x` whose scope is limited to that `if` block only and hence value of `x` is 2 inside the block and outside the block the x references to the `x` with value 1. 

For `y`, it could not find any variable in the scope of `if` condition. So it updates the value of y in the outer scope. 

> **Food for thought** If you are familiar with javascript, then must have already known this concept. However there is one thing that I found Golang does not support hoisting whereas javascript does. Feel free to explore more. 

Try out these examples in the [playground](https://play.golang.org/) for yourself

Read more about [Declarations and Scope](https://golang.org/ref/spec#Declarations_and_scope) in the official documentation

Do let me know your thoughts via [mail](mailto:saraiyatwisha@gmail.com)
