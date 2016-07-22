# R-notes

##General
- attach(x) can be used to avoid constatly specifing data frame name, for example:
     - attach(df) / df[column_name==x,] instead of df[df$column_name==x,]
- as.yearmon(x) from the 'zoo' package can be used to extract date and month only from the string;
- paste(x,y,z) is used to concatenate strings together;
- str() and summary() are used for fast data exploration, glimpse() is equivalent to str();
- when changing values in the column the desired mapping could be done using named vector like x=c('a'='A', 'b'='B',...), then values can be the assigned by df$column_name_1=x[df$column_name_2]
- rank() takes a group of values and calculates the rank of each value within the group ex. rank(c(21, 22, 24, 23)) returns [1] 1 2 4 3;
- prop.table(table(train$Survived)) <- gives table with proportions, you have to specify whether you want row-wise or column-wise proportions. This is done by setting the second argument of prop.table(), called margin, to 1 or 2, respectively;
- to merge data frames merge() is used, you can use by=c("c1_name","c2_name",...) when column names are the same in both dfs or by.x=c(...) and by.y=c(...), different types of join can by achieved by using all.x and all.y parameter as follows
     - inner join: merge(df1, df2);
     - outer join: merge(x = df1, y = df2, by = "CustomerId", all = TRUE)
     - left outer join: merge(x = df1, y = df2, by = "CustomerId", all.x = TRUE)
     - right outer join: merge(x = df1, y = df2, by = "CustomerId", all.y = TRUE)
     - cross join: merge(x = df1, y = df2, by = NULL)
- data frames can be converted into table using tbl_df(), which makes it easier to work with, however table has the same properties as data frame so it can be manipulated the same way as data frame;
- paste0("x", 1:5) generates x1,x2 up to x5;
- sample(1:100, 3, replace=FALSE) return given number of integers (3) between 1:100, without replacement;
- rnorm(4) returns 4 numbers from normal distribution, default mean=0 and sd=1, it can be specified by rnorm(4, mean=50, sd=10);
- runif(1) return number from uniform distribution;
- seq_along() is useful for creating interations for the loop, it handles empty object better (ex. column in data frame);
- to create empty data frame use df=data.frame("column_name_1"=numeric(),"column_name_1"=character()), then to add row: rbind(df, data.frame("a"=c(1,2),"b"=c("ala","kot")));
- when subsetting a list, list[1] returns first element as a list, however list[[1]] returns first element; 
- lubridate package is useful for working with dates, you can convert string with date using y,m,d,h,m,s set by for example ymd_hms("date"), or separately mdy("date");

##Dplyr
- there are five functions in dplyr that are called verbs
     - select(), which returns a subset of the columns,
     - filter(), that is able to return a subset of the rows,
     - arrange(), that reorders the rows according to single or multiple variables,
     - mutate(), used to add columns from existing data,
     - summarise(), which reduces each group to a single row by calculating aggregate measures;
- in the select statement both column names and indexes can be used, ex. select(df, 1:4, -2), -2 means that we don't want to display second column;
- dplyr comes with a set of helper functions that can help you select groups of variables inside a select() call, these are:
     - starts_with("X"): every name that starts with "X",
     - ends_with("X"): every name that ends with "X",
     - contains("X"): every name that contains "X",
     - matches("X"): every name that matches "X", where "X" can be a regular expression,
     - num_range("x", 1:5): the variables named x01, x02, x03, x04 and x05,
     - one_of(x): every name that appears in x, which should be a character vector.
- when you refer to columns directly inside select(), you don't use quotes. If you use the helper functions, you do use quotes;
- mutate syntax is as follows mutate(my_df, x = a + b, y = x + c);
- filter allows to use x %in% c(a, b, c) syntax;
- filter(df, a > 0 & b > 0) and filter(df, a > 0, b > 0) are equivalent;
- use filter(df, !is.na(x)) to filter out NA's;
- summarise functions include
     - min(x) - minimum value of vector x.
     - max(x) - maximum value of vector x.
     - mean(x) - mean value of vector x.
     - median(x) - median value of vector x.
     - quantile(x, p) - pth quantile of vector x.
     - sd(x) - standard deviation of vector x.
     - var(x) - variance of vector x.
     - IQR(x) - Inter Quartile Range (IQR) of vector x.
     - diff(range(x)) - total range of vector x.
     - first(x) - The first element of vector x.
     - last(x) - The last element of vector x.
     - nth(x, n) - The nth element of vector x.
     - n() - The number of rows in the data.frame or group of observations that summarise() describes.
     - n_distinct(x) - The number of unique values in vector x.
      
##googleVis
- provides an interface between R and the Google Chart Tools;
- gvisMotionChart() is used to create motion chart, in its simplest form it takes 3 arguments: data frame, subject to be analysed, and the time dimension data; 
- additional arguments include: 
     - xvar: Here you place the column name of the variable to be plotted on the x-axis
     - yvar: Here you place the column name of the variable to be plotted on the y-axis
     - sizevar: Here you provide the column name that will make the bubbles change size.

##Data.table
- the basic syntax of working with data.table looks as follows DT[i,j,by=], where: 
     - i is for rows selection; 
     - j is for column selection and also works similiar to the ddplyr's mutate, so you can use functions like sum(), mean() etc. there; 
     - in by you specify columns the output should be grouped by;
