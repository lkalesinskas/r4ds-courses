Tabular Data Foundations
========================================================
author: Alejandro Schuler, adapted from Steve Bagley and based on R for Data Science by Hadley Wickham
date: 2019
transition: none
width: 1680
height: 1050


<style>
.small-code pre code {
  font-size: 0.5em;
}
</style>

Introduction to the course
========================================================
type: section


Goals of this course
========================================================
By the end of the course you should be able to...

- write neat R scripts and markdown reports in R studio
- find, read, and understand package and function documentation 
- read and write tabular data into R from flat files
- perform basic manipulations on tabular data: subsetting, manipulating and summarizing data, and joining
- visualize tabluar data using line and scatter plots along with color and facets

<div align="center">
<img src="https://d33wubrfki0l68.cloudfront.net/571b056757d68e6df81a3e3853f54d3c76ad6efc/32d37/diagrams/data-science.png" width=800 height=300>
</div>

Tidyverse
========================================================
<div align="center">
<img src="https://pbs.twimg.com/media/DuRP1tVVAAEcpxO.jpg">
</div>

Resources for this course
========================================================

R for Data Science (R4DS): https://r4ds.had.co.nz
<div align="center">
<img src="https://r4ds.had.co.nz/cover.png">
</div>

***

- Fundamentals: ch 1, 4, 6
- Input/output: ch 11
- Data manipulation: ch 5, 13
- Visualization: ch 3, 28

Cheatsheets: https://www.rstudio.com/resources/cheatsheets/

Getting Started in RStudio
========================================================
type: section


The basics of interaction using the console window
========================================================
The R console window is the left (or lower-left) window in RStudio.
The R console uses a "read, eval, print" loop. This is sometimes
called a REPL.
- Read: R reads what you type ...
- Eval: R evaluates it ...
- Print: R prints the result ...
- Loop: (repeat forever)


A simple example in the console
========================================================
- The box contains an expression that will be evaluated by R, followed by the result of evaluating that expression.

```r
> 1 + 2
[1] 3
```
- `3` is the answer
- Ignore the `[1]` for now. 

- R performs operations (called *functions*) on data and values
- These can be composed arbitrarily

```r
> log(1 + 3)
[1] 1.386294
> paste("The answer is", log(1 + 3))
[1] "The answer is 1.38629436111989"
```

How do I...
===
- typing 
```
?function_name
``` 
gives you information about what the function does
- Google is your friend. Try "function_name R language" or "how do I X in R?"
- stackoverflow is your friend. It might take some scrolling, but you will eventually find what you need

Quadratic Equation
===
type: prompt
incremental: true

One of the solutions to a polynomial equation $ax^2 + bx + c = 0$ is given by

$$x = \frac{-b + \sqrt{b^2 -4ac}}{2a}$$

Figure out how to use R functions and operations for square roots, exponentiation, and multiplication to calculate $x$ given $a=3, b=14, c=-5$.


```r
> (-14 + sqrt(14^2 - 4 * 3 * (-5)))/(2 * 3)
[1] 0.3333333
```

