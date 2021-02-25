## Relational data

The data required for some analyses will typically be spread across multiple files that you will need to bring together. This may be because they are from entirely different sources or, alternatively, if datasets are really large, it may be more computationally efficient not to keep the files separate. 

Today, we will we working with datasets that can all be loaded into R. If your data gets really big, you will probably have to utilize some sort of relational database management system (RDBMS) -- a major example being SQL and its derivatives -- which we won't be covering in this course (but which are totally worth learning if you go into data science!).

There are a number of types of joins (ways to link up multiple datasets). Each of these performs a slightly different operation. [Here is a nice cheatsheet](https://dplyr.tidyverse.org/articles/two-table.html) that summarizes the different types of join operations in R. Basically these can be divided into three categories:

- *Mutating joins*, which add new variables to one data frame from matching observations in another.

- *Filtering joins*, which filter observations from one data frame based on whether or not they match an observation in the other table.

- *Set operations*, which treat observations as if they were set elements.

To get practice using these, we will be working through the examples in the [R for Data Science book chapter on Relational Data](https://r4ds.had.co.nz/relational-data.html). (I apologize for using the 'textbook' examples; good relational datasets are hard to find and assemble!)

## Exercises

1. Read section 13.2 and complete Exercise 13.2.1 (Questions 1, 2, and 3 only). Write down your answers in a new plain text file (click *File > New File > Text File*) named `lecture-5-exercises.txt`. Add, commit, and push to GitHub.

2. Read section 13.4 and complete Exercise 13.4.6 (Questions 1, 2, and 3 only). Write down your answers and add them to you `lecture-5-exercises.txt` file. Add, commit, and push to GitHub.

3. Read section 13.5 and complete Exercise 13.5.1 (Questions 2 and 5 only). Put the necessary code into a new R file `lecture-5-exercises.R`. Add, commit, and push to GitHub. 