- data.table() is used to create data tabe;
- when selecting rows one do not need to use comma, e.x. DT[2:3] selects second and third row;
- .N stores number of rows;
- When you use .() in j, the result is always a data.table. But data.table also provides the option to return a vector while computing on just a single column and not wrapping it with .(), for convenience;
- .SD can be used in j to do an operations on each column except the column named in by, ex. DT[,(.SD, sum),by=x] (you can bypass that by explicitly naming by column in .SDcols), to select fewer number of columns you can use .SDcols argument, after by ex. DT[,lapply(.SD, sum),,.SDcols=2:4];
- DT[[3]] chooses first column as a vector;
- := is defined for use in j only, and there are two ways of using it. The first is the LHS := RHS form, where LHS is a character vector of column names and RHS is the corresponding value, the second one is functional form ':=' that could be used as follows DT[,':='(B=B+1, C=A+B, D=2];
- DT[,Total:=sum(B), by=A] creates column "Total" equal to the sum of column B grouped by A;
- DT[,Total:=NULL,] deletes column "Total";
- when using := in j do you don't need to assign the result to DT, i.e. the DT <- part is superfluous;
- set(x, i=NULL, j, value) is loopable function for changing values in a data.table, ex. for(j in 2:4){ set(DT, sample(1:10,3, replace=FALSE), j, NA)}
- setnames(x,old,new) is used to changing column names in a data.table;
- setcolorder(x, neworder) changes column order;
- DT[,A=="a"] returns vector, but DT[,.(A=="a")] returns data.table;
- you can manually set a key via setkey(DT,A,B). setkey() will then sort the data by the columns that you specify, and change the table by reference. Having set a key will allow you to use it as a super-charged rowname when doing selections for example. Arguments like mult and nomatch then help you to only select particular parts of groups;
-by = .EACHI allows you to run j for each group in which each item in i joins too;
- to filter by numeric key use J(x);

#System operations
- when specyfing path use "/" instead of "\";
- setwd() sets working directory;
- dir() list files in the specified directory;
- dir.create() creates last folder in the path, ex. dir.create("C:/library/test"), creates "test" in "library" folder;
- dir.exists() checks if given path exists;

#Purrr Package
- all the map functions in purrr take a vector, .x, as the first argument, then return .f applied to each element of .x. The type of object that is returned is determined by function suffix (the part after _):
          - map() returns a list or data frame
          - map_lgl() returns a logical vector
          - map_int() returns a integer vector
          - map_dbl() returns a double vector
          - map_chr() returns a character vector
- The map functions use the ... ("dot dot dot") argument to pass along additional arguments to .f each time it’s called. For example, we can pass a trim argument to the mean() function: map_dbl(df, mean, trim = 0.5). Multiple arguments can be passed along using commas to separate them. For example, we can also pass the na.rm argument to mean(): map_dbl(df, mean, trim = 0.5, na.rm = TRUE);
- in purr package you can use map functions with anonymous functions like map_dbl(cyl, function(df) mean(df$disp)), or in shorter version: map_dbl(cyl, ~ mean(.$disp)), i.e. we replace the function definition (function(df)) with the '~', then when we need to refer to the element of cyl the function operates on (in this case df), we use a '.';
- there are also some useful shortcuts that come in handy when you want to subset each element of the .x argument. If the .f argument to a map function is set equal to a string, let's say "name", then purrr extracts the "name" element from every element of .x;
- another useful shortcut for subetting is to pass a numeric vector as the .f argument. This works just like passing a string but subsets by index rather than name;
- purrr also includes a pipe operator: %>%. The pipe operator is another shortcut that saves typing, but also increases readability. The explanation of the pipe operator is quite simple: x %>% f(y) is another way of writing f(x, y). That is, the left hand side of the pipe, x, becomes the first argument to the function, f(), on the right hand side of the pipe;
- safely() is an adverb; it takes a verb and modifies it. That is, it takes a function as an argument and it returns a function as its output. The function that is returned is modified so it never throws an error (and never stops the rest of your computation!). Instead, it always returns a list with two elements
          - result is the original result. If there was an error, this will be NULL;
          - error is an error object. If the operation was successful this will be NULL;
- safely can be used to create safe functions ex. safe_function=safe(function);
- purrr provides a function transpose() that reshapes a list so the inner-most level becomes the outer-most level. In otherwords, it turns a list-of-lists "inside-out";
- using map2 function you can run function with 2 alternative variables values ex. map2(n, mu, rnorm), both n and mu are lists of variables values, in pmap is used for more than 2 variables values, but they must be put into a list like pmap(list(n, mu), rnorm);
- invoke_map() is used to iterate over list of functions, the first argument is a list of functions, the second one specifies the arguments to the functions. In more complicated cases, the functions may take different arguments, or we may want to pass different values to each function. In this case, we need to supply invoke_map() with a list, where each element specifies the arguments to the corresponding function;
- walk() operates just like map() except it's designed for functions that don't return anything. You use walk() for functions with side effects like printing, plotting or saving;

##Tidyr
- gather(wide_df, my_key, my_val, -col) makes wide datasets long ex. gather(census, month (i.e. column name for column names), amount (i.e. column name for values column), -YEAR (you can also specify which columns should be gather using colname:colname); 
- spread(long_df, my_key, my_val) is the opposite of gather;
- separate(df, col=col_to_sep, into=c(), (sep="")) function allows you to separate one column into multiple columns. Unless you tell it otherwise, it will attempt to separate on any character that is not a letter or number. You can also specify a specific separator using the sep argument;
- the opposite to separate is unite(df, new_col, col_1, col_2, sep);
- unite(data, col, ..., sep = "_", remove = TRUE);

##Stringr
- str_detect(string, pattern)
- str_replace(string, pattern, replacement)
