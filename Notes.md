# R-notes

##General
- attach(x) can be used to avoid constatly specifing data frame nam, for example:
     - attach(df) / df[column_name==x,] instead of df[df$column_name==x,]
- as.yearmon(x) from the 'zoo' package can be used to extract date and month only from the string;
- paste(x,y,z) is used to concatenate strings together;
- str() and summary() are used for fast data exploration, glimpse() is equivalent to str();
- when changing values in the column the desired mapping could be done in a vector like x=c('a'='A', 'b'='B',...) values can be the assigned by df$column_name_1=x[df$column_name_2]
- rank() takes a group of values and calculates the rank of each value within the group ex. rank(c(21, 22, 24, 23)) returns [1] 1 2 4 3;
- prop.table(table(train$Survived)) <- gives table with proportions, you have to specify whether you want row-wise or column-wise proportions. This is done by setting the second argument of prop.table(), called margin, to 1 or 2, respectively;

##Dplyr
- data frames can be converted into table using tbl_df(), which makes it easier to work with, however table has the same properties as data frame so it can be manipulated the same way as data frame;
- there are five functions in dplyr that are called verbs
     - select(), which returns a subset of the columns,
     - filter(), that is able to return a subset of the rows,
     - arrange(), that reorders the rows according to single or multiple variables,
     - mutate(), used to add columns from existing data,
     - summarise(), which reduces each group to a single row by calculating aggregate measures;
- in the select statement both column names and indexes can be used, ex. select(df, 1:4, -2), -2 means that we don't want to display second column;
- dplyr comes with a set of helper functions that can help you select groups of variables inside a select() call
     - starts_with("X"): every name that starts with "X",
     - ends_with("X"): every name that ends with "X",
     - contains("X"): every name that contains "X",
     - matches("X"): every name that matches "X", where "X" can be a regular expression,
     - num_range("x", 1:5): the variables named x01, x02, x03, x04 and x05,
     - one_of(x): every name that appears in x, which should be a character vector.
- Pay attention here: When you refer to columns directly inside select(), you don't use quotes. If you use the helper functions, you do use quotes;
- mutate(my_df, x = a + b, y = x + c);
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
      
