# `DT::datatable` in a for loop

2022/10/08

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

A list of data frames will be used in this demp. Ensure that each data frame is named. The names `dataset1` and `dataset2` will be used both in printing the tables and naming subchunks.


```r
# Import input list
res.list <- readRDS("reslist.rds")

# Name each data frame
# (this step can be skipped if they are already properly named)
names(res.list) <- c("dataset1", "dataset2")

# Explore the input list
lapply(res.list, head)
```


```r
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

### Setting up a function creating subchunks

The key idea of this demo is using subchunks. I will set up a simpler version of `subchunkify` introduced [here](http://michaeljw.com/blog/post/subchunkify/). You can also take advantage of it in many other applications such as rendering multiple plot formats in a single chunk.


```r

# Define a function generating subchunks with DT::datatable(dataframe) each
subchunkify <- function(name) {
    t_deparsed <- paste0("DT::datatable(res.list[[",
                         deparse(substitute(name)),
                         "]] %>% dplyr::arrange(logFC))")

    sub_chunk <- paste0("```{r sub_chunk_", name, ", results='asis', echo=FALSE}",
        "\n",
        t_deparsed,
        "\n\n```\n\n\n")

    cat(knitr::knit(text = sub_chunk, quiet=TRUE))
}


```

While looping, the argument `name` will be set to the `dataset1` and `dataset2`. Calling `cat(knitr::knit(text = sub_chunk, quiet=TRUE))` will compile the subchunks.


### Printing tables

Once the function `subchunkify` is defined, just calling the function `subchunkify()` in a for loop will take care of everything.

```r
# Create tables in each tab
cat('## Table {.tabset}\n\n')
for (name in names(res.list)) {
    cat('###', name, '\n\n')
    subchunkify(name)
    cat('\n\n')
}
```

Running above six lines generate two subchunks as shown below.


```r

# ## Table {.tabset}
#
#
# ### dataset1
#
# 
# ```{r sub_chunk_dataset1, results='asis', echo=FALSE}
#
# DT::datatable(res.list[[dataset1]] %>% dplyr::arrange(logFC))
# 
#
# ```
# 
#
# 
# ## dataset2
# 
#
# ```{r sub_chunk_dataset2, results='asis', echo=FALSE}
#
# DT::datatable(res.list[[dataset2]] %>% dplyr::arrange(logFC))
# 
#
# ```

```


After rendering, ![dataset1](https://github.com/Mira0507/dtdatatable_loop/blob/master/images/table1.png) and ![dataset2](https://github.com/Mira0507/dtdatatable_loop/blob/master/images/table2.png)



