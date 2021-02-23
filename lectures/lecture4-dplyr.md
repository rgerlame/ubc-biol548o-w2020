### Packages

Today we are going to be using the `dplyr` package, which comes bundled as part of the `tidyverse`.

You can either load `dplyr` on its own
```
library(dplyr)
```
Or just bring the whole tidyverse along for the ride
```
library(tidyverse)
```
For the following we are going to be using multiple packages from the tidyverse so you may as well load the whole thing.

**Cool trick:** you can call functions from a package without loading it using the `::` notation. For example,`dplyr::select()` allows you to use the `select()` function even if `dplyr` isn't loaded. Why would you want to do this? First, it saves memory (potentially important if you are loading in a ton of packages and data) and second, it is useful if multiple packages have functions with the same names (which happens more often than you might think).

**Even cooler trick:** if you feel like living on the edge, you can even call hidden, internal functions from packages using `:::`. Typically only a very small subset of functions are **exported** into what is known as the `NAMESPACE`. Each of these functions will hopefully have documentation and can be called directly once you've loaded the library. But all the little functions that are called by the visible functions are still there and available to use; unfortunately these are not documented so you have to have an idea of what the function does.

### Introduction to tabular data

* We will be working with data from a real project called the Portal Project.
    * Long-term experimental study of small mammals in Arizona.
    * Download `surveys` from `data` into folder of the class repository and put it in the `data` folder of your repository.
    
* Load this into `R` using `read_csv()`.

```
surveys <- read_csv("data/surveys.csv")

```

* Display data by clicking on it in `Environment`

* `surveys` - main table, one row for each rodent captured, date on date,
      location, species ID, sex, and size

* Good tabular data structure -- keep it **tidy**
    * One table per type of data
        * Tables can be linked together to combine information.
    * Each row contains a single record.
        * A single observation or data point
    * Each column or field contains a single attribute.
        * A single type of information


### Basic `dplyr`

* Modern data manipulation library for R

```
surveys <- read_csv("data/surveys.csv")
```

* Select a subset of columns.

```
select(surveys, year, month, day)
```

* They can occur in any order.

```
select(surveys, month, day, year)
```

* Use `filter()` to get only the rows that meet certain criteria.
    * Combine the data frame to be filtered with a series of conditional statements.
    * Column, condition, value
  
```
filter(surveys, species_id == "DS")
```

* Add new columns with calculated values using `mutate()`

```
mutate(surveys, hindfoot_length_cm = hindfoot_length / 10)
```

* If we look at `surveys` now will it contain the new column?
* *Open `surveys`*
* All of these commands produce new values, data frames in this case
* To store them for later use we need to assign them to a variable

```
surveys_plus <- mutate(surveys,
                       hindfoot_length_cm = hindfoot_length / 10)
```

* Or we could overwrite the existing variable if we don't need it

```
surveys <- mutate(surveys,
                  hindfoot_length_cm = hindfoot_length / 10)
```

### Exercise 1

Dr. X is interested in studying the factors controlling the size and
carbon storage of shrubs. She has conducted an experiment looking at the effect
of three different treatments on shrub volume at four different locations. She
has placed the data file on the web for you to download:

