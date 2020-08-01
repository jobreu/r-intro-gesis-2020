---
title: "Introduction to R for Data Analysis"
subtitle: "Data Visualization Part 1"
author: "Johannes Breuer<br />Stefan Jünger"
date: "2020-08-05"
location: "GESIS, Cologne, Germany"
output:
  xaringan::moon_reader:
    lib_dir: libs
    css: ["default", "default-fonts", "../workshop.css"]
    nature:
      highlightStyle: "github"
      highlightLines: true
      countIncrementalSlides: false
editor_options: 
  chunk_output_type: console
---
layout: true



<div class="my-footer">
<div style="float: left;"><span></span></div>
<div style="float: right;"><span>, </span></div>
<div style="text-align: center;"><span></span></div>
</div>



---

## Generally, ...
- Graphical data analysis is great
- Good plots can contribute to a better understanding
- Generating a plot is easy
- Making a good plot can take very long
- Generating plots with R is fun
- Plots created with R have high quality
- Almost every plot type is supported by R
- A large number of export formats are available in R

---

## Content of the visualization sessions
.pull-left[
**Base R visualization**
- Standard plotting procedures in R
- scatterplots, lineplots, histograms and the like
]

.pull-right[
**Tidyverse/ggplot2 visualization**
- Modern interface to graphics
- grammar of graphics
]

There's more that we won't cover:
- lattice plots, for example

---

## Graphics in R
Since the early days, graphics are a first class citizen in R

A standard `R` installation doesn't require any additional packages to create graphics. It's part of the `graphics` package.




.pull-left[

```r
hist(gp_covid$hzcy001a)
```
]

.pull-right[
<img src="figure/unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />
]

---

## Ok, but let's start from the beginning
The most basic function to plot in R is `plot()`.

.pull-left[

```r
plot(gp_covid$hzcy001a)
```
]

.pull-right[
<img src="figure/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />
]

---

## We can turn this into a bivariate scatterplot
.pull-left[

```r
plot(
  gp_covid$age_cat, 
  gp_covid$hzcy001a
)
```
]

.pull-right[
<img src="figure/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />
]


---

## Adding stuff to the plot: titles & labels
.pull-left[

```r
plot(
  gp_covid$age_cat, 
  gp_covid$hzcy001a,
  main = "Relationship between Age and Subjective Likelihood of a COVID-19 Infection",
  xlab = "Age of Respondents",
  ylab = "Likelihood of Being Infected"
)
```
]

.pull-right[
<img src="figure/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />
]

---

## Adding stuff to the plot: axis labels
.tinyish[
.pull-left[

```r
plot(
  gp_covid$age_cat, 
  gp_covid$hzcy001a,
  main = "Relationship between Age and Likelihood of Covid-19 Infection",
  xlab = "Age of Respondents",
  ylab = "Likelihood of Being Infected"
)
axis(
  side = 2, 
  at = 1:7,
  labels = c(
    "Not at all",
    "Very\nunlikely", 
    "Rather\nunlikely",
    "Moderately",
    "Rather", 
    "Very",
    "Absolutely"
  ),
  las = 0
)
```
]
]

.pull-right[
<img src="figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />
]

---

## Annotating the plot: Adding a regression line
.tinyish[
.pull-left[

```r
plot(
  gp_covid$age_cat, gp_covid$hzcy001a,
  main = "Relationship between Age and Likelihood of Covid-19 Infection",
  xlab = "Age of Respondents",
  ylab = "Likelihood of Being Infected"
)
axis(
  side = 2, 
  at = 1:7,
  labels = c(
    "Not at all",
    "Very\nunlikely", 
    "Rather\nunlikely",
    "Moderately",
    "Rather", 
    "Very",
    "Absolutely"
  ),
  las = 0
)
abline(
  lm(
    hzcy001a ~ age_cat, 
    data = gp_covid
  )
)
```
]
]

.pull-right[
<img src="figure/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />
]

---

## More options and more plots
We will again turn to this example later in this week during the confirmatory analysis session. What you may have learned so far is that already with a few options you can dramatically change the output of the default plotting function in `R`.

We could now also adjust some more parameters, such as colors, axis limits etc., but this may create an example that is somewhat far-fetched. Instead, we will learn from other standard plotting functions in `R`, and will apply more options there.

---

## Histograms
Although we will turn to exploratory data analysis tomorrow, histograms and similar plots are really handy if you want to have a glimpse on the distribution of a variable. See, the distribution of the categorized age variable in the GESIS Panel:


```r
hist(gp_covid$age_cat)
```

<img src="figure/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

---

## Introduce axis limits
You can then pose axis limits using the option `xlim`, if you are only interested in the range of data.


```r
hist(gp_covid$age_cat, xlim = c(1, 5))
```

<img src="figure/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />

---
## Adding breaks
The `breaks` option is great to limit or expand the number of bins and their width in the data.


