#### **matfilter**
#### Remove rows or columns according to the proportion of elements that meet a condition
```R
matfilter(x, mar=2, cond=NA, fun=NULL, thresh=1, 
          na.rm=TRUE, filter=TRUE)
```
+ `x` a matrix or data.frame
+ `mar` [numeric] the margin along which the function will be applied - either 1 for rows or 2 for columns
+ `cond` [character] the completion of a Boolean expression, e.g. ">= 0", which is passed to `apply`
+ `fun` the name of a function that returns a logical, i.e. `is.numeric`
+ `thresh` [numeric] a value between 0 and 1. Only rows/columns with `<=` this proportion of elements satisfying `cond` or `fun` will be returned if `filter == TRUE`.
+ `na.rm` [logical] if `FALSE`, rows/columns containing `NA` values will be evaluated to `NA` if using `fun`.
+ `filter` [logical] if `TRUE`, the filtered matrix or data.frame will be returned.  If `FALSE`, only the proportions of values in each column satisfying `cond` or `fun` will be returned.


#### **_Example Usage_**
```R
> df <- data.frame(a=c(2,2,3,3), b=c(1,2,NA,1), c=c(2,NA,NA,3))
> df
  a  b  c
1 2  1  2
2 2  2 NA
3 3 NA NA
4 3  1  3

> matfilter(df, mar=2, fun=is.na, thresh=0.3)
$column_proportions
   a    b    c 
0.00 0.25 0.50 

  a  b
1 2  1
2 2  2
3 3 NA
4 3  1

> matfilter(df, mar=1, cond='== 2', thresh=0.5)
$row_proportions
[1] 0.6666667 0.6666667 0.0000000 0.0000000

  a  b  c
3 3 NA NA
4 3  1  3
```


#### **ultimate_reader**
#### Read and merge multiple files
+ `dir_args` a `list` of args passed to `dir`, which selects the files to be read/merged.
+ `read_args` a `list` of args passed to `read_table`, which by default is set up to read .csv files.
+ `merge` determines whether to merge all files and output the merged `data.frame`(`TRUE`) or leave them separate and add them to the global environment (`FALSE`)
ultimate_reader(dir_args=list(path='./', pattern='.csv'),
                              read_args=list(sep=',', quote="\"", header=TRUE, 
                                             fill=TRUE, comment.char=""),
                              merge=FALSE, ...)
