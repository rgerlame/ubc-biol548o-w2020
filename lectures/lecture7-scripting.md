For today's exercise, you will need to read in "shrub-volume-datan.csv" which should already be in your data folder :-)

### Understandable and reusable code

* Write code in understandable chunks.
* Write reusable code.

### Understandable chunks

* Human brain can only hold ~7 things in memory.
    * Write programs that don't require remembering more than ~7 things at once.
* What do you know about how `sum(1:5)` works internally?
    * Nothing.
    * Ignore the details and reduce `sum()` to a single conceptual chunk.
* All functions should work as a single conceptual chunk.

### Reuse

* Want to do the same thing repeatedly?
    * Inefficient & error prone to copy code
    * If it occurs in more than one place, it will eventually be wrong somewhere.
* Functions are written to be reusable.


### Function basics

```
function_name <- function(inputs) {
  output_value <- do_something(inputs)
  return(output_value)
}
```

* The braces indicate that the lines of code are a group that gets run together

```r
{a = 2
b = 3
a + b}
```

* Pressing run anywhere in this group runs all the lines in that group
* A function runs all of the lines of code in the braces
* Using the arguments provided
* And then returns the output

```
calc_shrub_vol <- function(length, width, height) {
  volume <- length * width * height
  return(volume)
}
```

* Creating a function doesn't run it.
* Call the function with some arguments.

```r
calc_shrub_vol(0.8, 1.6, 2.0)
```

* Store the output to use it later in the program

```r
shrub_vol <- calc_shrub_vol(0.8, 1.6, 2.0)
```

### Exercise 1

Write a function that converts pounds to grams (*there are 453.592 grams in one pound*). It should take a value in pounds as the input and return the equivalent value in grams (i.e., the number of pounds times 453.592). Use that function to calculate how many grams there are in 3.75 pounds.

In a new document `lecture7-exercises.R`, write code to do this. Add, commit, and push.

### Exercise 2

The length of an organism is typically strongly correlated with its body
mass. This is useful because it allows us to estimate the mass of an organism
even if we only know its length. This relationship generally takes the form:

Mass = a * Length<sup>b</sup>

Where the parameters `a` and `b` vary among groups. This allometric approach is
regularly used to estimate the mass of dinosaurs since we cannot weigh something
that is only preserved as bones.

