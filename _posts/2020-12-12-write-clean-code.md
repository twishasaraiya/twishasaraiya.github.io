---
title: "Write clean code using these 5 simple tips"
description: "On improving your coding style by writing clean code and maintaining readability of your codebase"
---

### 5 Essential takeways from Clean Code By Uncle Bob

As developers we can write code but the question that I started to look for is the code I am writing clean? Is it making the lives of next developer easier? 
Or as a reviewer how do I ensure that the code is readable and maintainable ? What are the basic things to look for ? This is when I came across the the book **Clean Code - Robert C Martin**

In this blog I am going to write a review of the book and what I learned while reading it. Obviously I cannot cover the entire content of the book as part of the review and I have no plan to do as well. So I am only going to talk about the basic important things. At the end of this blog I have listed some resoucres as well that I found during my research about clean code and code maintainibility.

> I highly recommend reading the entire book if you havent already because it will help you imbibe the skills into your daily life. However if you are looking for a short summary please continue to read

Few things I have learned and applied to the projects that I have been doing while reading this book

**1. Clean names - class, function, variable, files**

- Dont be afraid to use long names
- Intuitive names are far more better than long comments
- Not everyone who will look at your code have the same level of understanding
- Dont be afraid to take time while choosing names, it is required investment
- The names should tell a story

> My tip: The names could be intuitive to you when you read them but may not be to others. Review your code as if you are seeing it for the first time. I understood the importance of clean names when I started mentoring juniors and reviewing their code. I would ask them, are you able to understand the logic in this code ? They would ask questions and that helped me understand the areas where I had made assumptions in naming. Now I think in a way like, " I have to write the code not just to make it work but in a way, that it would be easy to for next developer to read and understand" 

**2. Functions - that do just one thing**

This was where I was very confused as to what is the one thing and how do I describe/identify one thing. This book helped me understand the kind of the abstraction that could be done on functions.

 - How to do abstraction ? Ensure that all the steps in the function are at same level of abstraction.
 - How to make your functions readable? Does the function name justify the purpose of the function? If your function is doing one thing then it should be easy to describe it with a name
 - Are you passing more than 3 arguments? Pass these arguments as object rather than passing all of them as separate variables
 - How much is the level of indentation ? Ensure that the level of indentation is not more than 2. If it is more, check if that piece could be extracted into another function
 - Is my current function clean ? If the answer to all questions above is yes, then well done :thumbs-up: If not, go through each of the points and work on them individually and iteratively.


> My tip: It is not necessary that you should be able to write a clean function in the first go itself. But you should definitely iterate over and over until you feel satisfied with what you have written. Software development is an iterative process. Slowly and gradually it will come to you naturally.

**3. Comments - Make them helpful**

All developers write comments, but what matters is how truthful or helpful they are. This book points out some really good points to keep in mind when you think of writing comments. Listing down some important points that I have seen people do while reviewing code

 - For comment in between code, ask yourself, Do I really need to write this comment? Is my code itself not able to clearly explain the concept.These code comments could be misleading as the code evolves and other developer doesnt put in the effort to make them reliable

- For TODO comments in code, ask yourself when will I get back on this? Am I leaving it for the next developer to do my work? It is not a good practice to leave the half baked code in the system to do it at later point of time. Avoid using them if you are not going to do it soon.

- For commented out code, ask yourself shouldn't I just remove? If anytime I want this back I can get it from commit history. Keeping functions with same name with one being commented and one being used is a code smell. First, this type of comment makes it really difficult for other developers to read the code. Secondly it is also misleading for next developer in terms of whether it is important or not ? Should I remove it or not? They would not have the courage to remove it, only you have that knowledge.

> My tip: For developers writing code, ensure that your comments are not pointing out obvious things that your code can already explain. Code is the source of truth, let is explain most of the things. Write comments to clarify, enhance your code rather than to add noise. For reviewers, ensure that there are proper comments if some piece of code is not clearly explainable. Ensure that TODO comments or commented out code is not left out in your system.

 **Fun thread** :laughing: : I found this thread while reading about importance of quality of comments. Hope you enjoy it. [What is the best comment in source code you have ever encountered?](https://stackoverflow.com/questions/184618/what-is-the-best-comment-in-source-code-you-have-ever-encountered)

**4. Clean Tests - keep them updated**

Tests are equally important part of the development as the code itself. The main factor that makes test clean are readability

- One test should test one thing. As said in the book One Asset per test
- Keep your test updated as the code evolves.
- Tests should run in any environment. The test should not be specific to the environment
- The test should have boolean output. Either they pass or fail


**5 Clean Classes - Keep it short**

 - As you might already have understood, the classes should be small in size.
 - A class should have one responsibility ie only a single reason to change. It is also called as Single Responsibility Principle. Identify the different responsibilities
 - Class name should be able to explain the responsibility of the class
 
 > My Tip: Think about what you need to perform to get the desired result. Write them down as high level steps. Check if each step is independent and if the step could be broken into smaller independednt steps. Convert each step to a class. For example, I want to read a some files(of type zip,tar) and perform some operation on those files. Now I could create a single class, pass the file path to the constructor and create two methods `read()` and `process()`. But instead I can separate the responsibilties. I can create a separate class to do read the file contents. The responsibility of this class is to be read the files(of any type) and of any location(we could provide a local path or a link to file stored somewhere). Then create a separate class and pass a instance of the reader class to the processor class and use it. The processor class responsibility is to do perform all the required operations on the file contents. 


## Resources

- Youtube series on **[Clean Code - Uncle Bob](https://www.youtube.com/watch?v=7EmboKQH8lM)**
- **[The Art of Code Comments - Sarah Drasner](https://www.youtube.com/watch?v=yhF7OmuIILc)** at JSConf
