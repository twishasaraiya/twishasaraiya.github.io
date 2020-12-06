I am going to review the book **Clean Code - Robert C Martin**. At the end of this blog I have listed some resoucres as well that I found during my research about clean code and code maintainibility.

> I highly recommend reading the entire book if you havent already because it will help you imbibe the skills into your daily life. However if you are looking for a short summary please continue to read

Few things I have learned and applied to the projects that I have been doing while reading this book

**1. Clean names - class, function, variable, files**

- Dont be afraid to use long names
- Intuitive names are far more better than long comments
- Not everyone who will look at your code have the same level of understanding
- Dont be afraid to take time while choosing names, it is required investment
- The names should tell a story

> My tip: The names could be intuitive to you when you read them but may not be to others. Review your code as if you are seeing it for the first time. I understood the importance of clean names when I started mentoring juniors and reviewing their code. I would ask them, are you able to understand the logic in this ? They would ask questions and that helped me realize areas where I had made assumptions in naming. Now I think in a way like, " I have to write the code not just to make it work but in a way, that it would be easy to for next developer to read and understand" 

**2. Short Functions - that do one thing**

This was were I was very confused as to what is the one thing and how do I describe/identify one thing. This book helped me understand the kind of the abstraction that could be done on functions. What I learned?

 - Is my current function clean ? If the answer to all questions below is yes, then :thumbs-up:
 - How to do abstraction ? Ensure that all the steps in the function are at same level of abstraction
 - How to make your functions readable? If your function is doing one thing then it should be easy to describe it with a name
 - Are you passing more than 3 arguments? Pass these arguments as object
 - How much is the level of indentation ? Ensure that the level of indentation is not more than 2. If it is more, check if that piece could be extracted into another function

> My tip: It is not necessary that you should be able to write a clean function in the first go itself. But you should definitely iterate over and over until you feel satisfied with what you have written. Software development is an iterative process.

**3. Comments**

All developers write comments, but what matters is how truthful or helpful they are. This book points out some really good points to keep in mind when you think of writing comments. Listing down some important points that I have seen people do while reviewing code

For comment in between code, ask yourself
- Do I really need to write this comment? Is my code itself not able to clearly explain the concept.
- These code comments could be misleading as the code evolves and other developer doesnt put in the effort to make them reliable

TODO comments
- It is not a good practice to leave half baked code in the system to do it at later point of time

Commented out code 
- This type of comment makes it really difficult for other developers to read the code
- Secondly it is also misleading in terms of whether it is important or not ? Should I remove it or not?

> My tip: For developers writing code, ensure that your comments are not pointing obvious things that your code can already explain. Code is the source of truth, let is explain most of the things. Write comments to clarify, enhance your code rather than to add noise. For reviewers, ensure that there are proper comments if some piece of code is clearly explainable. Ensure that TODO comments or commented out code is not left out in your system.

> Fun thread: I found this thread while reading about importance of quality of comments. Hope you enjoy it. [What is the best comment in source code you have ever encountered?](https://stackoverflow.com/questions/184618/what-is-the-best-comment-in-source-code-you-have-ever-encountered)
