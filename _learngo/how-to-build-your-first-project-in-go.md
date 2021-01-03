---
name: How to build a Quiz Game in Go
date: 20-12-2020
layout: single
---

### Exercise 1 of Gophercises - Build a terminal based Quiz game

**Have you just started learning go?**

**Are you one of those who learn by doing projects?**

**Are you looking for a project to get your hands dirty with go?**

With these questions in mind I started with my first project in Go. I started this after 1 day of going through the syntax of Go using [Tour of Go](https://tour.golang.org/) in the official documentation.

That is when I came across [Gophercises](https://gophercises.com/) and started with the first exercise of it. 
Without much blabbering, lets break down the problem into high level steps and then search for each of them

- [Exercise 1 of Gophercises - Build a terminal based Quiz game](#exercise-1-of-gophercises---build-a-terminal-based-quiz-game)
- [How to read a local file?](#how-to-read-a-local-file)
- [How to parse csv?](#how-to-parse-csv)
- [Score calculation](#score-calculation)
- [How to output to the terminal?](#how-to-output-to-the-terminal)
- [Command line flags](#command-line-flags)
- [What else can be do?](#what-else-can-be-do)
- [Resources](#resources)


### How to read a local file?

- Package [os](https://golang.org/pkg/os/)
- I have used the **os** package but the `Go by example` provides another way of [reading file](https://gobyexample.com/reading-files) as well. You can explore that as well
- The `Open` method takes string file name or the absolute path as input and returns a `File`, if not then the second value is the `error`
- We handle the error by checking if it is not nil and simply log out the error for our first project and dont do any error handling

```go
// main.go
import (
    "os"
)

var filePath = "problems.csv"
file, err := os.Open(filePath)
if err != nil {
	log.Fatal(err)
}
```

- We have seen how to read local file, but we still cant access the question since it is of type `csv`. We now look into how to parse the csv files and extract data in the next section
  
### How to parse csv?

- Package [csv](https://golang.org/pkg/encoding/csv/)
- The `NewReader()` method reads the file that we got from the `os.Open()`
- The method returns an instance of type `Reader`. 
- The default delimiter is comma, but other can be specifed as well by modifying the `Reader`

```go
reader := csv.NewReader(file)
questions, err := reader.ReadAll()
```

- Now that we have the parsed csv file, we can ask these questions to the user and calculate the scores based on the answer provided by them


### Score calculation

- We have the question and answer pair as array of slices, where first value if the question and second is the correct answer
- We define a score variable of type `int`
- Then Loop through all the questions
- The `range` returns two values. First is index and second is the slice consisting of question and answer (i.e each row in the csv file)
- We also import the `strconv` package to convert the correct answer from csv file to be of type int, so that we can compare it with the value provided by the user. 
- To convert the string type to int, we use `Atoi` method.
- The `_` allows to us to not provide any variable name since we do not plan to use them 

```go
score := 0
var answer int
var correctAnswer int
for _, row := range questions {
	fmt.Println(row[0])
	fmt.Scanf("%d", &answer)
	correctAnswer, _ = strconv.Atoi(row[1])
	if answer == correctAnswer {
		score += 1
	}
}
```

- Now that we have calculated the scores and we have the final answer in `score` variable. We will look at simply output them to the terminal

### How to output to the terminal?

Package: [fmt](https://golang.org/pkg/fmt/)
- The final score is stored in `score` variable
- There are various output methods available. But for simplicity we use `Print()`
- User input can be taken using `Scanf`
- If you are already know c/c++, like me, you must already be familiar with `printf` and `scanf`. Go uses similar methods for output
- For simplicity purpose, we just output the final score.

```go
fmt.Print("Your score is",score)
```

- Next we explore if we can dynamically provide the path of the csv file while running the command instead of defining a static file path in the code.

### Command line flags

- Package: [flag](https://golang.org/pkg/flag/)
- This package allows us to provide the csv file via the command line
- We define a flag of type string with default value as `problems.csv`
- The flag returns location to the file name provided by the user. So we use `*fileName` to read its value

```go
var fileName = flag.String("csv", "problems.csv","a csv file in the format of 'question,answer' (default 'problems.csv')")
	flag.Parse()
	file, err := os.Open(*fileName)
```

Additional to see all flags available we can the following commands

```go
go run main.go --help
// or
go run main.go -h
```

- With this, the main goal of the project to design a quiz game with basic functionality is completed.
- Lets take a break and come back to get do some more optimizations and additional exploration

### What else can be do?

- In this section, I compared my code to the official solution provided and found these things that could be done better.


- Define a type of the problem
```go
type problem struct{
	question string
	correctAnswer string
}
```

- Invalid csv input. If the csv contains strings

```go 
import "strings"
for idx,row := range rows {
	data[idx] = problem{
		question: row[0],
		correctAnswer: strings.Trimspace(row[1]),
	}
}
```

### Resources

- [Github Source Code](https://github.com/twishasaraiya/learngo/tree/master/quiz-game)