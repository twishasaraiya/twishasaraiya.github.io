I am going to review the book **Clean Code -**. At the end of this blog I have listed some resoucres as well that I found during my research about clean code and code maintainibility.

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
 - 

> My tip: It is not necessary that you should be able to write a clean function in the first go itself. But you should definitely iterate over and over until you feel satisfied with what you have written. Software development is an iterative process.

**3. Comments**