* [Shrub dimensions data](https://github.com/mwpennell/ubc-biol548o-w2020/blob/master/data/shrub-volume-data.csv)

Download this into your `data` folder and get familiar with the data by
importing it using `read_csv()` and then:

1. Check the column names in the data using the function `names()`.
2. Use `str()` to show the structure of the data frame and its individual 
   columns.

   *Use `dplyr` to complete the remaining tasks.*
3. Select the data from the length column and print it out.
4. Select the data from the site and experiment columns and print it out.
5. Filter the data for all of the plants with heights greater than 5 and
   print out the result.
6. Create a new **tibble** or **data.frame** called `shrub_data_w_vols` that includes all of the original data and a new column containing the volumes, and display it.
   
Add, commit, and push your code to GitHub.

### Basic aggregation

* Aggregation combines rows into groups based on one of more columns.
* Calculates combined values for each group.
* First, group the data frame.

```
group_by(surveys, species_id)
```

* Different looking kind of `data.frame`
    * Source, grouping, and data type information

```r
surveys_by_species <- group_by(surveys, species_id)
```

* Use `summarize()` to calculate values for each group.
* Count the number of rows for each group (individuals in each species).

```r
summarize(surveys_by_species, abundance = n())
```

* Can group by multiple columns

```r
surveys_by_species_plot <- group_by(surveys, species_id, plot_id)
summarize(surveys_by_species, abundance = n())
```

* Use any function that returns a single value from a vector.
* E.g., mean, max, min

```r
species_weight <- summarize(surveys_by_species, avg_weight = mean(weight))
```

* *Open table*
* Why did we get `NA`?
    * `mean(weight)` returns `NA` when `weight` has missing values (`NA`)
* Can fix using `mean(weight, na.rm = TRUE)`

```
species_weight <- summarize(surveys_by_species,
                            avg_weight = mean(weight, na.rm = TRUE))
```

* Still has `NaN` for species that have never been weighed
* Can use `na.omit()` to drop rows with `NA` or `NaN` in any column

```
na.omit(species_weight)
```

#### Intermediate variables

* Run a command
* Store the output in a variable
* Use that variable later in the code
* Repeat

* Determine the mean weight of DS in each year

```
ds_data <- filter(surveys, species_id == "DS")
ds_data_by_year <- group_by(ds_data, year)
ds_weight_by_year <- summarize(ds_data_by_year,
                               avg_weight = mean(weight, na.rm = TRUE))
```

### Exercise 2

In this exercise you will use the `surveys` data you downloaded above. For each of the 5 tasks below, start anew from the 'surveys' data.

***Do not use pipes for this exercise.***

1. Use `select()` to create a new data frame with just the `year`, `month`,
   `day`, and `species_id` columns in that order.
2. Use `mutate()`, `select()`, and `na.omit()` to create a new data frame with
   the `year`, `species_id`, and weight **in kilograms** of each individual,
   with no null weights. The weight in the table is given in grams so you will
   need to divide it by 1000.
3. Use the `filter()` function to get all of the rows in the data frame for the
   species ID `SH`.
4. Use the `group_by()` and `summarize()` functions to get a count of the number
   of individuals in each species ID.
5. Use the `group_by()` and `summarize()` functions to get a count of the number
   of individuals in each species ID in each year.
6. Use the `filter()`, `group_by()`, and `summarize()` functions to get the mean
   mass of species `DO` in each year.
   
Add, commit, and push your code to GitHub.

#### Pipes

* Intermediate variables can get cumbersome if their are lots of steps.
* `%>%` ("pipe") takes the output of one command and passes it as input to the
  next command
* Want to take the mean of a vector
* Normally we would run the `mean` function with the vector as the input:

```
x = c(1, 2, 3)
mean(x)
```

* Instead we could pipe the vector into the function

```
x %>% mean()
```

* So `x` becomes the first argument in `mean`
* If we want to add other arguments they get added to the function call

```
x = c(1, 2, 3, NA)
mean(x, na.rm = TRUE)
x %>% mean(na.rm = TRUE)
```


```
surveys %>%
  filter(species_id == "DS", !is.na(weight))
```

```
ds_weight_by_year <- surveys %>%
  filter(species_id == "DS") %>%
  group_by(year) %>%
  summarize(avg_weight = mean(weight, na.rm = TRUE))
```

### Exercise 3 

Again working with the `surveys` data, use pipes (`%>%`) to combine the following operations to manipulate the data. For each of the 5 tasks below, start anew from the 'surveys' data.

1. Use `mutate()`, `select()`, and `na.omit()` to create a new data frame with
   the `year`, `species_id`, and weight **in kilograms** of each individual,
   with no null weights.
2. Use the `filter()` and `select()` to get the `year`, `month`, `day`, and
   `species_id` columns for all of the rows in the data frame where `species_id`
   is `SH`.
3. Use the `group_by()` and `summarize()` functions to get a count of the number
   of individuals in each species ID.
4. Use the `group_by()` and `summarize()` functions to get a count of the number
   of individuals in each species ID in each year.
5. Use the `filter()`, `group_by()`, and `summarize()` functions to get the mean
   mass of species `DO` in each year.
   
Add, commit, and push your code to GitHub



### Multiple filter conditions

* We can filter on multiple conditions at once
* In computing we combine conditions in two ways "and" & "or"
* "and" means that all of the conditions must be true
* Do this in `dplyr` using either additional comma separate arguments

```
filter(surveys, species_id == "DS", year > 1995)
```

* Or by using `&`

```
filter(surveys, species_id == "DS" & year > 1995)
```

* "or" means that one or more of the conditions must be true
* Do this using `|`
* Say we wanted data on all of the *Dipodomys* species.

```
filter(surveys, species_id == "DS" | species_id == "DM" | species_id == "DO")
```

### Filtering by aggregated properties

* You can also filter based on aggregated values
* If we wanted to estimate species weights only for species with > 100 individuals

```r
species_weights <- surveys %>%
  group_by(species) %>%
  filter(n() > 100) %>%
  summarize(avg_weight = mean(weight, na.rm = TRUE))
```