```r
hist(gp_covid$age_cat, breaks = 10)
```

<img src="figure/unnamed-chunk-10-1.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" style="display: block; margin: auto;" />

However, age in our data are categorical data and, therefore, we may want to use a plotting technque that is more approriate: barplots.

---

## Tabulate and `barplot`
The command `barplot()` generates a barplot from a frequency table. So we need to build it first. Fortunately, that's really easy and something you will need all the time. For example, to create a frequency table for the categorized age variable in the GESIS Panel use:


```r
table_age <- table(gp_covid$age_cat)
```

We can then create our barplot from that


```r
barplot(table_age)
```

<img src="figure/unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" style="display: block; margin: auto;" />

---

## Make it colorful: adding blue colors
Everything's so far was more or less grayish, so we use the example of barplots to first create blue bars for our barplot. For this purpose, we use the `rgb()` function. You could also add hex codes as character strings (see below).


```r
barplot(table_age, col = rgb(0,0,1))
```

<img src="figure/unnamed-chunk-13-1.png" title="plot of chunk unnamed-chunk-13" alt="plot of chunk unnamed-chunk-13" style="display: block; margin: auto;" />

---

## Green colour 
Here's a green version...


```r
barplot(table_age, col = rgb(0,1,0))
```

<img src="figure/unnamed-chunk-14-1.png" title="plot of chunk unnamed-chunk-14" alt="plot of chunk unnamed-chunk-14" style="display: block; margin: auto;" />

---

## Red colour
...and a red one.


```r
barplot(table_age, col = rgb(1,0,0))
```

<img src="figure/unnamed-chunk-15-1.png" title="plot of chunk unnamed-chunk-15" alt="plot of chunk unnamed-chunk-15" style="display: block; margin: auto;" />

---

## Transparency (alpha value)
Granted, this looks a little bit oldschool. What boosts the appearance a little bit are alpha-values. They make the colors transparent to a certain degree.


```r
barplot(table_age, col = rgb(1, 0, 0, .3))
```

<img src="figure/unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" style="display: block; margin: auto;" />

---

## Rstudio addin `colourpicker`
Choosing colors is not always an easy task. The `colourpicker` adding for `Rstudio` helps you choosing colors.


```r
install.packages("colourpicker")
```

![](figure/colourpicker.PNG){height=80%}

---

## Set various colors


```r
barplot(table_age, col = c(20, "#62D6C8", "darkorange"))
```

<img src="figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />

---

## A two dimensional table


```r
table_age_likelihood <- table(gp_covid$age_cat, gp_covid$hzcy001a)

table_age_likelihood
```

```
##     
##        1   2   3   4   5   6   7
##   1    1   2  15  22  20   8   4
##   2    2  11  23  49  71  35   4
##   3    1  12  25  61  56  43  10
##   4    4  15  31  94  78  36   9
##   5    4  17  39  74  78  37  11
##   6    5  16  51 102  78  47   9
##   7   26  55 155 305 186  83  32
##   8   12  29  79 137  61  21   3
##   9    3  37  79 133  40  17   2
##   10  16  52 124 113  29  11   2
```

If the passed table object is two-dimensional, a conditional barplot is created.

---

## Conditional `barplot`
The barplot for a two-dimensional table object can either be stackend (default).


```r
barplot(table_age_likelihood)
```

<img src="figure/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" style="display: block; margin: auto;" />

Or it can be plotted side by side.


```r
barplot(table_age_likelihood, beside = TRUE)
```

<img src="figure/unnamed-chunk-21-1.png" title="plot of chunk unnamed-chunk-21" alt="plot of chunk unnamed-chunk-21" style="display: block; margin: auto;" />

---

## More plot types you can choose from
Boxplots
- `boxplot(y ~ x)`

Pie charts (still possible in 2020)
- `pie(x)`

Scatterplot matrices
- `pairs(x)`

Mosaic Plots
- `mosaicplot(table)`

Densities
- `plot(density(x))`

---

## Integrating plots in functions/packages
Creating plots in R is not complicated. Since the real advantage of R is the
community with additional packages and routines, it's clear that the real benefit
is this accessible interface to plotting techniques.

This is why `plot()` often is a generic function not only to plot data directly. Some statistical models have their own plotting method, which is called when you use the `plot()` command.

---

## Example: OLS model
.pull-left[

```r
linear_model <- 
  lm(
    hzcy001a ~ age_cat, 
    data = gp_covid
  )

par(mfrow = c(2, 2))
plot(linear_model)
```
]

.pull-right[
<img src="figure/unnamed-chunk-22-1.png" title="plot of chunk unnamed-chunk-22" alt="plot of chunk unnamed-chunk-22" style="display: block; margin: auto;" />
]

---

## Added-value plotting methods in other packages
What is more is than other package developer provide new R functions that, in the background, make use of R's plotting techniques. These packages rescue you from writing lengthy R-scipts just to get a specific layout of a plot.

