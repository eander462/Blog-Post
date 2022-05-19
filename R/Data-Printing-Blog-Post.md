---
title: "Blog post 2"
author: "Erik Andersen"
date: "19 May, 2022"
output: 
  html_document:
    toc: true
    toc_float: true
    keep_md: yes
---



## Intro

Hello and welcome to the first in our weekly^[Definietly not in anyway weekly.] posts I am eloquently calling "Erik Discovers a New Cool Feature in R and Wants to Share It". EDNCFRWSI for short. 

Today I will be showing you some very simple ways to print wrangled data to screen without hoving to annoyingly type its name again. I can already hear you saying "Erik this is easy, and you shouldn't sully your pristine blog with such nonsense," but worry not good reader; I have two degrees in economics where all my professors use R, and I haven't seen one of them use write code this way^[Hopefully that is because they don't know this simple trick, and not because they have decided its not useful. If its the latter, this post would be a bit embarassing.]. We are going to look at two methods: one is part of base R, and the other is a [data.table](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html){target="_blank"} function (because data.table is a rad package).

## Boring data wrangling

We'll begain by looking at the way most people I've seen print out their wrangled object using the [mtcars](https://www.rdocumentation.org/packages/datasets/versions/3.6.2/topics/mtcars){targer="_blank"} dataset from base R. 

Let's say we want to create a really useful variable that tells us the horsepower to cylinders I'll use the base function [within](https://www.rdocumentation.org/packages/plm/versions/0.1-2/topics/within){targer="_blank"} to show this doesn't rely on the tidyverse or any other dependancies.


```r
cars = within(mtcars, ratio <- hp / cyl) |> # Note I have to use <- rather than = for the within function
  subset(select = ratio) |> 
  head(5)

# Now we have to type the name again to print out the new cars object
cars
```

```
##                      ratio
## Mazda RX4         18.33333
## Mazda RX4 Wag     18.33333
## Datsun 710        23.25000
## Hornet 4 Drive    18.33333
## Hornet Sportabout 21.87500
```

We had to use a whole extra line^[For a while, the pinnacle of R coding for me was using a semicolon at the end of the first line the writing the object's name after to print it. E.g cars = within(...);cars] there and type four extra letters! That's clearly unacceptable. Let's see how we can do it better.

### Base R way

It is really simple to print out our cars object without typing it again. This is lucky because its not actually very annoying to just type it again. All we have to do is wrap our expression in parentheses^[Credit [R for Data Science by Hadley Wickham])https://r4ds.had.co.nz/workflow-basics.html#calling-functions){target="_blank"}.]


```r
# All that's different is we wrapped the expression in parenthesis
(cars = within(mtcars, ratio <- hp / cyl) |> 
   subset(select = ratio) |> 
   head(5))
```

```
##                      ratio
## Mazda RX4         18.33333
## Mazda RX4 Wag     18.33333
## Datsun 710        23.25000
## Hornet 4 Drive    18.33333
## Hornet Sportabout 21.87500
```

And its exactly the same output. Easy! I don't have anymore to say on this since its so simple, so let's move quickly on to data.table.

### Data.table

Data.table is a powerful data wrangling tool, and most pertinent to our purposes, it has its own method for printing out the new object on the same line^[Check out [Grant McDermot's](https://raw.githack.com/uo-ec607/lectures/master/05-datatable/05-datatable.html#1){target="_blank"} for an excellent guild to data.table.]. If you've never used data.table before, ignore the specifics of how the wrangling works, all we're really interested in is the final "[]" which serves the same function as the parenthesis in base R.


```r
# Load data.table
library(data.table)

# We first need to make the mtcars object a data.table object
cars = as.data.table(mtcars)

# The [] at the end prints out the new object
cars[, ratio := hp/cyl][, ratio][] |> head()
```

```
## [1] 18.33333 18.33333 23.25000 18.33333 21.87500 17.50000
```

```r
# Note converting mtcars to a data.table got rid of the car names because of how they are stored in the original object
```

Again its very simple.

## Conclusion

I hope these tricks will be useful to you. I know they are basic, but I haven't seen anyone use them, and I have found it very helpful. Go forth now and change the world with all the time you'll save with these simple tricks. 














