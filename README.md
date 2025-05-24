# Quickly Exploring Data

`ggplot2` is note the only way to plot data. In some situations, it's useful to use the plotting functions in base R.

## Scatter Plot

```r
plot(mtcars$wt, mtcars$mpg)
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524181432060.png)

```r
library(ggplot2)

ggplot(mtcars, aes(x = wt, y = mpg)) +
  geom_point()
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524182013554.png)

> “The first part, ggplot(), tells it to create a plot object, and the\
> second part, geom\_point(), tells it to add a layer of points to the\
> plot.
>
> The usual way to use ggplot() is to pass it a data frame (mtcars)\
> and then tell it which columns to use for the x and y values. If you\
> want to pass it two vectors for x and y values, you can use\
> data = NULL, and then pass it the vectors.”
>
> Excerpt From\
> R Graphics Cookbook\
> Winston Chang\
> This material may be protected by copyright.

## Line Graph

```r
plot(pressure$temperature, pressure$pressure, type = "l")
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524182235955.png)

To add points and/or multiple lines:

```r
plot(pressure$temperature, pressure$pressure, type = "l")
points(pressure$temperature, pressure$pressure)

lines(pressure$temperature, pressure$pressure/2, col = "red")
points(pressure$temperature, pressure$pressure/2, col = "red")
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524182421273.png)

With `ggplot2`:

```r
library(ggplot2)

ggplot(pressure, aes(x = temperature, y = pressure)) +
  geom_line()
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524182542793.png)

With points added:

```r
ggplot(pressure, aes(x = temperature, y = pressure)) +
+   geom_line() +
+   geom_point()
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524182758352.png)

## Bar Graph

Take a look at the `BOD` data:

```r
BOD
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524182950290.png)

```r
barplot(BOD$demand, names.arg=BOD$Time)
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524183155294.png)

To generate the count of each unique value in a vector, use the `table()` function:

```r
table(mtcars$cyl)
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524183837657.png)

Then pass the table to `barplot()` to generate the graph of counts:

```r
barplot(table(mtcars$cyl))
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524213337924.png)

With `ggplot2`:

```r
library(ggplot2)

# Bar graph of values. This uses the BOD data frame, with the
# "Time" column for x values and the "demand" column for y values.
ggplot(BOD, aes(x = Time, y = demand)) +
  geom_col()

# Convert the x variable to a factor, so that it is treated as discrete
ggplot(BOD, aes(x = factor(Time), y = demand)) +
  geom_col()
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524213538683.png)

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524213608637.png)

Also with `geom_bar()`:

```r
# Bar graph of counts This uses the mtcars data frame, with the "cyl" column for
# x position. The y position is calculated by counting the number of rows for
# each value of cyl.
ggplot(mtcars, aes(x = cyl)) +
  geom_bar()

# Bar graph of counts
ggplot(mtcars, aes(x = factor(cyl))) +
  geom_bar()
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524214550959.png)

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524214616145.png)

> **Note**
>
> In previous versions of ggplot2, the recommended way to create a bar graph of values was to use `geom_bar(stat = "identity")`. As of ggplot2 2.2.0, there is a `geom_col()` function which does the same thing.

## Histogram

```r
hist(mtcars$mpg)

# Specify approximate number of bins with breaks
hist(mtcars$mpg, breaks = 10)
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524214806679.png)

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524214746588.png)

> When you create a histogram without specifying the bin width, `ggplot()` prints out a message telling you that it’s defaulting to 30 bins, and to pick a better bin width. This is because it’s important to explore your data using different bin widths; the default of 30 may or may not show you something useful about your data.

## Box Plot

```r
plot(ToothGrowth$supp, ToothGrowth$len)
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524215518618.png)

If the two vectors are in the same data frame, you can also use the `boxplot()` function with formula syntax. With this syntax, you can combine two variables on the x-axis:

```r
# Formula syntax
boxplot(len ~ supp, data = ToothGrowth)

# Put interaction of two variables on x-axis
boxplot(len ~ supp + dose, data = ToothGrowth)
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524215633010.png)

With `ggplot2`:

```r
library(ggplot2)
ggplot(ToothGrowth, aes(x = supp, y = len)) +
  geom_boxplot()
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524215809150.png)

box plots for multiple variables (combine the variables with `interaction()`) :

```r
ggplot(ToothGrowth, aes(x = interaction(supp, dose), y = len)) +
  geom_boxplot()
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524215905083.png)

## Function Curve

```r
curve(x^3 - 5*x, from = -4, to = 4)
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524215956918.png)

function that takes a numeric vector as input and returns a numeric vector (using `add = TRUE` will add a curve to the previously created plot) :

```r
# Plot a user-defined function
myfun <- function(xvar) {
  1 / (1 + exp(-xvar + 10))
}
curve(myfun(x), from = 0, to = 20)
# Add a line:
curve(1 - myfun(x), add = TRUE, col = "red")
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524220110992.png)

With `ggplot2` (by using `stat_function(geom = "line")` and passing it a function that takes a numeric vector as input and returns a numeric vector) :

```r
library(ggplot2)
# This sets the x range from 0 to 20
ggplot(data.frame(x = c(0, 20)), aes(x = x)) +
  stat_function(fun = myfun, geom = "line")
```

![](https://lai-s-image-host.oss-cn-beijing.aliyuncs.com/img/20250524220145194.png)