---

## A correlation plot
.pull-left[

```r
library(corrplot, quietly = TRUE)

correlations <-
  gp_covid %>% 
  dplyr::select(
    age_cat, 
    hzcy001a, 
    political_orientation, 
    education_cat
  ) %>% 
  cor(use = "complete.obs", method = "spearman")

corrplot(correlations, method = "color")
```
]

.pull-right[

```
## corrplot 0.84 loaded
```

<img src="figure/unnamed-chunk-23-1.png" title="plot of chunk unnamed-chunk-23" alt="plot of chunk unnamed-chunk-23" style="display: block; margin: auto;" />
]

---

## Fancy panels
.pull-left[

```r
library(psych, quietly = TRUE)

gp_covid %>% 
  dplyr::select(
    age_cat, 
    hzcy001a, 
    political_orientation, 
    education_cat
  ) %>% 
  pairs.panels()
```
]

.pull-right[

```
## 
## Attaching package: 'psych'
```

```
## The following objects are masked from 'package:ggplot2':
## 
##     %+%, alpha
```

<img src="figure/unnamed-chunk-24-1.png" title="plot of chunk unnamed-chunk-24" alt="plot of chunk unnamed-chunk-24" style="display: block; margin: auto;" />
]

---

## But sometimes that's not enough
In a way, read-made functions for nice plots requires again to learn new commands,
options, and the like. And sometimes you may have requirements that cannot even 
fulfilled with these functions anyway.

While these functions are helpful to combine information from different variables
in a dataset, sometimes you also want to re-create the same old boring graph
over and over again, e.g., for plotting results from similar but different 
statistical models.

So may want to exploit your skills on looping.

---

## Let's create some analysis results


```r
dependent_variables <-
  c("hzcy001a", "hzcy002a", "hzcy003a", "hzcy004a", "hzcy005a")

linear_models <-
  lapply(dependent_variables, function (dv) {
    lm(gp_covid[[dv]] ~ age_cat, data = gp_covid)
  }) %>% 
  magrittr::set_names(dependent_variables)
```

---

## The plot that we want to re-create multiple times
.pull-left[

```r
library(margins)
invisible(
  capture.output(
    cplot(
      linear_models[["hzcy001a"]], 
      main = "hzcy001a"
    )
  )
)
```
]

.pull-right[
<img src="figure/unnamed-chunk-26-1.png" title="plot of chunk unnamed-chunk-26" alt="plot of chunk unnamed-chunk-26" style="display: block; margin: auto;" />
]

---

## Let's write a function

```r
fancy_pants_margins_plot <- function (my_models) {
  
  # define layout
  par(mfrow = c(ceiling(length(my_models)/2), 2))
  
  # plot all models
  for (model in names(my_models)) {
    invisible(
      capture.output(
        cplot(
          my_models[[model]],
          main = model
          
          # add plot parameter(s) whichever you want
          # ...
          
        )
      )
    )
  }
}
```

---

## Run our function
.pull-left[

```r
fancy_pants_margins_plot(linear_models)
```
]

.pull-right[
<img src="figure/unnamed-chunk-28-1.png" title="plot of chunk unnamed-chunk-28" alt="plot of chunk unnamed-chunk-28" style="display: block; margin: auto;" />
]

---

## Outlook on the next session
In the next session, we will work with the **tidyverse** and its plotting techniques using `ggplot2`. We will learn an alternative  and easier way of plotting data or results from different models exploiting so-called facets.  However, the trade-off is that the input data for the plots require more attention and simply more preparation.

---

## Exporting graphics
It's nice that R provides such nice plotting opportunities.

However, to include them in our papers, we need to export them.

As said in the beginning, numerous exports formats are baked-in into R.

---

## Export with Rstudio

<img src="./pics/saveGraphic.PNG" title="plot of chunk unnamed-chunk-29" alt="plot of chunk unnamed-chunk-29" style="display: block; margin: auto;" />

---

## Command to save graphic

- Alternatively also with the commands `png`, `pdf` or `jpeg` for example


```r
png("Histogramm.png")
hist(dat$duration)
dev.off()
```


```r
pdf("Histogramm.pdf")
hist(dat$duration)
dev.off()
```


```r
jpeg("Histogramm.jpeg")
hist(dat$duration)
dev.off()
```

---

## My personal note on R base plotting
Hopefully, you may have gotten the feeling that already R's base techniques for plotting are well-suited for daily data exploration. On some days, `hist()` is my most often used command.

But to be honest: I use all the other functions not that often, particularly from all the added-value packages for plotting. The syntax is sometimes cumbersome with all the `par()`  or `dev.off()` calls, and manipulating parameters simply feels 'old'.

In the next session, we will learn more modern techniques using **tidyverse**/`ggplot2`. Yet, we still believe that it is worthwhile to get comfortable with base R plotting since `ggplot2` sometimes can simply too much for simple data exploration.


---


