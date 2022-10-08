# DT::datatable in a for loop

2022/10/07

Mira Sohn

## Motivation

I love to use [`DT::datatable`](https://rstudio.github.io/DT/) in my data analysis in R. It's especially practical when printing tables in rmarkdown (Rmd) by providing a wide variety of functionality such as interactive column arrangement, search bar, entry adjustment, and many more. It allows me to explore my RNA-seq data table by arranging differentially expressed genes by the size of log2FoldChange or false discovery rate. On the other hand, it was pretty much painful when implementing in a for loop. 

In this demo, I'd like to share a simple trick to implement `DT::datatable` in a for loop which I learned recently. My demo is super simple but you will find more useful applications [here](http://michaeljw.com/blog/post/subchunkify/). I also recomment to visit [this thread](https://stackoverflow.com/questions/39732560/why-does-datatable-not-print-when-looping-in-rmarkdown).


## Requirements

My demo requires the R package `DT`. More info bout the package is found here:

- GitHub: https://github.com/rstudio/DT
- Documentation: https://rstudio.github.io/DT/

