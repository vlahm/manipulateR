# **manipulateR**

### Tools for streamlining the general data processing pipeline
### **Description**
Some data manipulation tasks are just easy enough that we never get around to automating them. We just perform them over and over, making mental notes that it would be nice to write functions for them one day. Here are those functions.

### **Contents**
1. _matfilter_ for subsetting matrices and data.frames by proportion of column/row elements meeting a condition
2. _ultimate_reader_ for loading and merging multiple files
3. _rowcol_inserter_ for... doing exactly what you'd expect
4. _excel_to_r_ for pasting spreadsheet values as comma-separated strings (for vector definition)
5. _comma_blaster_ for doing the same as #4 with R output
6.  Installation
7.  Contact the author

---
### **1. matfilter**
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
---
#### **2. ultimate_reader**
#### Read and merge multiple files
+ `dir_args` a `list` of args passed to `dir`, which selects the files to be read/merged.
+ `read_args` a `list` of args passed to `read_table`, which by default is set up to read .csv files.
```R
ultimate_reader(dir_args=list(path='./',pattern='.csv'),
    read_args=list(sep=',', quote="\"", header=TRUE,
    fill=TRUE, comment.char=""), 
    merge=FALSE, ...)
```
#### **_Example Usage_**
```R
ultimate_reader(dir_args=list(path='~/project/data', pattern='\w{3}_2016.csv'))
ultimate_reader(merge=TRUE, by='data', all=TRUE)
```
---
### **3. rowcol_inserter**
#### Insert a row or column into a matrix or data.frame
```R
rowcol_inserter(x, vec, pos, name=pos, dim='row')
```
+ `x` = data.frame or matrix
+ `vec` = [vector] Row/column to be inserted
+ `pos` = [numeric] Position where row/column will be inserted
+ `name` = [character] Name of the row/column
+ `dim` = [character] Should the vector be inserted as a row ('row') or column ('col')?

#### **_Example Usage_**
```R
#example dataframe
x <- data.frame('col1'=1:10, 'col2'=rnorm(10), 'col3'=letters[1:10],
    stringsAsFactors=FALSE)

#insert new row at position 8
rowcol_inserter(x, c(99,100,'new'), 8)

#insert new column at position 3
rowcol_inserter(x, 101:110, 3, name='newcol', dim='col')
```
---
### **4. excel_to_r**
#### Paste values from spreadsheets into R in vector-ready format
+ **Call the function, paste values from Excel, LibreOffice Calc, etc., hit Enter.**
+ **The values will be printed as a comma-separated character string that can now be copied and used to define a vector.**
```R
excel_to_r(char_out=FALSE, spaces=TRUE, ...)
```
+ `char_out` = [logical] Should the output be printed with quotes around each vector element?
+ `spaces` = [logical] Should each comma be followed by a space (just for looks)?
+ `...` = additional arguments to 'scan'

#### **_Example Usage_**
```R
> excel_to_r(spaces=FALSE)
Paste Excel values into console, then hit ENTER
1: 0    3   3.5 2.5 3   4   2.5 0
9: 
Read 8 items
[1] "0,3,3.5,2.5,3,4,2.5,0"
newvec <- c(0,3,3.5,2.5,3,4,2.5,0)

> excel_to_r(char_out=TRUE, allowEscapes=FALSE)
Paste Excel values into console, then hit ENTER
1: 4    6   3.2 \n
5: 
Read 4 items
[1] "'4', '6', '3.2', '\\n'"
```
---
### **5. comma_blaster**
#### Format R console printouts for definition of new vectors
+ **Sometimes faster than figuring out how to extract data from complex objects**
+ **Often useful during exploratory phase of analysis**
```R
comma_blaster(x, sep=" ", char_out=FALSE, spaces=TRUE)
```
+ `x` = [string] A character string to be comma-separated
+ `sep` = [string] Element in the input string to be replaced with commas.  Defaults to " ".
+ `char_out` = [logical] Should the output be printed with quotes around each vector element?
+ `spaces` = [logical] Should each comma be followed by a space (just for looks)?

#### **_Example Usage_**
```R
> comma_blaster(month.abb[1:5], char_out=TRUE)
[1] "'Jan', 'Feb', 'Mar', 'Apr', 'May'"
> newvec <- c('Jan', 'Feb', 'Mar', 'Apr', 'May')
> 
> comma_blaster("1-800-555-0100", sep='-')
[1] "1, 800, 555, 0100"
```
---
### **6. Installation**
\# in R:
install.packages('devtools')
library(devtools)

install_github('vlahm/manipulateR')
library(manipulateR)
---
### **7. Contact the author**
Mike Vlah
vlahm13@gmail[dot]com


