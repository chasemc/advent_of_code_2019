2
================
Chase Clark
12/2/2019

# Part 1

``` r
input <- c(1,0,0,3,1,1,2,3,1,3,4,3,1,5,0,3,2,10,1,19,1,19,9,23,1,23,13,27,1,10,27,31,2,31,13,35,1,10,35,39,2,9,39,43,2,43,9,47,1,6,47,51,1,10,51,55,2,55,13,59,1,59,10,63,2,63,13,67,2,67,9,71,1,6,71,75,2,75,9,79,1,79,5,83,2,83,13,87,1,9,87,91,1,13,91,95,1,2,95,99,1,99,6,0,99,2,14,0,0)
```

> before running the program, replace position 1 with the value 12 and
> replace position 2 \>with the value 2.

Instructions say to replace positions 1 and 2, but R is not 0-indexed

``` r
input[2] <- 12
input[3] <- 2
```

``` r
i <- 1
buzzkill <- input[i]

while (buzzkill == 1L || buzzkill == 2) {
  
  buzzkill <- input[i]
  if(buzzkill == 99) break
  
  value_1 <- input[input[i + 1] + 1]
  value_2 <- input[input[i + 2] + 1]
  
  if (input[i] == 1L){
  input[input[i + 3] + 1] <- value_1 + value_2
  }

if (input[i] == 2L) {
  input[input[i + 3] + 1] <- value_1 * value_2
}

i <- i + 4

}

cat("Answer:", input[1])
```

    ## Answer: 3085697

-----

# Part 2

``` r
input <- c(1,0,0,3,1,1,2,3,1,3,4,3,1,5,0,3,2,10,1,19,1,19,9,23,1,23,13,27,1,10,27,31,2,31,13,35,1,10,35,39,2,9,39,43,2,43,9,47,1,6,47,51,1,10,51,55,2,55,13,59,1,59,10,63,2,63,13,67,2,67,9,71,1,6,71,75,2,75,9,79,1,79,5,83,2,83,13,87,1,9,87,91,1,13,91,95,1,2,95,99,1,99,6,0,99,2,14,0,0)
```

> before running the program, replace position 1 with the value 12 and
> replace position 2 \>with the value 2.

Instructions say to replace positions 1 and 2, but R is not 0-indexed

``` r
input[2] <- 12
input[3] <- 2
```

``` r
my_func <- function(input,
                    combination){
  
  input[2] <- combination[[1]]
  input[3] <- combination[[2]]
  
  i <- 1
  buzzkill <- input[i]
  
  while (buzzkill == 1L || buzzkill == 2) {
    
    buzzkill <- input[i]
    if(buzzkill == 99) break
    
    value_1 <- input[input[i + 1] + 1]
    value_2 <- input[input[i + 2] + 1]
    
    if (input[i] == 1L){
      input[input[i + 3] + 1] <- value_1 + value_2
    }
    
    if (input[i] == 2L) {
      input[input[i + 3] + 1] <- value_1 * value_2
    }
    
    i <- i + 4
    
  }

  input[1] 
}
```

``` r
grid_search <- as.data.frame(t(expand.grid(1:100, 100:1)))

zap <- sapply(grid_search,
              function(x){
                my_func(input = input,
                        combination = x)
              })
```

``` r
answer <- grid_search[ , which(zap == 19690720)]

answer
```

    ## [1] 94 25

``` r
(100 * answer[1]) + answer[2] 
```

    ## [1] 9425
