# DT::datatable in a for loop

2022/10/07

Mira Sohn

## Motivation

I love to use [`DT::datatable`](https://rstudio.github.io/DT/) in my data analysis in R. It's especially practical when printing tables by providing a wide variety of functionality such as interactive column arrangement, search bar, entry adjustment, and many more. It allows me to explore my RNA-seq data table by arranging differentially expressed genes by the size of log2FoldChange or false discovery rate. On the other hand, it was pretty much painful when implementing in a for loop. A few days ago, I learned an extremely useful trick which enables `DT::datatable` to
