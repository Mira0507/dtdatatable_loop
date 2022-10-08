# DT::datatable in a for loop

2022/10/07

Mira Sohn

## Motivation

I love to use [`DT::datatable`](https://rstudio.github.io/DT/) in my data analysis using rmarkdown (Rmd). It's especially practical when printing tables by providing a wide variety of functionality such as interactive column arrangement, search bar, entry adjustment, and more. It allows me to explore my RNA-seq data table by arranging differentially expressed genes by the size of log2FoldChange or false discovery rate. On the other hand, it was pretty much painful when implementing in a for loop. 

In this demo, I'll be sharing a simple trick to implement `DT::datatable` in a for loop which I learned recently. My demo is super simple but you will find more useful applications [here](http://michaeljw.com/blog/post/subchunkify/). I also recomment to visit [this thread](https://stackoverflow.com/questions/39732560/why-does-datatable-not-print-when-looping-in-rmarkdown).


## Requirement

My demo requires the R package `DT`. More info bout the package is found here:

- GitHub: https://github.com/rstudio/DT
- Documentation: https://rstudio.github.io/DT/


## Demo

### Setting chunk options

Chunk options are set to avoid printing unnecessary messages when rendering rmarkdown. More info about available chunk options is given [here](https://bookdown.org/yihui/rmarkdown-cookbook/chunk-options.html).


```r

knitr::opts_chunk$set(warning=FALSE,
                      message=FALSE)

```




### Loading `DT`

The R package `DT` is loaded using the function `library`.

```r
library(DT)
```

### Input list

I prepared a list which consists of two data frames for this demo.


```r
res.list <- readRDS("reslist.rds")
names(res.list) <- c("dataset1", "dataset2")

```



```r
# > lapply(res.list, head)
# $dataset1
          # logFC   logCPM         F    PValue       FDR
# 3713  0.8356375 8.568426 0.6551161 0.4183335 0.9985701
# 4237 -0.6125029 8.698834 0.4496273 0.5025474 0.9985701
# 559   0.8195851 8.762568 0.8161195 0.3663669 0.9985701
# 2185  0.5064151 8.630167 0.3023414 0.5824463 0.9985701
# 3879  1.3974391 8.552710 1.7199910 0.1897630 0.9985701
# 2587 -0.8452143 8.754667 0.8121409 0.3675376 0.9985701
# $dataset2
           # logFC   logCPM           F     PValue       FDR
# 1657  2.03334676 8.544415 4.851563327 0.05330251 0.9636926
# 2112 -0.64937221 8.693690 0.638405188 0.44361494 0.9636926
# 2470 -0.01181960 8.627389 0.000167514 0.98992395 0.9996974
# 3493 -0.03265589 8.471250 0.001013631 0.97525400 0.9996974
# 1164  0.53423700 8.471309 0.309831002 0.59023387 0.9636926
# 2862 -0.01096336 8.627729 0.000151129 0.99044330 0.9996974

```

