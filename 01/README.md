Advent of Code; Day 01
================
Chase Clark
12/1/2019

``` r
library(here)
```

    ## here() starts at C:/Users/CMC/Documents/GitHub/advent_of_code

### Part 1

`https://adventofcode.com/2019/day/1`

Data from: `https://adventofcode.com/2019/day/1/input`

Read data

``` r
file_path <- here::here("01/input.txt")
masses <- read.table(file_path)
```

Instead of a data.frame with 100 rows, let’s convert to a vector of
length 100

``` r
masses <- masses[ , 1]
```

The instructions were to…

    take its mass, divide by three, round down, and subtract 2.

For clarity I’ll do each step separately

``` r
new_masses <- masses / 3

new_masses <- floor(new_masses)

new_masses <- new_masses - 2 
```

And the total:

``` r
sum(new_masses)
```

    ## [1] 3563458

-----

### Part Two

Instructions:

    for each module mass, calculate its fuel and add it to the total. Then, treat the fuel amount you just calculated as the input mass and repeat the process, continuing until a fuel requirement is zero or negative.

Make a function of the fuel calculation

``` r
my_func <- function(x){
  as.integer(floor(x  / 3) - 2)
}
```

We can calculate the maximum iterations we’ll have to complete and then
only loop that many times, using vectorized functions.

The max mass is

``` r
max(masses)
```

    ## [1] 149508

Find the max number of iterations needed.

``` r
temp <- max(masses)
i <- 0

while(temp >= 0L){
  temp <- my_func(temp)
  i <- i + 1
}

i
```

    ## [1] 10

Create a matrix with as many rows as there modules, and 10 columns as
determined above. Also, append the module masses as the first column

``` r
new_masses <- matrix(data = 0L,
                     nrow = length(masses), 
                     ncol = 10)

new_masses <- cbind(masses, 
                    new_masses)
```

Now just iterate the 10 times, repl

``` r
for(i in 1:10){
  new_masses[ , i + 1] <- my_func(new_masses[ , i])
}

knitr::kable(head(new_masses))
```

| masses |       |       |      |      |     |     |    |    |   |     |
| -----: | ----- | ----- | ---- | ---- | --- | --- | -- | -- | - | --- |
|  85364 | 28452 | 9482  | 3158 | 1050 | 348 | 114 | 36 | 10 | 1 | \-2 |
|  97431 | 32475 | 10823 | 3605 | 1199 | 397 | 130 | 41 | 11 | 1 | \-2 |
| 135519 | 45171 | 15055 | 5016 | 1670 | 554 | 182 | 58 | 17 | 3 | \-1 |
| 119130 | 39708 | 13234 | 4409 | 1467 | 487 | 160 | 51 | 15 | 3 | \-1 |
| 137800 | 45931 | 15308 | 5100 | 1698 | 564 | 186 | 60 | 18 | 4 | \-1 |
|  85946 | 28646 | 9546  | 3180 | 1058 | 350 | 114 | 36 | 10 | 1 | \-2 |

Replace negative numbers with `0`

``` r
new_masses[new_masses < 0] <- 0

knitr::kable(head(new_masses))
```

| masses |       |       |      |      |     |     |    |    |   |   |
| -----: | ----- | ----- | ---- | ---- | --- | --- | -- | -- | - | - |
|  85364 | 28452 | 9482  | 3158 | 1050 | 348 | 114 | 36 | 10 | 1 | 0 |
|  97431 | 32475 | 10823 | 3605 | 1199 | 397 | 130 | 41 | 11 | 1 | 0 |
| 135519 | 45171 | 15055 | 5016 | 1670 | 554 | 182 | 58 | 17 | 3 | 0 |
| 119130 | 39708 | 13234 | 4409 | 1467 | 487 | 160 | 51 | 15 | 3 | 0 |
| 137800 | 45931 | 15308 | 5100 | 1698 | 564 | 186 | 60 | 18 | 4 | 0 |
|  85946 | 28646 | 9546  | 3180 | 1058 | 350 | 114 | 36 | 10 | 1 | 0 |

And summing our answer:

``` r
# We don't want the initial mass so we remove the first column
new_masses <- new_masses[ , -1]
# If we wanted each module's fuel consumption we could do: rowSums(new_masses)
# But we don't, we only want the sum of all the modules
cat("Answer: ", sum(new_masses))
```

    ## Answer:  5342292
