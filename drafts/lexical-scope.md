---
name: Lexical Scope in Go
layout: single
---

## What is Lexical Scope in Go ? 

<!--Explain the conecpt similar to javascript -->

What does lexical scope mean? To simply answer this, it means that it contains the reference to the parent environment/ surrounding state.

For example: In the case below the output is as expected.

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

What if we didnt have variable `a` in main function?
For example

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

What happens here is that it did not find in the scope of function main so it went one level up to the parent and checked for global variable with name `a` and outputs it value as hello

Also if we keep both the variables (global and function level), what would be the output in that case?

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

The go language takes the function level variable `a` and prints its value


Lets take an another example. Here we define the variable `b` after the main function but we access it before it is declared and intialized.

```go
package main

import (
	"fmt"
)

var a string = "hello"

func main() {

	var a int = 1
	//var x = b		
	fmt.Println(a)
	fmt.Println(b)
}

var b string = "world"

// Output
// 1
// world
```

In the example we see even if it is declared later, we are still able to access it


> **Note: We cannot have non-declaration statement outside the function** What this means is we cannot have `x:= 1` or
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

For the last example, can you guess the output of this code snippet

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

Why? The if block creates a new variable `x` whose scope is limited to that if block only and hence its value is 2 inside the block and outside the block the x reference to the `x` with value 1. 

For `y`, it could not find any variable that had scope of if condition. So it access and updates the value of y in the outer scope. 

> **Note** If you are familiar with javascript, then must have already known this concept. However there is one thing that I found Golang does not support hoisting whereas javascript does

Try out these examples in the [playground](https://play.golang.org/) for yourself

Read more about [Declarations and Scope](https://golang.org/ref/spec#Declarations_and_scope) in the official documentation