The following function estimates the mass of an organism in kg based on its
length in meters for a particular set of parameter values, those for *Theropoda*
(where `a` has been estimated as `0.73` and `b` has been estimated as `3.63`;
[Seebacher 2001](http://www.jstor.org/stable/4524171)).

```
get_mass_from_length_theropoda <- function(length){
  mass <- 0.73 * length ^ 3.63
  return(mass)
}
```

1. In your file `lecture7-exercises.R`, add a comment to this function so that you know what it does.
2. Use this function to print out the mass of a Spinosaurus that is 16 m long
   based on its reassembled skeleton.
3. Create a new version of this function called `get_mass_from_length()` that
   estimates the mass of an organism in kg based on its length in meters by
   taking `length`, `a`, and `b` as parameters. This lets us pass the function
   all 3 values that it needs to estimate a mass as parameters, which makes it
   much easier to reuse for all of the non-theropod species. Use this new
   function to estimate the mass of a Sauropoda (`a = 214.44`, `b = 1.46`) that
   is 26 m long.
   
Add, commit, and push.


### Default arguments

* Defaults can be set for common inputs.

```
calc_shrub_vol <- function(length = 1, width = 1, height = 1) {
  volume <- length * width * height
  return(volume)
}

calc_shrub_vol()
calc_shrub_vol(width = 2)
calc_shrub_vol(0.8, 1.6, 2.0)
calc_shrub_vol(height = 2.0, length = 0.8, width = 1.6)
```

### Exercise 3

Allowing `a` and `b` to be passed as arguments to `get_mass_from_length()` made the function more flexible, but for some types of dinosaurs we don't have specific values of `a` and `b` and so we have to use general values that can be applied to a number of different species.

Set default values for `a` and `b` of `a = 39.9` and `b = 2.6` (the average values from [Seebacher 2001](http://www.jstor.org/stable/4524171)).

1. Use this function to estimate the mass of a Sauropoda (`a = 214.44`, `b = 1.46`) that
   is 22 m long (by setting `a` and `b` when calling the function).
2. Use this function to estimate the mass of a dinosaur from an unknown
   taxonomic group that is 16m long (by not setting `a` and `b` so that the
   default values are used).
   
Add, commit, and push.

> *Discuss why passing `a` and `b` in is more useful than having them fixed*

### Named vs unnamed arguments

* When to use or not use argument names

```
calc_shrub_vol(length = 2.0, width = 0.8, height = 1.6)
```

Or

```
calc_shrub_vol(2.0, 0.8, 1.6)
```

* You can always use names
    * Value gets assigned to variable of that name
* If not using names then order determines naming
    * First value is `length`, second value is `width`, third value is `height`
    * If order is hard to remember use names
* In many cases there are *a lot* of optional arguments
    * Convention to always name optional argument

### Combining Functions

* Each function should be single conceptual chunk of code
* Functions can be combined to do larger tasks in two ways

* Calling multiple functions in a row

```r
est_shrub_mass <- function(volume){
  mass <- 2.65 * volume^0.9
}

shrub_volume <- calc_shrub_vol(0.8, 1.6, 2.0)
shrub_mass <- est_shrub_mass(shrub_volume)
```

* We can also use pipes with our own functions
* The output from the first function becomes the first argument for the second function

```r
library(dplyr)
shrub_mass <- calc_shrub_vol(0.8, 1.6, 2.0) %>%
  est_shrub_mass()
```

### Exercise 4

Measuring things using the metric system is the standard approach for scientists, but when communicating your results more broadly it may be useful to use different units (at least in some countries). Write a function that converts kilograms into pounds (there are 2.205 pounds in a kilogram). Use that function along with your `get_mass_from_length()` function from Exercise 2
to estimate the weight, in pounds, of a 12 m long Stegosaurus. In *Stegosauria*, `a` has been estimated as `10.95` and `b` has been estimated as
`2.64` ([Seebacher2001](http://www.jstor.org/stable/4524171)).

Write code to do this. Add, commit, and push.

* We can nest functions

```r
shrub_mass <- est_shrub_mass(calc_shrub_vol(0.8, 1.6, 2.0))
```

* But we careful with this because it can make code difficult to read
* Don't nest more than two functions

* Can also call functions from inside other functions
* Allows organizing function calls into logical groups

```
est_shrub_mass_dim <- function(length, width, height){
  volume = calc_shrub_vol(length, width, height)
  mass <- est_shrub_mass(volume)
  return(mass)
}

est_shrub_mass_dim(0.8, 1.6, 2.0)
```

* We ***don't*** need to pass the function name into the function
* That's the one violation of the black box rule

### Documentation & Comments

* Documentation
    * How to use code
    * Use Roxygen comments for functions
* Comments
    * Why & how code works
    * Only if it code is confusing to read

### Working with functions in RStudio

* It is possible to find and jump between functions
* Click on list of functions at bottom of editor and select

* Can be helpful to clearly see what is a function
* Can have RStudio highlight them
*  Global Options -> Code -> Display -> Highlight R function calls

### Some notes on R Style

### Whitespace

* Indentation what the brain recognizes, use it regardless of language (and braces)
* 2 space indents

```
stuff <- data %>%
filter() %>%
select()
things <- 2
```

```
stuff <- data %>%
  filter() %>%
  select()
things <- 2
```

* Spaces after commas and around operators

```
range(x, x + 10)
```

### Naming

* snake_case
* Descriptive: `estimate_biomass`
* Not too long: `estimate_biomass_using_hanson_method`
* Use whole words for accessibility

### Long lines

* Lines < 80 characters long
* Add an indicator for this in RStudio
    * Tools -> Global Options -> Code -> Display -> Show Margin
* Use implied line continuation
    * If it's clear that a line isn't finished R will go on to the next line

```
site_data <- data.frame(
  site_id = c(1, 2, 3, 4),
  type = c('control', 'control', 'experiment', 'experiment')
)
```

* Read top to bottom
