# R-notes

##General
- attach(x) can be used to avoid constatly specifing data frame nam, for example:
     - attach(df) / df[column_name==x,] instead of df[df$column_name==x,]
- as.yearmon(x) from the 'zoo' package can be used to extract date and month only from the string;
- paste(x,y,z) is used to concatenate strings together;
- str() and summary() are used for fast data exploration, glimpse() is equivalent to str();
- when changing values in the column the desired mapping could be done in a vector like x=c('a'='A', 'b'='B',...) values can be the assigned by df$column_name_1=x[df$column_name_2]

##Dplyr
- data frames can be converted into table using tbl_df(), which makes it easier to work with, however table has the same properties as data frame so it can be manipulated the same way as data frame;
- there are five functions in dplyr that are called verbs:
     -select(), which returns a subset of the columns,
     -filter(), that is able to return a subset of the rows,
     -arrange(), that reorders the rows according to single or multiple variables,
     -mutate(), used to add columns from existing data,
     -summarise(), which reduces each group to a single row by calculating aggregate measures.
      