What did you learn? What did you notice?
- Parentheses are used to encapsulate the *arguments* to a function like `sqrt()`
- Operators like `\`, `*`, and `^` are useful for math
- Parentheses can also be used to establish order of operations

Packages
===
- The amazing thing about programming is that you are not limited to what is built into the language
- Millions of R users have written their own functions that you can use
- These are bundled together into *packages*
- To use functions that aren't built into the "base" language, you have to tell R to first go download the relevant code, and then to load it in the current session

```r
> install.packages("tidyverse")  # go download the package called 'tidyverse'- only have to do this once
> library("tidyverse")  # load the package into the current R session - do this every time you use R and need functions from this package
```

Packages
===
- The `tidyverse` package has a function called `read_csv()` that lets you read csv (comma-separated values) files into R. 
- csv is a common format for data to come in, and it's easy to export csv files from microsoft excel, for instance. 


```r
> # I have a file called 'mpg.csv' in a folder called data
> my_data = read_csv("data/mpg.csv")
Error in read_csv("data/mpg.csv"): could not find function "read_csv"
```

- This fails because I haven't yet loaded the `tidyverse` package

```r
> library(tidyverse)
```



```r
> my_data = read_csv("data/mpg.csv")
Parsed with column specification:
cols(
  `18.0   8   307.0      130.0      3504.      12.0   70  1` = col_character(),
  `chevrolet chevelle malibu` = col_character()
)
```

Packages
===
- packages only need to be loaded once per R session (session starts when you open R studio, ends when you shut it down)
- once the package is loaded it doesn't need to be loaded again before each function call

```r
> my_data_2 = read_csv("data/poly.csv")  # reading another csv file
Parsed with column specification:
cols(
  x = col_double(),
  y = col_double()
)
```


Using R to look at your data
========================================================
type: section

Data analysis workflow
====
1. Read data into R (done!)
2. ~~Manipulate data~~
3. Get results, **make plots and figures**

Getting your data in R
===
- Getting your data into R is easy. We already saw, for example:


```r
> my_data = read_csv("data/mpg.csv")
Parsed with column specification:
cols(
  `18.0   8   307.0      130.0      3504.      12.0   70  1` = col_character(),
  `chevrolet chevelle malibu` = col_character()
)
```

- `read_csv()` requires you to tell it where to find the file you want to read in
  - Windows, e.g.: `"C:\Users\me\Desktop\myfile.csv"`
  - Mac, e.g.: `"/Users/me/Desktop/myfile.csv"`
- If your data is not already in csv format, google "covert X format to csv" or "read X format data in R"
- We'll learn the details of this later, but this is enough to get you started! 
- the `mpg` dataset actually comes pre-loaded with `tidyverse`, so you have it already

Looking at data
===
- `mpg` is now a dataset loaded into R. To look at it, just type

```r
> mpg
# A tibble: 234 x 11
   manufacturer model    displ  year   cyl trans   drv     cty   hwy fl    class
   <chr>        <chr>    <dbl> <int> <int> <chr>   <chr> <int> <int> <chr> <chr>
 1 audi         a4         1.8  1999     4 auto(l… f        18    29 p     comp…
 2 audi         a4         1.8  1999     4 manual… f        21    29 p     comp…
 3 audi         a4         2    2008     4 manual… f        20    31 p     comp…
 4 audi         a4         2    2008     4 auto(a… f        21    30 p     comp…
 5 audi         a4         2.8  1999     6 auto(l… f        16    26 p     comp…
 6 audi         a4         2.8  1999     6 manual… f        18    26 p     comp…
 7 audi         a4         3.1  2008     6 auto(a… f        18    27 p     comp…
 8 audi         a4 quat…   1.8  1999     4 manual… 4        18    26 p     comp…
 9 audi         a4 quat…   1.8  1999     4 auto(l… 4        16    25 p     comp…
10 audi         a4 quat…   2    2008     4 manual… 4        20    28 p     comp…
# … with 224 more rows
```

This is a **data frame**, one of the most powerful features in R (a "tibble" is a kind of data frame).
- Similar to an Excel spreadsheet.
- One ro ~ one
instance of some (real-world) object.
- One column ~ one variable, containing the values for the
corresponding instances.
- All the values in one column should be of the same type (a number, a category, text, etc.), but
different columns can be of different types.

Investigating a relationship
===
Let's say we're curious about the relationship between a car's engine size (the column `displ`) and a car's highway fuel efficiency (column `hww`).
- Can we use R to make a plot of these two variables?


```r
> ggplot(mpg) + 
+   geom_point(aes(x = displ, y = hwy))
```

![plot of chunk unnamed-chunk-13](tabular-data-figure/unnamed-chunk-13-1.png)

- `ggplot(dataset)` says "start a chart with this dataset"
- `+ geom_point(...)` says "put points on this chart"
- `aes(x=x_values y=y_values)` says "map the values in the column `x_values` to the x-axis, and map the values in the column `y_values` to the y-axis" `aes` is short for *aesthetic*

Investigating a relationship
===
type: prompt
Make a scatterplot of `hwy` vs `cyl` (how many cylinders the car has)

```r
> ggplot(mpg) + 
+   geom_point(aes(x = hwy, y = cyl))
```

![plot of chunk unnamed-chunk-14](tabular-data-figure/unnamed-chunk-14-1.png)


Investigating a relationship
===
type: prompt
incremental: true

Make a scatterplot of `hwy` vs `cyl` (how many cylinders the car has)


```r
> ggplot(mpg) + 
+   geom_point(aes(x = hwy, y = cyl))
```

![plot of chunk unnamed-chunk-15](tabular-data-figure/unnamed-chunk-15-1.png)

Investigating a relationship
===
Let's say we're curious about the relationship between a car's engine size (the column `displ`) and a car's highway fuel efficiency (column `hww`).

![plot of chunk unnamed-chunk-16](tabular-data-figure/unnamed-chunk-16-1.png)

- What's going on with these cars? They have higher gas mileage than cars of similar engine size, so maybe they are hybrids?
- If they are hybrids, they would probably be of `class` "compact" or "subcompact"?

Aesthetics
===
- Aesthetics aren't just for mapping columns to the x- and y-axis
- You can also use them to assign color, for instance


```r
> ggplot(mpg) + 
+   geom_point(aes(
+     x = displ, 
+     y = hwy, 
+     color=class
+   )) 
```

- ggplot automatically gives each value of the column a unique level of the aesthetic (here a color) and adds a legend
- What did we learn about the cars we were interested in?

***
<img src="tabular-data-figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" width="90%" />

Aesthetics
===
- Aesthetics aren't just for mapping columns to the x- and y-axis
- Or we could have done a shape


```r
> ggplot(mpg) + 
+   geom_point(aes(
+     x = displ, 
+     y = hwy, 
+     shape=class
+   )) 
```


***

```
Warning: The shape palette can deal with a maximum of 6 discrete values because
more than 6 becomes difficult to discriminate; you have 7. Consider
specifying shapes manually if you must have them.
Warning: Removed 62 rows containing missing values (geom_point).
```

<img src="tabular-data-figure/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" width="90%" />

Aesthetics
===
- Aesthetics aren't just for mapping columns to the x- and y-axis
- Or size


```r
> ggplot(mpg) + 
+   geom_point(aes(
+     x = displ, 
+     y = hwy, 
+     size=class
+   )) 
```
- This one doesn't really make sense because we're mapping a categorical variable to an aesthetic that can take continuous values that imply some ordering

***

```
Warning: Using size for a discrete variable is not advised.
```

<img src="tabular-data-figure/unnamed-chunk-22-1.png" title="plot of chunk unnamed-chunk-22" alt="plot of chunk unnamed-chunk-22" width="90%" />

Aesthetics
===
- If we set a property *outside* of the aesthetic, it no longer maps that property to a column. 


```r
> ggplot(mpg) + 
+   geom_point(
+     aes(
+       x = displ, 
+       y = hwy
+     ),
+     color = "blue"
+   ) 
```
- However, we can use this to assign fixed properties to the plot that don't depend on the data

***
<img src="tabular-data-figure/unnamed-chunk-24-1.png" title="plot of chunk unnamed-chunk-24" alt="plot of chunk unnamed-chunk-24" width="90%" />

Exercise
===
incremental: true
type: prompt

Can you recreate this plot?

![plot of chunk unnamed-chunk-25](tabular-data-figure/unnamed-chunk-25-1.png)
***

```r
> ggplot(mpg) + 
+   geom_point(
+     aes(
+       x = displ, 
+       y = hwy,
+       color = displ,
+       size = hwy
+     )
+   ) 
```

Exercise
===
incremental: true
type: prompt

What will this do? Why?


```r
> ggplot(mpg) + 
+   geom_point(aes(x = displ, y = hwy, color = "blue"))
```
***
![plot of chunk unnamed-chunk-28](tabular-data-figure/unnamed-chunk-28-1.png)

Facets
===
- Aesthetics are useful for mapping columns to particular properties of a single plot
- Use **facets** to generate multiple plots with shared structure


```r
> ggplot(mpg) + 
+   geom_point(aes(x = displ, y = hwy)) + 
+   facet_wrap(~ class, nrow = 2)
```

<img src="tabular-data-figure/unnamed-chunk-29-1.png" title="plot of chunk unnamed-chunk-29" alt="plot of chunk unnamed-chunk-29" style="display: block; margin: auto;" />
- `facet_wrap` is good for faceting according to unordered categories

Facets
===
- `facet_grid` is better for ordered categories, and can be used with two variables


```r
> ggplot(mpg) + 
+   geom_point(aes(x = displ, y = hwy)) + 
+   facet_grid(drv ~ cyl)
```

<img src="tabular-data-figure/unnamed-chunk-30-1.png" title="plot of chunk unnamed-chunk-30" alt="plot of chunk unnamed-chunk-30" style="display: block; margin: auto;" />

Exercise
===
type:prompt
incremental: true

Run this code and comment on what role `.` plays:


```r
> ggplot(mpg) + 
+   geom_point(aes(x = displ, y = hwy)) +
+   facet_grid(drv ~ .)
```

<img src="tabular-data-figure/unnamed-chunk-31-1.png" title="plot of chunk unnamed-chunk-31" alt="plot of chunk unnamed-chunk-31" style="display: block; margin: auto;" />


Geoms
===


```r
> ggplot(mpg) + 
+   geom_point(aes(x = displ, y = hwy))
```

<img src="tabular-data-figure/unnamed-chunk-32-1.png" title="plot of chunk unnamed-chunk-32" alt="plot of chunk unnamed-chunk-32" style="display: block; margin: auto;" />
- Both these plots represent the same data, but they use a different geometric representation ("geom")
- e.g. bar chart vs. line chart, etc. 

***

```r
> ggplot(mpg) + 
+   geom_smooth(aes(x = displ, y = hwy))
```

<img src="tabular-data-figure/unnamed-chunk-33-1.png" title="plot of chunk unnamed-chunk-33" alt="plot of chunk unnamed-chunk-33" style="display: block; margin: auto;" />

Geoms
===
- Different geoms are configured to work with different aesthetics. 
- e.g. you can set the shape of a point, but you ca’t set the “shape” of a line.
- On the other hand, you *can* set the "line type" of a line:


```r
> ggplot(mpg) + 
+   geom_smooth(aes(x = displ, y = hwy, linetype = drv))
```

<img src="tabular-data-figure/unnamed-chunk-34-1.png" title="plot of chunk unnamed-chunk-34" alt="plot of chunk unnamed-chunk-34" style="display: block; margin: auto;" />

Geoms
===
- It's possible to add multiple geoms to the same plot

```r
> ggplot(mpg) + 
+   geom_smooth(aes(x = displ, y = hwy, color = drv)) + 
+   geom_point(aes(x = displ, y = hwy, color = drv))
```

<img src="tabular-data-figure/unnamed-chunk-35-1.png" title="plot of chunk unnamed-chunk-35" alt="plot of chunk unnamed-chunk-35" style="display: block; margin: auto;" />

Geoms
===
- To assign the same aesthetics to all geoms, pass the aesthetics to the `ggplot` function directly instead of to each geom individually

```r
> ggplot(mpg, aes(x = displ, y = hwy, color = drv)) + 
+   geom_smooth() + 
+   geom_point()
```

<img src="tabular-data-figure/unnamed-chunk-36-1.png" title="plot of chunk unnamed-chunk-36" alt="plot of chunk unnamed-chunk-36" style="display: block; margin: auto;" />

Geoms
===
- You can also use different mappings in different geoms

```r
> ggplot(mpg, mapping = aes(x = displ, y = hwy)) + 
+   geom_point(aes(color = class)) + 
+   geom_smooth()
```

<img src="tabular-data-figure/unnamed-chunk-37-1.png" title="plot of chunk unnamed-chunk-37" alt="plot of chunk unnamed-chunk-37" style="display: block; margin: auto;" />

Exercise
===
- Use google or other resources to figure out how to make this plot in R:

```
ggplot(mpg) + 
  ...
```

<img src="tabular-data-figure/unnamed-chunk-38-1.png" title="plot of chunk unnamed-chunk-38" alt="plot of chunk unnamed-chunk-38" style="display: block; margin: auto;" />
<!-- Spaces (mostly) don't matter -->
<!-- ======================================================== -->
<!-- ```{r eval=FALSE,tidy=FALSE} -->
<!-- 1 +2 -->
<!-- 1+ 2 -->
<!-- 1+2 -->
<!-- 1 + 2 -->
<!-- ``` -->
<!-- - These all do the same thing. -->

<!-- Basic calculations and vectors -->
<!-- ======================================================== -->
<!-- type: section -->

<!-- R is a scientific calculator -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- 1 + 2 * 3 -->
<!-- log(10) -->
<!-- 4 * atan(1) -->
<!-- 6^3 -->
<!-- ``` -->

<!-- Vectors -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- c(2.1, -4, 22) -->
<!-- ``` -->
<!-- - A vector is a one-dimensional sequence of zero or more numbers (or other values). -->
<!-- - Vectors are created by wrapping the values separated by commas with the `c(` `)` function, which is short for "combine" -->
<!-- ```{r} -->
<!-- 1:50 -->
<!-- ``` -->
<!-- - The colon `:` is a handy shortcut to create a vector that is -->
<!-- a sequence of integers from the first number to the second number -->
<!-- (inclusive). -->
<!-- - Many R functions and operators automatically work with -->
<!-- multi-element vector arguments. -->
<!-- - Long vectors wrap around. (Your screen may have a different width than what is shown here.) -->
<!-- - Look at the `[ ]` notation. The second output line starts -->
<!-- with 23, which is the 24^th element of the vector. -->
<!-- - This notation will help you figure out where you are in a long vector. -->

<!-- Elementwise operations on a vector -->
<!-- ======================================================== -->
<!--   - This multiplies each element of `1:10` by the corresponding -->
<!-- element of `1:10`, that is, it squares each element. -->
<!-- ```{r} -->
<!-- (1:10)*(1:10) -->
<!-- ``` -->
<!-- - Equivalently, we -->
<!-- could use exponentiation: -->
<!-- ```{r} -->
<!-- (1:10)^2 -->
<!-- ``` -->

<!-- Operator precedence: the rules -->
<!-- ======================================================== -->
<!-- - R does not evaluate strictly left to right. Instead, -->
<!-- operators have their own precedence, a kind of priority for -->
<!-- evaluation. -->
<!-- - The sequence operator : has a higher precedence than addition +. -->
<!-- ```{r} -->
<!-- 1 + 0:10 -->
<!-- 0:10 + 1 # which operator gets executed first? -->
<!-- (0:10) + 1 -->
<!-- 0:(10 + 1) -->
<!-- ``` -->


<!-- Variables -->
<!-- ======================================================== -->
<!-- type: section -->

<!-- Saving values by setting a variable -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- x <- 10 -->
<!-- ``` -->
<!-- - To do more complex computations, we need to be able to give -->
<!-- names to things. -->
<!-- - Read this as "x gets 10" or "assign 10 to x" -->
<!-- - R prints no result from this assignment, but what you entered -->
<!-- causes a side effect: R has stored the association between -->
<!-- x and 10. (Look at the Environment pane.) -->


<!-- Using the value of a variable -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- x -->
<!-- x / 5 -->
<!-- ``` -->
<!-- - When R sees the name of a variable, it uses the stored value of -->
<!-- that variable in the calculation. -->
<!-- - Here R uses the value of x, which is 10. -->
<!-- - `/` is the division operator. -->
<!-- - We can break complex calculations into named parts. This is a -->
<!-- simple, but very useful kind of abstraction. -->


<!-- Two ways to assign -->
<!-- ======================================================== -->
<!-- In R, there are (unfortunately) two assignment operators. They have -->
<!-- subtly different meanings (more details later). -->
<!-- - `<-` requires that you type two characters. Don't put a -->
<!-- space between `<` and `-`. (What would happen?) -->
<!-- - RStudio hint: Use "`Option -`" (Mac) or "`Alt -`" (PC) -->
<!-- to type this using one key combination. -->
<!-- - `=` is easier to type. -->
<!-- - You will see both used throughout R and user code. -->

<!-- ```{r} -->
<!-- x <- 10 -->
<!-- x -->
<!-- x = 20 -->
<!-- x -->
<!-- ``` -->


<!-- Assignment has no undo -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- x <- 10 -->
<!-- x -->
<!-- x <- x + 1 -->
<!-- x -->
<!-- ``` -->
<!-- - If you assign to a name with an existing value, that value is overwritten. -->
<!-- - There is no way to undo an assignment, so be careful in reusing variable names. -->

<!-- Naming variables -->
<!-- ======================================================== -->
<!-- - It is important to pick meaningful variable names. -->
<!-- - Names can be too short, so don't use `x` and `y` everywhere. -->
<!-- - Names can be too long (`Main.database.first.object.header.length`). -->
<!-- - Avoid silly names. -->
<!-- - Pick names that will make sense to someone else (including the -->
<!-- person you will be in six months). -->
<!-- - ADVANCED: See `?make.names` for the complete rules on -->
<!-- what can be a name. -->


<!-- Case matters for names in R -->
<!-- ======================================================== -->
<!-- ```{r, eval=F} -->
<!-- a <- 1 -->
<!-- A # this causes an error because A does not have a value -->
<!-- ``` -->
<!-- ``` -->
<!-- Error: object 'A' not found -->
<!-- ``` -->
<!-- - R cares about upper and lower case in names. -->
<!-- - We also see that some error messages in R are a bit obscure. -->


<!-- More about naming -->
<!-- ======================================================== -->
<!-- There are different conventions for constructing compound names. Warning: -->
<!-- disputes over the right way to do this can get heated. -->
<!-- ```{r, prompt=FALSE,eval=FALSE,tidy=FALSE} -->
<!-- stringlength -->
<!-- string.length -->
<!-- StringLength -->
<!-- stringLength -->
<!-- string_length (underbar) -->
<!-- string-length (hyphen) -->
<!-- ``` -->
<!-- - To be consistent with the packages we will use, I recommend snake_case where you separate lowercase words with _ -->
<!-- - Note that R itself uses several of these conventions. -->
<!-- - One of these won't work. Which one and why? -->

<!-- R saves some names for itself -->
<!-- ======================================================== -->
<!-- ```{r eval=FALSE} -->
<!-- for <- 7 # this causes an error -->
<!-- ``` -->
<!-- - `for` is a reserved word in R. (It is used in loop control.) -->
<!-- - ADVANCED: see `?Reserved` for the complete rules. -->

<!-- Exercise: birth year -->
<!-- === -->
<!-- - Make a variable that represents the age you will be at the end of this year -->
<!-- - Make a variable that represents the current year -->
<!-- - Use them to compute the year of your birth and save that as a variable -->
<!-- - Print the value of that variable -->

<!-- Answer: birth year -->
<!-- === -->
<!-- ```{r} -->
<!-- my_age_end_of_year = 29 -->
<!-- this_year = 2019 -->
<!-- my_birth_year = this_year - my_age_end_of_year -->
<!-- my_birth_year -->
<!-- ``` -->

<!-- Functions -->
<!-- ======================================================== -->
<!-- type: section -->

<!-- Calling built-in functions -->
<!-- ======================================================== -->
<!-- - To call a function, type the function name, then the argument or -->
<!-- arguments in parentheses. (Use a comma to separate the arguments, if -->
<!--                            more than one.) -->
<!-- ```{r} -->
<!-- sqrt(2) -->
<!-- ``` -->
<!-- - Many basic R functions operate on multi-element vectors as -->
<!-- easily as on vectors containing a single number. -->
<!-- ```{r} -->
<!-- sqrt(0:10) -->
<!-- ``` -->

<!-- Functions and variable assignment -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- x <- 4 -->
<!-- sqrt(x) -->
<!-- x -->
<!-- y <- sqrt(x) -->
<!-- y -->
<!-- x <- 10 -->
<!-- y -->
<!-- ``` -->
<!-- - What do you observe? -->

<!-- Functions and variable assignment -->
<!-- ======================================================== -->
<!-- - functions generally do not affect the variables you pass to them (`x` remains the same after `sqrt(x)`) -->
<!-- - The results of a function call will simply be printed out if you do not save the result to a variable -->
<!-- - Saving the result to a variable lets you use it later, like any other variable you define manually -->
<!-- - Once a variable has been assigned (`y`), it keeps its value until updated, even if you change other variables (`x`) that went into the original assignment of that variable -->

<!-- Getting help in the RStudio console -->
<!-- ======================================================== -->
<!-- ```{r eval=FALSE} -->
<!-- sum -->
<!-- ``` -->
<!-- - start typing, then hit `TAB` (or just wait a second) -->
<!-- - RStudio will display help on all functions that start `sum`. -->
<!-- - Use (up arrow, down arrow) to move through -->
<!-- the list. -->
<!-- - Use `RETURN` or `ENTER` to select the current -->
<!-- item. -->

<!-- Using the built-in help -->
<!-- ======================================================== -->
<!-- Type `?name` for help on name. Example: -->
<!-- ```{r eval=FALSE,tidy=FALSE} -->
<!-- ?log -->
<!-- ``` -->
<!-- - This will show information about the `log` function (and related functions), including the name and meaning of the arguments and returned values. -->
<!-- - The help display is hyperlinked, so clicking on a blue link will take you to related material. -->
<!-- - R documentation often describes a set of functions, such as all the ones related to logarithms, on a single help page. -->
<!-- - Parts of the R documentation are rather obscure. -->


<!-- Getting help on operator symbols -->
<!-- ======================================================== -->
<!--   ```{r eval=FALSE,tidy=FALSE,highlight=FALSE} -->
<!-- ?"+" -->
<!-- ``` -->
<!-- - This will show help about the `+` operator. -->
<!-- - Put symbols in quotes to get help. -->


<!-- Exercise: convert weights -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- weights <- c(1.1, 2.2, 3.3) -->
<!-- ``` -->
<!-- - These weights are in pounds. -->
<!-- - Convert them to kilograms. -->
<!-- - (Hint: 2.2 lb = 1.0 kg) -->


<!-- Answer: convert weights -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- weights <- c(1.1, 2.2, 3.3) -->
<!-- ## this divides the weights, element-wise, by the conversion factor: -->
<!-- weights / 2.2 -->
<!-- ``` -->


<!-- Some functions operate on vectors and give back a single number -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- shoesize <- c(9, 12, 6, 10, 10, 16, 8, 4) -->
<!-- shoesize -->
<!-- sum(shoesize) -->
<!-- sum(shoesize)/length(shoesize) -->
<!-- mean(shoesize) -->
<!-- ``` -->


<!-- Exercise: subtract the mean -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- x <- c(7, 3, 1, 9) -->
<!-- ``` -->
<!-- - Subtract the mean of `x` from `x`, and then `sum` -->
<!-- the result. -->


<!-- Answer: subtract the mean -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- x <- c(7, 3, 1, 9) -->
<!-- mean(x) -->
<!-- x - mean(x) -->
<!-- sum(x - mean(x)) # answer in one expression -->
<!-- ``` -->


<!-- Exercise: compute a confidence interval -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- m <- 13 -->
<!-- se <- 0.25 -->
<!-- ``` -->
<!-- - Given the values of `m` (mean), and `se` (standard error), construct a vector containing the two values, $m \pm 2 \times se$. -->
<!-- - That is, add and subtract two times the standard error -->
<!-- value to/from the mean value. -->
<!-- - Your output should look like: -->
<!-- ```{r, echo=FALSE} -->
<!-- m + c(-2, 2)*se -->
<!-- ``` -->


<!-- Answer: compute a confidence interval -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- ## one way: -->
<!-- c(m - 2*se, m + 2*se) -->
<!-- ## another way: -->
<!-- m + c(-2, 2)*se -->
<!-- ``` -->


<!-- Arguments by position -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- 1:5 -->
<!-- seq(1,5) -->
<!-- ``` -->
<!-- - `seq` is the function equivalent of the colon operator. -->
<!-- - Arguments can be specified by position, with one supplied -->
<!-- argument for each name in the function parameter list, and in the -->
<!-- same order. -->


<!-- Arguments by name -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- seq(from = 1, to = 5) -->
<!-- seq(to = 5, from = 1) -->
<!-- ``` -->
<!-- - Sometimes, arguments can be supplied by name using the syntax, -->
<!-- variable `=` value. -->
<!-- - When using names, the order of the named arguments -->
<!-- does not matter. -->
<!-- - You can mix positional and named arguments (carefully). -->
<!-- - Do not use `<-` in place of `=` when specifying -->
<!-- named arguments. -->


<!-- Using the correct argument name -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- seq(1, 5) -->
<!-- seq(from = 1, to = 5) -->
<!-- seq(begin = 1, end = 5) -->
<!-- ``` -->
<!-- - You have to use the correct argument name. -->


<!-- How to find the names of a function's arguments -->
<!-- ======================================================== -->
<!--   - How can you figure out the names of seq's arguments? It is -->
<!-- a built-in function. -->
<!-- - Answer: the arguments are listed in the R documentation of the -->
<!-- function. -->
<!-- ```{r eval=FALSE,tidy=FALSE} -->
<!-- ## Try this: -->
<!-- ?seq -->
<!-- ``` -->


<!-- Calling functions from a package -->
<!-- ============================================================ -->
<!-- - Sometimes packages introduce name conflicts, which is when the pacakge loads a function that is named the same thing as a function that's already in the environment -->
<!-- - Typically, the package being loaded will take precedence over what is already loaded. -->
<!-- - For instance: -->
<!-- ```{r, eval=F} -->
<!-- ?filter # returns documentation for a function called filter in the stats package -->
<!-- library(dplyr) -->
<!-- ?filter # now returns documentation for a function called filter in the dplyr package! -->
<!-- ``` -->
<!-- - You can tell R which function you want by specifying the package name and then `::` before the function name -->
<!-- ```{r, eval=F} -->
<!-- ?stats::filter -->
<!-- ?dplyr::filter -->
<!-- ``` -->
<!-- - This also works when calling the function in your code -->

<!-- RStudio scripts -->
<!-- ============================================================ -->
<!-- type: section -->


<!-- Using the script pane -->
<!-- ============================================================ -->
<!-- - Writing a series of expressions in the console rapidly gets -->
<!-- messy and confusing. -->
<!-- - The console window gets reset when you restart RStudio. -->
<!-- - It is better (and easier) to write expressions and functions -->
<!-- in the script pane (upper left), building up your analysis. -->
<!-- - There, you can enter expressions, evaluate them, and save the -->
<!-- contents to a .R file for later use. -->
<!-- - Look at the RStudio ``Code'' menu for some useful keyboard -->
<!-- commands. -->


<!-- Script pane example -->
<!-- ============================================================ -->
<!-- - Create a script pane: File > New File > R Script -->
<!-- - Put your cursor in the script pane. -->
<!-- - Type: `factorial(1:10)` -->
<!-- - Then hit `Command-RETURN` (Mac), or `Ctrl-ENTER` (Windows). -->
<!-- - That line is copied to the console pane and evaluated. -->
<!-- - You can save the script to a file. -->
<!-- - Explore the RStudio Code menu for other commands. -->

<!-- Adding comments -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- ## This is a comment -->
<!-- 1 + 2 # add some numbers -->
<!-- ``` -->
<!-- - Use a `#` to start a comment. -->
<!-- - A comment extends to the end of the -->
<!-- line and is ignored by R. -->

<!-- Introduction to Data Manipulation -->
<!-- ======================================================== -->
<!-- type: section -->


<!-- Need to install the tidyverse set of packages -->
<!-- ======================================================== -->
<!-- - Do this: -->
<!-- ```{r eval=FALSE} -->
<!-- install.packages("tidyverse") -->
<!-- ``` -->
<!-- ```{r } -->
<!-- library("tidyverse") -->
<!-- ``` -->
<!-- - "tidyverse" is a coherent set of packages for operating a kind of data called the "data frame". -->
<!-- - It is not built-in, so you need to install it (once), then load it each time you restart R. -->
<!-- - Put `library("tidyverse")` at the top of every script file.... -->


<!-- Reading in flat files -->
<!-- ======================================================== -->
<!-- ```{r, eval=F} -->
<!-- mtc = read_csv("https://tinyurl.com/mtcars-csv") -->
<!-- ``` -->
<!-- - `read_csv` (from the `readr` package, part of `tidyverse`) reads in data frames that are stored in `.csv` files (.csv = comma-separated values) -->
<!-- - these files may be online (accessed via URL), or local, in which case the syntax is `read_csv("path/to/file/mtcars.csv")` -->
<!-- - try `?read_csv` to learn a bit more -->
<!-- - `.csv`s can also be exported from databases and saved locally to be read into R, although later we will learn how to communicate directly with a database to pull data into `R`. -->

<!-- Making data frames -->
<!-- ======================================================== -->
<!-- - use `tibble()` to make your own data frames from scratch in R -->
<!-- ```{r} -->
<!-- my_data = tibble( # newlines don't do anything, just increase clarity -->
<!--   mrn = c(1, 2, 3, 4), -->
<!--   age = c(33, 48, 8, 29) -->
<!-- ) -->
<!-- my_data -->
<!-- ``` -->

<!-- Data frame properties -->
<!-- ======================================================== -->
<!-- - `dim()` gives the dimensions of the data frame. `ncol()` and `nrow()` give you the number of columns and the number of rows, respectively. -->
<!-- ```{r} -->
<!-- dim(my_data) -->
<!-- ncol(my_data) -->
<!-- nrow(my_data) -->
<!-- ``` -->

<!-- - `names()` gives you the names of the columns (a vector) -->
<!-- ```{r} -->
<!-- names(my_data) -->
<!-- ``` -->

<!-- Data frame properties -->
<!-- ======================================================== -->
<!-- - `glimpse()` shows you a lot of information, `head()` returns the first `n` rows -->
<!-- ```{r} -->
<!-- glimpse(my_data) -->
<!-- head(my_data, n=2) -->
<!-- ``` -->

<!-- dplyr verbs -->
<!-- ======================================================== -->
<!-- The rest of this section shows the basic data frame functions ("verbs") in the `dplyr` package (part of `tidyverse`). Each operation takes a data frame and produces a new data frame. -->

<!-- - `filter()` picks out rows according to specified conditions -->
<!-- - `select()` picks out columns according to their names -->
<!-- - `arrange()` sorts the row by values in some column(s) -->
<!-- - `mutate()` creates new columns, often based on operations on other columns -->
<!-- - `summarize()` collapses many values in one or more columns down to one value per column -->

<!-- These can all be used in conjunction with `group_by()` which changes the scope of each function from operating on the entire dataset to operating on it group-by-group. These six functions provide the verbs for a language of data manipulation.  -->

<!-- All verbs work similarly: -->

<!-- 1. The first argument is a data frame. -->
<!-- 2. The subsequent arguments describe what to do with the data frame, using the variable names (without quotes). -->
<!-- 3. The result is a new data frame. -->

<!-- Together these properties make it easy to chain together multiple simple steps to achieve a complex result. Let’s dive in and see how these verbs work. -->

<!-- filter() subsets the rows of a data frame -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- filter(mtc, mpg >= 25) -->
<!-- ``` -->
<!-- - This produces (and prints out) a new tibble, which contains all the rows where the mpg value in that row is greater than or equal to 25. -->
<!-- - There are only 6 rows in this data frame. -->
<!-- - There are still 11 columns. -->

<!-- Combining constraints in filter -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- filter(mtc, mpg >= 25, qsec < 19) -->
<!-- ``` -->
<!-- - This filters by the **conjunction** of the two constraints---both must be satisfied. -->
<!-- - Constraints appear as second (and third...) arguments, separated by commas. -->


<!-- Filtering out all rows -->
<!-- ========================================================= -->
<!-- ```{r} -->
<!-- filter(mtc, mpg > 60) -->
<!-- ``` -->
<!-- - If the constraint is too severe, then you will select **no** rows, and produce a zero row sized tibble. -->

<!-- Comparison operators -->
<!-- ========================================================= -->
<!-- - `==` tests for equality (do not use `=`) -->
<!-- - `>` and `<` test for greater-than and less-than -->
<!-- - `>=` and `<=` are greater-than-or-equal and less-than-or-equal -->
<!-- - these can also be used directly on vectors outside of data frames -->
<!-- ```{r} -->
<!-- c(1,5,-22,4) > 0 -->
<!-- ``` -->

<!-- Logical conjunctions -->
<!-- ========================================================= -->
<!-- ```{r} -->
<!-- filter(mtc, mpg > 30 | mpg < 20) -->
<!-- ``` -->
<!-- - `|` stands for OR, `&` is AND -->
<!-- - as we have seen, separating conditions by a comma is the same as using `&` inside `filter()` -->
<!-- - these can be made into complex logical conditions  -->

<!-- Logical conjunctions -->
<!-- ========================================================= -->
<!-- ```{r} -->
<!-- filter(mtc, !(mpg > 30 | mpg < 20)) -->
<!-- ``` -->
<!-- - `!` is NOT, which negates the logical condition -->

<!-- Logical conjunctions -->
<!-- ========================================================= -->
<!-- ```{r} -->
<!-- filter(mtc, cyl %in% c(6,8)) # equivalent to filter(mtc, cyl==6 | cyl==8) -->
<!-- ``` -->
<!-- - `%in%` returns true for all elements of the thing on the left that are also elements of the thing on the right -->

<!-- Exercise: cars with powerful engines -->
<!-- ========================================================== -->
<!-- - How many cars have engines with horsepower (`hp`) greater than 200? -->


<!-- Answer: cars with powerful engines -->
<!-- =========================================================== -->
<!-- ```{r} -->
<!-- filter(mtc, hp > 200) -->
<!-- ``` -->
<!-- - Answer: 7 -->


<!-- Exercise: filtering rows -->
<!-- ======================================================== -->
<!-- - List all cars with `mpg` between 15 and 20. -->


<!-- Answer: filtering rows -->
<!-- ========================================================= -->
<!-- ```{r} -->
<!-- filter(mtc, mpg > 15, mpg < 20) -->
<!-- ``` -->


<!-- Filtering by row number -->
<!-- ========================================================== -->
<!-- ```{r} -->
<!-- filter(mtc, row_number()<=3) -->
<!-- ``` -->
<!-- - use `row_number()` to get specific rows. This is more useful once you have sorted the data in a particular order, which we will soon see how to do. -->

<!-- Sampling rows -->
<!-- ========================================================== -->
<!-- ```{r} -->
<!-- sample_n(mtc, 5) -->
<!-- ``` -->
<!-- - You can use `sample_n()` to get `n` randomly selected rows if you don't have a particular condition you would like to filter on. -->
<!-- - `sample_frac()` is similar -->
<!-- - do `?sample_n()` to see how you can sample with replacement or with weights -->

<!-- select() subsets columns by name -->
<!-- ========================================================= -->
<!-- ```{r} -->
<!-- select(mtc, mpg, qsec, wt) -->
<!-- ``` -->
<!-- - The select function will return a subset of the tibble, using only the requested columns in the order specified. -->

<!-- select() subsets columns by name -->
<!-- ========================================================= -->
<!-- - `select()` can also be used with handy helpers like `starts_with()` and `contains()` -->
<!-- ```{r} -->
<!-- select(mtc, starts_with("m")) -->
<!-- ``` -->

<!-- select() subsets columns by name -->
<!-- ========================================================= -->
<!-- - `select()` can also be used with handy helpers like `starts_with()` and `contains()` -->
<!-- ```{r} -->
<!-- select(mtc, hp, contains("m")) -->
<!-- ``` -->
<!-- - The quotes around the letter `"m"` make it a character string (or string for short). If we did not do this, `R` would think it was looking for a variable called `m` and not just the plain letter.  -->
<!-- - We don't have to quote the names of columns (like `hp`) because the `tidyverse` functions know that we are working within the dataframe and thus treat the column names like they are variables in their own right -->

<!-- select() subsets columns by name -->
<!-- ========================================================= -->
<!-- - `select()` can also be used to select everything **except for** certain columns -->
<!-- ```{r} -->
<!-- select(mtc, -contains("m"), -hp) -->
<!-- ``` -->

<!-- pull() is a friend of select() -->
<!-- ========================================================= -->
<!-- - `select()` has a friend called `pull()` which returns a vector instead of a (one-column) data frame -->
<!-- ```{r} -->
<!-- select(mtc, hp) -->
<!-- pull(mtc, hp) -->
<!-- ``` -->

<!-- Saving the result -->
<!-- ========================================================= -->
<!-- ```{r} -->
<!-- filter(mtc, row_number()==1) -->
<!-- head(mtc) -->
<!-- ``` -->
<!-- - `select()` and `filter()` are functions, so they do not modify their input. You can see `mtc` is unchanged after calling `filter()` on it. This holds for functions in general. -->

<!-- Saving the result -->
<!-- ========================================================= -->
<!-- - To save a new version of mtc, use a variable -->
<!-- ```{r} -->
<!-- mtc_first_row = filter(mtc, row_number()==1) -->
<!-- mtc_first_row -->
<!-- ``` -->

<!-- Combining filtering and selecting -->
<!-- ========================================================= -->
<!-- - If the result of the operation will only be used by one other function, you can nest the calls: -->
<!-- ```{r} -->
<!-- # tmp = select(mtc, mpg, qsec, wt) -->
<!-- # filter(tmp, mpg >= 25) -->
<!-- filter(select(mtc, mpg, qsec, wt), mpg >= 25) -->
<!-- ``` -->


<!-- arrange() sorts rows -->
<!-- =========================================================== -->
<!-- - `arrange` takes a data frame and a column, and sorts the rows by the values in that column (ascending order). -->
<!-- ```{r} -->
<!-- powerful <- filter(mtc, hp > 200) -->
<!-- arrange(powerful, mpg) -->
<!-- ``` -->

<!-- Arrange can sort by more than one column -->
<!-- =========================================================== -->
<!-- - This is useful if there is a tie in sorting by the first column. -->
<!-- ```{r} -->
<!-- arrange(powerful, gear, disp) -->
<!-- ``` -->


<!-- Use the desc function to sort by descending values -->
<!-- =========================================================== -->
<!-- ```{r} -->
<!-- arrange(powerful, desc(mpg)) -->
<!-- ``` -->

<!-- Exercise: top 5 mpg cars -->
<!-- =========================================================== -->
<!-- Use `arrange()` and `filter()` to get the data for the 5 cars with the highest mpg. -->

<!-- Answer: top 5 mpg cars -->
<!-- ================================================================ -->
<!-- ```{r} -->
<!-- filter(arrange(mtc, desc(mpg)), row_number()<=5) # "nesting" the calls to filter and arrange -->
<!-- ``` -->
<!-- or -->
<!-- ```{r} -->
<!-- cars_by_mpg = arrange(mtc, desc(mpg)) # using a temporary variable -->
<!-- filter(cars_by_mpg, row_number()<=5) -->
<!-- ``` -->

<!-- mutate() creates new columns -->
<!-- ================================================================ -->
<!-- ```{r} -->
<!-- mtc_vars_subset = select(mtc, mpg, hp) -->
<!-- mutate(mtc_vars_subset, gpm = 1/mpg) -->
<!-- ``` -->
<!-- - This uses `mutate` to add a new column to which is the reciprocal of `mpg`. -->
<!-- - The thing on the left of the `=` is a new name that you make up which you would like the new column to be called -->
<!-- - The expresssion on the right of the `=` defines what will go into the new column -->
<!-- -mutate() can create multiple columns at the same time and use multiple columns to define a single new one -->

<!-- mutate() can create multiple new columns at once -->
<!-- ================================================================ -->
<!-- ```{r} -->
<!-- mutate(mtc_vars_subset, # the newlines make it more readable -->
<!--       gpm = 1/mpg, -->
<!--       mpg_hp_ratio = mpg/hp)  -->
<!-- ``` -->
<!-- - As before, the result of this function is only saved if you assign it to a variable. In this example, `mtc_vars_subset` is unchanged after the mutate. -->

<!-- mutate() for data type conversion -->
<!-- === -->
<!-- - Data is sometimes given to you in a form that makes it difficult to do operations on -->
<!-- ```{r} -->
<!-- df = tibble(number = c("1", "2", "3")) -->
<!-- mutate(df, number_plus_1 = number + 1) -->
<!-- ``` -->

<!-- - `mutate()` is also useful for converting data types, in this case text to numbers -->
<!-- ```{r} -->
<!-- mutate(df, number = as.numeric(number))  -->
<!-- ``` -->
<!-- - if you save the result into a column that already exists, it will be overwritten -->

<!-- summarize() computes desired summaires across rows -->
<!-- ================================================================ -->
<!-- ```{r} -->
<!-- summarize(mtc, mpg_avg=mean(mpg)) -->
<!-- ``` -->
<!-- - `summarize()` boils down the data frame according to the conditions it gets. In this case, it creates a data frame with a single column called `mpg_avg` that contains the mean of the `mpg` column -->
<!-- - Summaries can be very useful when you apply them to subgoups of the data, which we will soon see how to do. -->

<!-- summarize() computes desired summaires across rows -->
<!-- ================================================================ -->
<!-- - you can also pass in multiple conditions that operate on multiple columns at the same time -->
<!-- ```{r} -->
<!-- summarize(mtc, # newlines not necessary, again just increase clarity -->
<!--           mpg_avg = mean(mpg),  -->
<!--           mpg_2x_max = max(2*mpg),  -->
<!--           hp_mpg_ratio_min = min(hp/mpg)) -->
<!-- ``` -->

<!-- dplyr verbs summary -->
<!-- ======================================================== -->

<!-- - `filter()` picks out rows according to specified conditions -->
<!-- - `select()` picks out columns according to their names -->
<!-- - `arrange()` sorts the row by values in some column(s) -->
<!-- - `mutate()` creates new columns, often based on operations on other columns -->
<!-- - `summarize()` collapses many values in one or more columns down to one value per column -->

<!-- All verbs work similarly: -->

<!-- 1. The first argument is a data frame. -->
<!-- 2. The subsequent arguments describe what to do with the data frame, using the variable names (without quotes). -->
<!-- 3. The result is a new data frame. -->

<!-- Together these properties make it easy to chain together multiple simple steps to achieve a complex result. -->

<!-- Computing over groups -->
<!-- ================================================================== -->
<!-- type: section -->


<!-- group_by() groups data according to some variable(s) -->
<!-- ================================================================== -->
<!-- First, let's load in some new data. -->
<!-- ```{r} -->
<!-- data1 <- read_csv("http://stanford.edu/~sbagley2/bios205/data/data1.csv") -->
<!-- data1 -->
<!-- ``` -->
<!-- - `<chr>` is short for "character string", which means text data -->
<!-- - Let's compute the mean weight for each gender. -->
<!-- - There are two values of gender in this data, so there will be two groups. -->
<!-- - The following builds a new version of the data frame that saves the grouping information: -->
<!-- ```{r} -->
<!-- data1_by_gender <- group_by(data1, gender) -->
<!-- ``` -->
<!-- - We can now use the grouped data frame in further calculations. -->

<!-- group_by() groups data according to some variable(s) -->
<!-- ================================================================== -->
<!-- ```{r} -->
<!-- data1_by_gender -->
<!-- ``` -->
<!-- - The grouped data looks exactly the same, but under the hood, `R` knows that this is really two sub-data-frames (one for each group) instead of one. -->

<!-- Grouped summary: computing the mean of each group -->
<!-- =================================================================== -->
<!-- ```{r} -->
<!-- summarize(data1_by_gender, mean_weight = mean(weight)) -->
<!-- ``` -->
<!-- - `summarize()` works the same as before, except now it returns two rows instead of one because there are two groups that were defined by `group_by(gender)`. -->
<!-- - The result also always contains colunmns corresponding to the unique values of the grouping variable -->

<!-- Grouping can also be applied across multiple variables -->
<!-- =================================================================== -->
<!-- - This computes the mean weight and the mean age for each group: -->
<!-- ```{r} -->
<!-- data1_by_gender_and_shoesize = group_by(data1, gender, shoesize) -->
<!-- summarize(data1_by_gender_and_shoesize,  -->
<!--           mean_weight = mean(weight),  -->
<!--           mean_age = mean(age)) -->
<!-- ``` -->
<!-- - Now both `gender` and `shoesize` appear as columns in the result -->
<!-- - There are 3 rows because there are 3 unique combinations of `gender` and `shoesize` in the original data -->

<!-- Computing the number of rows in each group -->
<!-- ===================================================================== -->
<!-- - The `n()` function counts the number of rows in each group: -->
<!-- ```{r} -->
<!-- summarize(data1_by_gender, count = n()) -->
<!-- ``` -->


<!-- Computing the number of distinct values of a column in each group -->
<!-- ===================================================================== -->
<!-- - The `n_distinct()` function counts the number of distinct (unique) values in the specified column: -->
<!-- ```{r} -->
<!-- summarize(data1_by_gender, n_sizes = n_distinct(shoesize)) -->
<!-- ``` -->
<!-- - Note: `distinct()` filters out any duplicate rows in a dataframe. The equivalent for vectors is `unique()` -->


<!-- Exercise: count states in each region -->
<!-- ===================================================================== -->
<!-- ```{r} -->
<!-- state_data <- read_csv("http://stanford.edu/~sbagley2/bios205/data/state_data.csv") -->
<!-- state_data -->
<!-- ``` -->
<!-- - How many states are in each region? -->


<!-- Answer: count states in each region -->
<!-- ===================================================================== -->
<!-- ```{r} -->
<!-- state_data_by_region <- group_by(state_data, region) -->
<!-- summarize(state_data_by_region, n_states = n()) -->
<!-- ``` -->


<!-- Challenge exercise: finding rows by group -->
<!-- =================================================================== -->
<!-- `filter()` the grouped data in `data1_by_gender` to pick out the rows for the youngest male and female (hint: use `min()` and `==`). -->


<!-- Answer: finding rows by group -->
<!-- =================================================================== -->
<!-- ```{r} -->
<!-- filter(data1_by_gender, age==min(age)) -->
<!-- ``` -->
<!-- - This shows how filter can be applied to grouped data. Instead of applying the condition across all the data, it applies it group-by-group. -->

<!-- Chaining: combining a sequence of function calls -->
<!-- ================================================================= -->
<!-- type: section -->

<!-- Both nesting and temporary variables can be ugly and hard to read -->
<!-- ================================================================= -->
<!-- - In this expression, the result of `summarize` is used as an argument to `arrange`. -->
<!-- - The operations are performed "inside out": first `summarize`, then `arrange`. -->
<!-- ```{r eval=FALSE} -->
<!-- arrange(summarize(group_by(state_data, region), sd_area = sd(area)), sd_area) -->
<!-- ``` -->
<!-- - We could store the first result in a temporary variable: -->
<!-- ```{r} -->
<!-- state_data_by_region <- group_by(state_data, region) -->
<!-- region_area_sds <- summarize(state_data_by_region, sd_area = sd(area)) -->
<!-- arrange(region_area_sds, sd_area) -->
<!-- ``` -->


<!-- Chaining using the pipe operator -->
<!-- ================================================================= -->
<!-- - Or, we can use a new operator, `%>%`, to "pipe" the result from the first -->
<!-- function call to the second function call. -->
<!-- ```{r} -->
<!-- state_data %>% -->
<!--   group_by(region) %>% -->
<!--   summarize(sd_area = sd(area)) %>% -->
<!--   arrange(sd_area) -->
<!-- ``` -->

<!-- - This makes explicit the flow of data through operations: -->
<!--   - Start with `state_data` -->
<!--   - group it by region -->
<!--   - summarize by region, computing `sd_area` -->
<!--   - arrange rows by `sd_area` -->
<!-- - The code reads like instructions that a human could understand -->
<!-- - putting the function calls on different lines also improves readability -->

<!-- Pipe: details -->
<!-- ================================================================= -->
<!-- ```{r eval=FALSE} -->
<!-- df1 %>% fun(x) -->
<!-- ``` -->
<!-- is converted into: -->
<!-- ```{r eval=FALSE} -->
<!-- fun(df1, x) -->
<!-- ``` -->
<!-- - That is: the thing being piped in is used as the _first_ argument of `fun`. -->
<!-- - The tidyverse functions are consistently designed so that the first argument is a data frame, and the result is a data frame, so piping always works as intended. -->

<!-- Pipe: details -->
<!-- ================================================================= -->
<!-- - However, the pipe works for all variables and functions, not just tidyverse functions -->
<!-- ```{r} -->
<!-- c(1,44,21,0,-4) %>% -->
<!--     sum() -->
<!-- sum(c(1,44,21,0,-4)) -->
<!-- 1 %>% `+`(1) # `+` is just a function that takes two arguments! -->
<!-- ``` -->

<!-- Piping to another position -->
<!-- ===  -->
<!-- - The pipe typically pipes into the first argument of a function, but you can use the `.` syntax to send the argument elsewhere: -->
<!-- ```{r} -->
<!-- values = c(1,2,3,NA) -->

<!-- TRUE %>% -->
<!--   mean(values, na.rm=.) -->
<!-- ``` -->
<!-- - This is typically not done, but can be a handy shortcut in many situations -->

<!-- dplyr cheatsheet -->
<!-- ============================================================ -->
<!-- <div align="center"> -->
<!-- <img src="https://www.rstudio.com/wp-content/uploads/2018/08/data-transformation.png", height=1000, width=1400> -->
<!-- </div> -->

<!-- Visualization basics -->
<!-- ============================================================ -->
<!-- type: section -->

<!-- ggplot2 -->
<!-- ============================================================= -->
<!-- - `ggplot2` is a very powerful graphics package. -->
<!-- - gg stands for "grammar of graphics" -->
<!-- - It was installed and loaded as part of `tidyverse`. -->
<!-- - Otherwise, you must do the following: -->
<!-- ```{r eval=FALSE} -->
<!-- install.packages("ggplot2") -->
<!-- library("ggplot2") -->
<!-- ``` -->
<!-- ```{r include=FALSE} -->
<!-- ## better font size for slides -->
<!-- theme_set(theme_grey(base_size = 22)) -->
<!-- ``` -->


<!-- A simple scatterplot -->
<!-- ============================================================= -->
<!-- ```{r} -->
<!-- ggplot(data = mtc, mapping = aes(x = hp, y = mpg)) +  -->
<!--   geom_point() -->
<!-- ``` -->
<!-- - Note that, although the package is named `ggplot2`, the function is called simply `ggplot()` -->

<!-- How to call ggplot -->
<!-- ============================================================== -->
<!-- ```{r, prompt=FALSE,eval=FALSE,tidy=FALSE} -->
<!-- ggplot(data = mtc, mapping = aes(x = hp, y = mpg)) + geom_point() -->
<!-- ``` -->
<!-- - `data = mtc`: this tells which tibble contains the data to be plotted -->
<!-- - `mapping = aes(x = hp, y = mpg)`: use the data in the hp column on x-axis, mpg column on y-axis -->
<!-- - `geom_point()`: plot the data as points -->
<!-- - Note that you can use positional instead of named arguments to make this expression shorter: -->
<!-- ```{r prompt=FALSE,eval=FALSE} -->
<!-- ggplot(mtc, aes(hp, mpg)) +  -->
<!--   geom_point() -->
<!-- ``` -->
<!-- - The use of "+" to glue these operations together will be explained later. -->


<!-- Change points to lines -->
<!-- =============================================================== -->
<!-- ```{r} -->
<!-- ggplot(mtc, aes(hp, mpg)) +  -->
<!--   geom_line() -->
<!-- ``` -->
<!-- - This is pretty ugly. Line plots are better for time series. -->


<!-- Fit straight line to points -->
<!-- =============================================================== -->
<!-- ```{r} -->
<!-- ggplot(mtc, aes(hp, mpg)) +  -->
<!--   geom_point() +  -->
<!--   geom_smooth(method="lm") -->
<!-- ``` -->
<!-- - `"lm"` means "linear model," which is a least-squares regression line. -->
<!-- - The gray band is the confidence interval. -->


<!-- Fit smooth line to points -->
<!-- ================================================================ -->
<!-- ```{r} -->
<!-- ggplot(mtc, aes(hp, mpg)) +  -->
<!--   geom_point() +  -->
<!--   geom_smooth(method="loess") -->
<!-- ``` -->
<!-- - "loess" fits a collection of tiny regression lines, then glues them together. -->
<!-- - This is a better approximation than a straight line for these data. -->


<!-- Fit smooth line to points without standard error -->
<!-- ================================================================ -->
<!-- ```{r} -->
<!-- mtc %>% # with the pipe -->
<!-- ggplot(aes(hp, mpg)) +  -->
<!--   geom_point() +  -->
<!--   geom_smooth(method="loess", se=FALSE) -->
<!-- ``` -->
<!-- - `se = FALSE` means do not plot the confidence band (using the standard error) -->

<!-- Plotting categorical variables -->
<!-- ==================================================================== -->
<!-- - `gender` is a discrete variable, with two values. -->
<!-- ```{r} -->
<!-- data1 %>% -->
<!--   group_by(gender) %>% -->
<!--   summarize(mean_age=mean(age), mean_weight=mean(weight)) %>% -->
<!-- ggplot(aes(gender, mean_weight)) +  -->
<!--   geom_col() -->
<!-- ``` -->
<!-- - `geom_col()` is used to make a bar plot. Height of bar is the value for that group. -->

<!-- The grammar of graphics -->
<!-- ============================================================ -->
<!-- - Most graphics systems are a large collection of functions to call to -->
<!-- construct a graph piece by piece. -->
<!-- - `ggplot2` is different, and is based on the idea of a "grammar of -->
<!-- graphics," a set of primitives and rules for combining them in a way -->
<!-- that makes sense for plotting data. -->
<!-- - This perspective is quite powerful, but requires learning a bit of -->
<!-- vocabulary and a new way of thinking about graphics. -->


<!-- The ggplot2 model (simplified version) -->
<!-- ============================================================ -->
<!-- 1. supply data frame (rows of observations, columns of variables) -->
<!-- 2. use `aes` to map from variables (columns in data frame) to -->
<!-- aethetics (visual properties of the plot): x, y, color, size, -->
<!-- shape, and others. -->
<!-- 3. choose a `geom`. This determines the type of the plot: point (a -->
<!-- scatterplot), line (line graph or line chart), bar (barplot), and -->
<!-- others. -->
<!-- 4. choose a `stat` (statistical transformation): often `identity` (do -->
<!-- no transformation), but can be used to count, bin, or summarize -->
<!-- data (e.g., in a histogram). -->
<!-- 5. choose a `scale`. This converts from the units used in the data -->
<!-- frame to the units used for display. -->
<!-- 6. provide optional facet specification. -->

<!-- ggplot2 cheatsheet -->
<!-- ============================================================ -->
<!-- <div align="center"> -->
<!-- <img src="https://www.rstudio.com/wp-content/uploads/2018/08/data-visualization-2.1.png", height=1000, width=1400> -->
<!-- </div> -->

<!-- Putting it together -->
<!-- ================================================================= -->
<!-- type: section -->

<!-- Exercise: Is there a linear relationship between hp and 1/mpg? -->
<!-- ================================================================= -->
<!-- - Use `ggplot` to look for a linear relationship between `hp` and `1/mpg` in our `mtc` data -->


<!-- Answer: Is there a linear relationship between hp and 1/mpg? -->
<!-- ================================================================= -->
<!-- ```{r} -->
<!-- ggplot(mtc, aes(hp, 1/mpg)) +  -->
<!--   geom_point() +  -->
<!--   geom_smooth(method="lm", se=FALSE) -->
<!-- ``` -->
<!-- - So, probably "yes" -->

<!-- Answer: Is there a linear relationship between hp and 1/mpg? -->
<!-- ================================================================= -->
<!-- - Could also have done: -->
<!-- ```{r, eval=F} -->
<!-- mtc %>% -->
<!--   mutate(gpm = 1/mpg) %>% -->
<!-- ggplot(aes(hp, gpm)) +  -->
<!--   geom_point() +  -->
<!--   geom_smooth(method="lm", se=FALSE) -->
<!-- ``` -->

<!-- Exercise: orange trees -->
<!-- ================================================================= -->
<!-- ```{r} -->
<!-- orange <- as_tibble(Orange) # this data is pre-loaded into R -->
<!-- ``` -->
<!-- 1. Pull out the data for tree 2 only -->
<!-- 2. Plot circumference versus age for those data -->

<!-- Answer: orange trees -->
<!-- ================================================================== -->
<!-- ```{r} -->
<!-- orange %>% -->
<!--   filter(Tree == 2) %>% -->
<!-- ggplot(aes(age, circumference)) +  -->
<!--   geom_point() -->
<!-- ``` -->


<!-- Exercise: more orange trees -->
<!-- ============================================================ -->
<!-- 1. Pull out the data for tree 2 where `age > 1000` -->
<!-- 2. Plot circumference versus age for those data -->

<!-- Answer: more orange trees -->
<!-- ================================================================== -->
<!-- ```{r} -->
<!-- orange %>% -->
<!--   filter(Tree == 2, age > 1000) %>% -->
<!-- ggplot(aes(age, circumference)) +  -->
<!--   geom_point() -->
<!-- ``` -->


<!-- Exercise: even more orange trees -->
<!-- ============================================================ -->
<!-- - Add a new column called `circum_in` which is the circumference in inches, not in millimeters. -->


<!-- Answer: even more orange trees -->
<!-- ================================================================= -->
<!-- ```{r} -->
<!-- mutate(orange, circum_in = circumference/(10 * 2.54)) -->
<!-- ``` -->


<!-- Exercise: compute mean area per region -->
<!-- ===================================================================== -->
<!-- Use the `state_data` data frame for this exercise. -->
<!-- - What is the mean area for each region? -->
<!-- - Sort the result by decreasing area. -->


<!-- Answer: compute mean area per region -->
<!-- ===================================================================== -->
<!-- ```{r} -->
<!-- state_data %>% -->
<!--   group_by(region) %>% -->
<!--   summarize(mean_area = mean(area)) %>% -->
<!--   arrange(desc(mean_area)) -->
<!-- ``` -->


<!-- Exercise: Sort the regions by area range -->
<!-- ===================================================================== -->
<!-- - Sort the regions by the difference between the areas of the largest and smallest state in each region. -->


<!-- Answer: Sort the regions by area range -->
<!-- ===================================================================== -->
<!-- ```{r} -->
<!-- state_data %>% -->
<!--   group_by(region) %>% -->
<!--   summarize(area_range = max(area) - min(area)) %>% -->
<!--   arrange(area_range) -->
<!-- ``` -->


<!-- Adding new column with data by group -->
<!-- ===================================================================== -->
<!-- ```{r} -->
<!-- state_data2 <- state_data %>% -->
<!--   group_by(region) %>% -->
<!--   mutate(region_mean = mean(area)) -->
<!-- state_data2 -->
<!-- ``` -->
<!-- - This computes the mean area for each region, and places those values in a new column. -->
<!-- - The `region_mean` column has 50 values, one for each state, depending on the region the state is in. -->


<!-- Exercise: closest to region mean -->
<!-- ===================================================================== -->
<!-- - Which state is closest to the mean of its region? -->


<!-- Answer: closest to region mean -->
<!-- ===================================================================== -->
<!-- ```{r} -->
<!-- state_data2 %>% -->
<!--     mutate(diff = abs(area-region_mean)) %>% -->
<!--     filter(diff == min(diff)) -->
<!-- ``` -->

<!-- - We should use `ungroup()` to undo the `group_by()` so that the `filter()` is applied across the whole data frame and not region-by-region -->

<!-- ```{r} -->
<!-- state_data2 %>% -->
<!--     mutate(diff = abs(area-region_mean)) %>% -->
<!--     ungroup() %>%  -->
<!--     filter(diff == min(diff)) -->
<!-- ``` -->
<!-- - Answer: Florida -->



<!-- Exercise: smallest state in each region -->
<!-- ===================================================================== -->
<!-- - What is the smallest state in each region? -->


<!-- Answer: smallest state in each region -->
<!-- ===================================================================== -->
<!-- ```{r} -->
<!-- state_data %>% -->
<!--     group_by(region) %>% -->
<!--     filter(area == min(area)) -->
<!-- ``` -->


<!-- Relational data and joins -->
<!-- ============================================================ -->
<!-- type: section -->
<!-- ```{r} -->
<!-- # install.packages("nycflights13") -->
<!-- library(nycflights13) -->
<!-- ``` -->


<!-- Relational data -->
<!-- ===================================================================== -->
<!-- class: small-code -->

<!-- ```{r} -->
<!-- head(flights) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- head(airports) -->
<!-- ``` -->

<!-- *** -->

<!-- ```{r} -->
<!-- head(planes) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- head(weather) -->
<!-- ``` -->

<!-- Relational data -->
<!-- === -->

<!-- <div align="center"> -->
<!-- <img src="https://d33wubrfki0l68.cloudfront.net/245292d1ea724f6c3fd8a92063dcd7bfb9758d02/5751b/diagrams/relational-nycflights.png"> -->
<!-- </div> -->

<!-- - `flights` connects to `planes` via a single variable, `tailnum`. -->
<!-- - `flights` connects to `airlines` through the `carrier` variable. -->
<!-- - `flights` connects to `airports` in two ways: via the `origin` and `dest` variables. -->
<!-- - `flights` connects to `weather` via `origin` (the location), and `year`, `month`, `day` and `hour` (the time). -->

<!-- An example join -->
<!-- === -->
<!-- - Imagine we want to add the full airline name to some columns from `flights` -->
<!-- ```{r} -->
<!-- flights %>% -->
<!--   select(tailnum, origin, dest, carrier) %>% -->
<!--   inner_join(airlines, by="carrier") -->
<!-- ``` -->

<!-- Joins -->
<!-- === -->
<!-- ```{r} -->
<!-- x <- tibble( -->
<!--   key = c(1,2,3), -->
<!--   val_x = c("x1","x2","x3") -->
<!-- ) -->
<!-- y <- tibble( -->
<!--   key = c(1,2,4), -->
<!--   val_y = c("y1","y2","y3") -->
<!-- ) -->
<!-- ``` -->

<!-- <div align="center"> -->
<!-- <img src="https://d33wubrfki0l68.cloudfront.net/108c0749d084c03103f8e1e8276c20e06357b124/5f113/diagrams/join-setup.png"> -->
<!-- </div> -->

<!-- *** -->

<!-- ```{r} -->
<!-- inner_join(x, y, by="key") -->
<!-- ``` -->
<!-- - An inner join matches pairs of observations when their keys are equal -->
<!-- - the column that is joined on is specified with the named argument `by="column"` -->

<!-- <div align="center"> -->
<!-- <img src="https://d33wubrfki0l68.cloudfront.net/3abea0b730526c3f053a3838953c35a0ccbe8980/7f29b/diagrams/join-inner.png"> -->
<!-- </div> -->

<!-- Duplicate keys -->
<!-- === -->
<!-- ```{r} -->
<!-- x <- tibble( -->
<!--   key = c(1,2,2,3), -->
<!--   val_x = c("x1","x2","x3","x4") -->
<!-- ) -->
<!-- y <- tibble( -->
<!--   key = c(1,2,2,4), -->
<!--   val_y = c("y1","y2","y3","y4") -->
<!-- ) -->
<!-- ``` -->

<!-- <div align="center"> -->
<!-- <img src="https://d33wubrfki0l68.cloudfront.net/d37530bbf7749f48c02684013ae72b2996b07e25/37510/diagrams/join-many-to-many.png"> -->
<!-- </div> -->

<!-- *** -->

<!-- ```{r} -->
<!-- inner_join(x, y, by="key") -->
<!-- ``` -->

<!-- When keys are duplicated, multiple rows can match multiple rows, so each possible combination is produced -->

<!-- Specifying the keys -->
<!-- === -->
<!-- ```{r} -->
<!-- inner_join(airports, flights, by="origin") -->
<!-- ``` -->
<!-- - Why does this fail? -->

<!-- Specifying the keys -->
<!-- === -->
<!-- - When keys have different names in different dataframes, the syntax to join is: -->
<!-- ```{r} -->
<!-- inner_join(airports, flights, by=c("faa"="origin")) -->
<!-- ``` -->

<!-- Exercise: finding planes -->
<!-- === -->
<!-- Use joins to find the models of airplane that fly into Seattle Tacoma Intl. -->

<!-- Answer: finding planes -->
<!-- === -->
<!-- Use joins to find the models of airplane that fly into Seattle Tacoma Intl. -->
<!-- ```{r} -->
<!-- airports %>% -->
<!--   filter(name=="Seattle Tacoma Intl") %>% -->
<!--   inner_join(flights, by=c("faa"="dest")) %>% -->
<!--   inner_join(planes, by="tailnum") %>% -->
<!--   select(model) %>% -->
<!--   distinct() -->
<!-- ``` -->

<!-- Other joins -->
<!-- === -->
<!-- - A left join keeps all observations in `x`. -->
<!-- - A right join keeps all observations in `y`. -->
<!-- - A full join keeps all observations in `x` and `y`. -->

<!-- <div align="center"> -->
<!-- <img src="https://d33wubrfki0l68.cloudfront.net/9c12ca9e12ed26a7c5d2aa08e36d2ac4fb593f1e/79980/diagrams/join-outer.png"> -->
<!-- </div> -->

<!-- - Left join should be your default -->
<!--   - it looks up additional information in other tables -->
<!--   - preserves all rows in the table you're most interested in -->

<!-- *** -->

<!-- <div align="center"> -->
<!-- <img src="https://d33wubrfki0l68.cloudfront.net/aeab386461820b029b7e7606ccff1286f623bae1/ef0d4/diagrams/join-venn.png"> -->
<!-- </div> -->

<!-- Joining on multiple columns -->
<!-- === -->
<!-- - It is often desirable to find matches along more than one column -->
<!-- ```{r} -->
<!-- flights %>% -->
<!--   select(tailnum, year:day, hour, origin) %>% -->
<!--   left_join(weather, by=c("year", "month", "day", "hour", "origin")) %>%  -->
<!--   head(3) -->
<!-- ``` -->
<!-- - This is also possible if the columns have different names -->
<!-- ```{r, eval=F} -->
<!-- flights %>% -->
<!--   select(tailnum, year:day, hour, origin) %>% -->
<!--   rename(departure = origin) %>% -->
<!--   left_join(weather, by=c("year", "month", "day", "hour", "departure"="origin")) -->
<!-- ``` -->

<!-- Join problems -->
<!-- === -->
<!-- - Joins can be a source of subtle errors in your code -->
<!-- - check for `NA`s in variables you are going to join on -->
<!-- - make sure rows aren't being dropped if you don't intend to drop rows -->
<!--   - checking the number of rows before and after the join is not sufficient. If you have an inner join with duplicate keys in both tables, you might get unlucky as the number of dropped rows might exactly equal the number of duplicated rows -->
<!-- - `anti_join()` and `semi_join()` are useful tools (filtering joins) to diagnose problems -->
<!--   - `anti_join()` keeps only the rows in `x` that *don't* have a match in `y` -->
<!--   - `semi_join()` keeps only the rows in `x` that *do* have a match in `y` -->

<!-- Exercise: nonexistent planes -->
<!-- ==== -->
<!-- It appears some of the `tailnum`s in `flights` do not appear in `planes`. Is there something those flights have in common that might help us diagnose the issue?  -->

<!-- Answer: nonexistent planes -->
<!-- ==== -->
<!-- ```{r} -->
<!-- bad_flight_carrier_count = flights %>%  -->
<!--   anti_join(planes, by="tailnum") %>% -->
<!--   sample_n(10) -->
<!-- ``` -->

<!-- Answer: nonexistent planes -->
<!-- ==== -->
<!-- It appears some of the `tailnum`s in `flights` do not appear in `planes`. Is there something those flights have in common that might help us diagnose the issue? -->
<!-- ```{r} -->
<!-- bad_flight_carrier_count = flights %>%  -->
<!--   anti_join(planes, by="tailnum") %>%  -->
<!--   count(carrier) %>%  -->
<!--   arrange(desc(n)) -->
<!-- bad_flight_carrier_count -->
<!-- ``` -->

<!-- - `count(x)` is a shortcut for `group_by(x) %>% summarize(n=n())`  -->

<!-- *** -->

<!-- Let's compare the counts of airlines with missing planes to the counts of airlines across all flight data -->
<!-- ```{r} -->
<!-- flight_carrier_count = flights %>%  -->
<!--   count(carrier) %>%  -->
<!--   arrange(desc(n)) -->
<!-- flight_carrier_count -->
<!-- ``` -->

<!-- Answer: nonexistent planes -->
<!-- ==== -->
<!-- We can already see the trend but let's clean it up a bit -->
<!-- ```{r, eval=F} -->
<!-- flight_carrier_count %>% -->
<!--   left_join(bad_flight_carrier_count,  -->
<!--             by="carrier",  -->
<!--             suffix=c("_all", "_bad")) %>% -->
<!--   replace_na(list(n_bad=0)) %>%  -->
<!--   mutate(bad_ratio = n_bad/n_all) %>% -->
<!--   left_join(airlines, by="carrier") %>% -->
<!-- ggplot(aes(y=name, x=bad_ratio)) +  -->
<!--   geom_point() -->
<!-- ``` -->

<!-- *** -->

<!-- ```{r, echo=F} -->
<!-- flight_carrier_count %>% -->
<!--   left_join(bad_flight_carrier_count,  -->
<!--             by="carrier",  -->
<!--             suffix=c("_all", "_bad")) %>% -->
<!--   replace_na(list(n_bad=0)) %>%  -->
<!--   mutate(bad_ratio = n_bad/n_all) %>% -->
<!--   left_join(airlines, by="carrier") %>% -->
<!-- ggplot(aes(y=name, x=bad_ratio)) +  -->
<!--   geom_point() -->
<!-- ``` -->

<!-- - Envoy Air and American Airlines are the culprits! -->

<!-- More about ggplot2 -->
<!-- ============================================================ -->
<!-- type: section -->

<!-- Distinguishing groups in plots -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- head(orange, 3) -->
<!-- ``` -->
<!-- - The data frame `orange` contains data on five trees. -->

<!-- Distinguishing groups in plots -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- ggplot(orange, aes(age, circumference)) +  -->
<!--   geom_point() -->
<!-- ``` -->
<!-- - If we plot it, we see the data from all the trees in a single -->
<!-- plot, but would be better to visually distinguish the different trees -->

<!-- Example of ggplot2 with color -->
<!-- ============================================================ -->
<!-- ```{r} -->
<!-- ggplot(orange, aes(age, circumference)) +  -->
<!--   geom_point(aes(color = Tree)) -->
<!-- ``` -->


<!-- Example of ggplot2 with color with larger dots -->
<!-- ============================================================ -->
<!-- ```{r} -->
<!-- ggplot(orange, aes(age, circumference)) +  -->
<!--   geom_point(aes(color = Tree), size=3) -->
<!-- ``` -->
<!-- - the aesthetics (what's inside of `aes()`) tell ggplot how the columns of the data relate to what should go on the plot -->
<!-- - things that don't relate to the data should go outisde of the call to `aes()`. e.g. `size=3` in this case. -->
<!-- - aesthetics defined in `ggplot()` are passed down to the geoms -->

<!-- Example of ggplot2 with plot shape -->
<!-- ============================================================ -->
<!-- ```{r tidy=FALSE} -->
<!-- ggplot(orange, aes(age, circumference)) +  -->
<!--   geom_point(aes(shape = Tree), size=3) -->
<!-- ``` -->


<!-- Example of ggplot2 with plot color and shape -->
<!-- ============================================================ -->
<!-- ```{r tidy=FALSE} -->
<!-- ggplot(orange, aes(age, circumference)) +  -->
<!--   geom_point(aes(color = Tree, shape = Tree), size=3) -->
<!-- ``` -->


<!-- Exercise: lines and colors -->
<!-- ======================================================== -->
<!-- Reproduce this plot -->

<!-- ```{r tidy=FALSE, echo=F} -->
<!-- ggplot(orange, aes(age, circumference)) + -->
<!--   geom_point(aes(color = Tree)) + -->
<!--   geom_line(aes(color = Tree)) -->
<!-- ``` -->

<!-- Answer: lines and colors -->
<!-- ======================================================== -->
<!-- ```{r tidy=FALSE, eval=F} -->
<!-- ggplot(orange, aes(age, circumference)) + -->
<!--   geom_point(aes(color = Tree)) + -->
<!--   geom_line(aes(color = Tree)) -->
<!-- ``` -->
<!-- or -->
<!-- ```{r tidy=FALSE} -->
<!-- orange %>% -->
<!-- ggplot(aes(age, circumference, color=Tree)) + -->
<!--   geom_point() + -->
<!--   geom_line() -->
<!-- ``` -->
<!-- - Recall that the aesthetics in `ggplot` are passed down to the geoms -->

<!-- Using the loess smoother -->
<!-- ======================================================== -->
<!-- ```{r tidy=FALSE,fig.width=11} -->
<!-- ggplot(orange, aes(age, circumference)) + -->
<!--   geom_point() + -->
<!--   facet_wrap(~ Tree) + -->
<!--   geom_smooth(method = loess, se = FALSE) -->
<!-- ``` -->

<!-- Exercise: loess with color -->
<!-- ======================================================== -->
<!-- Reproduce the following plot: -->

<!-- ```{r echo=FALSE} -->
<!-- ggplot(orange, aes(age, circumference, color=Tree)) + -->
<!--   geom_point() + -->
<!--   geom_smooth(method = loess, se = FALSE) -->
<!-- ``` -->

<!-- Answer: loess with color -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- ggplot(orange, aes(age, circumference, color=Tree)) + -->
<!--   geom_point() + -->
<!--   geom_smooth(method = loess, se = FALSE) -->
<!-- ``` -->

<!-- Aesthetics can be used within each plot element separately -->
<!-- ======================================================== -->
<!-- ```{r} -->
<!-- ggplot(orange, aes(age, circumference)) + -->
<!--   geom_point(aes(shape=Tree)) + -->
<!--   geom_smooth(aes(color=Tree), method = loess, se = FALSE) -->
<!-- ``` -->

<!-- Facets: multiple aligned plots by group -->
<!-- ============================================================ -->
<!-- ```{r tidy=FALSE,fig.width=14} -->
<!-- ggplot(orange, aes(age, circumference)) +  -->
<!--   geom_point(size = 3) +  -->
<!--   facet_wrap(~ Tree) -->
<!-- ``` -->


<!-- Alternative using facets -->
<!-- ======================================================== -->
<!-- ```{r fig.width=11} -->
<!-- orange %>% -->
<!-- ggplot(aes(age, circumference)) +  -->
<!--   geom_point() +  -->
<!--   geom_line() + -->
<!--   facet_wrap(~ Tree) -->
<!-- ``` -->


<!-- Facet syntax -->
<!-- ======================================================== -->
<!-- - `facet_wrap(~ facet_variable)`: This puts the -->
<!-- facets left-to-right, top-to-bottom, wrapping around. -->
<!-- - `facet_grid(y-variable ~ .)`: This puts them in -->
<!-- a vertically-aligned stack, by value of the y-variable. -->
<!-- - `facet_grid(. ~ x-variable)`: This puts them in -->
<!-- a horizontally-aligned stack, by value of the x-variable. -->


<!-- Facet grid example 1 -->
<!-- ======================================================== -->
<!-- ```{r tidy=FALSE,fig.width=13} -->
<!-- ggplot(orange, aes(age, circumference)) +   -->
<!--   geom_point() + -->
<!--   facet_grid(. ~ Tree) -->
<!-- ``` -->


<!-- Facet grid example 2 -->
<!-- ======================================================== -->
<!-- ```{r tidy=FALSE,fig.width=11} -->
<!-- ggplot(orange, aes(age, circumference)) +  -->
<!--   geom_point() + -->
<!--   facet_grid(Tree ~ .) -->
<!-- ``` -->


<!-- Fitting straight lines -->
<!-- ======================================================== -->
<!-- - The data in each facet show a nearly linear relation between -->
<!-- circumference and age. -->
<!-- - R can calculate the least squares fit line and plot it atop -->
<!-- the data points. -->


<!-- Facet example with fitted lines (linear regression) -->
<!-- ======================================================== -->
<!-- ```{r tidy=FALSE,fig.width=11} -->
<!-- ggplot(orange, aes(age, circumference)) + -->
<!--   geom_point() + -->
<!--   facet_wrap(~ Tree) + -->
<!--   geom_smooth(method = lm, se=FALSE) -->
<!-- ``` -->


<!-- Error bars -->
<!-- ============================================================ -->
<!-- - Let's add error bars to a plot. We'll use the state data, which has -->
<!-- multiple area measurements for each of the four regions. -->

<!-- Error bars, first plot -->
<!-- ============================================================= -->
<!-- ```{r} -->
<!-- ggplot(state_data, aes(region, area)) +  -->
<!--   geom_point() -->
<!-- ``` -->

<!-- Error bars, computing group mean and standard deviation -->
<!-- ============================================================ -->
<!-- ```{r tidy=FALSE} -->
<!-- plot = state_data %>% -->
<!--   group_by(region) %>% -->
<!--   summarize( -->
<!--     region_mean = mean(area), -->
<!--     region_sd = sd(area)) %>% -->
<!-- ggplot(aes(region, region_mean)) +  -->
<!--   geom_point() -->
<!-- plot -->
<!-- ``` -->

<!-- Error bars: plot the error bars -->
<!-- ============================================================ -->
<!-- ```{r tidy=FALSE} -->
<!-- plot + -->
<!--   geom_errorbar(aes(ymin = region_mean - region_sd, -->
<!--                     ymax = region_mean + region_sd, -->
<!--                     width = 0.3)) -->
<!-- ``` -->


<!-- ggplot2 and the + operator -->
<!-- ======================================================== -->
<!-- - `ggplot(...) + geom_point()` is a strange -->
<!-- expression: it uses the `+` operator to add things (plots and -->
<!-- plot specifications), which are not numbers. -->
<!-- - This uses a feature called generic functions: the types -->
<!-- of the arguments to `+` determine which piece of code, called a -->
<!-- method, to run. -->
<!-- - ggplot2 relies on this feature heavily. -->
<!-- ```{r eval=FALSE} -->
<!-- p <- ggplot(mtc, aes(hp, mpg)) -->
<!-- l <- geom_point() -->
<!-- p + l -->
<!-- ``` -->
<!-- - Note you can save parts of the graph specification and then add -->
<!-- them together. -->
<!-- - The graph will only be displayed when the third expression is -->
<!-- evaluated. -->

<!-- More on ggplot2 -->
<!-- === -->
<!-- - Almost everything is customizable in ggplot.  -->
<!-- - See R for Data Science ch. 28 for more on changing legends, colors, axis labels, etc. -->
<!-- - There are thousands of examples online -->

<!-- <div align="left"> -->
<!-- <img src="https://www.r-graph-gallery.com/wp-content/uploads/2017/12/327_Chloropleth_geoJSON_ggplot2_4.png", height=700, width=1400> -->
<!-- </div> -->


<!-- Reproducible analysis -->
<!-- ============================================================ -->
<!-- type: section -->


<!-- Reproducible analysis -->
<!-- ======================================================== -->

<!-- The goal of reproducible analysis is to produce a computational -->
<!-- artifact that others can view, scrutinize, test, and run, to convince -->
<!-- themselves that your ideas are valid. (It's also good for you to be as -->
<!-- skeptical of your work.) This means you should write code to be run -->
<!-- more than once and by others. -->

<!-- Doing so requires being organized in several ways: -->

<!-- - Combining text with code (the focus of this section) -->
<!-- - Project/directory organization -->
<!-- - Version control -->

<!-- The problem -->
<!-- ======================================================== -->
<!-- - You write text in a word processor. -->
<!-- - You write code to compute with data and produce output and -->
<!-- graphics. -->
<!-- - These are performed using different software. -->
<!-- - So when integrating both kinds of information into a notebook, -->
<!-- report, or publication, it is very easy to make mistakes, -->
<!-- copy/paste the wrong version, and have information out of sync. -->


<!-- A solution -->
<!-- ======================================================== -->
<!-- - Write text and code in the same file. -->
<!-- - Use special syntax to separate text from code. -->
<!-- - Use special syntax for annotating the text with formatting -->
<!-- operations (if desired). -->
<!-- - RStudio can then: -->
<!-- 1. run the code blocks, -->
<!-- 2. insert the output and graphs at the correct spot in the text, -->
<!-- 3. then call a text processor for final formatting. -->
<!-- - This whole process is called "knitting". -->
<!-- - (live demo) -->
<!-- - jupyter notebooks (outside of RStudio) are an alternative for doing the same thing -->


<!-- R Markdown: The special syntax for formatting text -->
<!-- ======================================================== -->
<!-- - RStudio supports a simple and easy-to-use format called "R Markdown". -->
<!-- - This is a very simple markup language: -->
<!-- - use * or _ around italics. -->
<!-- - use ** or __ around bold. -->
<!-- - Markdown Quick Reference (RStudio internal help) -->
<!-- - [Introduction to R Markdown](http://shiny.rstudio.com/articles/rmarkdown.html) -->
<!-- - [R Markdown web page](http://rmarkdown.rstudio.com/) -->
<!-- - [R Markdown Cheat Sheet](http://shiny.rstudio.com/articles/rm-cheatsheet.html) -->


<!-- Wrap-up -->
<!-- ======================================================== -->
<!-- type: section -->

<!-- Goals of this course -->
<!-- ======================================================== -->
<!-- By the end of the course you should be able to... -->

<!-- - write neat R scripts and markdown reports in R studio -->
<!-- - find, read, and understand package and function documentation  -->
<!-- - read and write tabular data into R from flat files -->
<!-- - perform basic manipulations on tabular data: subsetting, manipulating and summarizing data, and joining -->
<!-- - visualize tabluar data using line and scatter plots along with color and facets -->

<!-- <div align="center"> -->
<!-- <img src="https://d33wubrfki0l68.cloudfront.net/571b056757d68e6df81a3e3853f54d3c76ad6efc/32d37/diagrams/data-science.png" width=800 height=300> -->
<!-- </div> -->

<!-- Tidyverse -->
<!-- ======================================================== -->
<!-- <div align="center"> -->
<!-- <img src="https://pbs.twimg.com/media/DuRP1tVVAAEcpxO.jpg"> -->
<!-- </div> -->

<!-- Resources for this course -->
<!-- ======================================================== -->

<!-- R for Data Science (R4DS): https://r4ds.had.co.nz -->
<!-- <div align="center"> -->
<!-- <img src="https://r4ds.had.co.nz/cover.png"> -->
<!-- </div> -->

<!-- *** -->

<!-- - Fundamentals: ch 1, 4, 6 -->
<!-- - Input/output: ch 11 -->
<!-- - Data manipulation: ch 5, 13 -->
<!-- - Visualization: ch 3, 28 -->

<!-- Cheatsheets: https://www.rstudio.com/resources/cheatsheets/ -->
