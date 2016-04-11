# R-notes

##General
- attach(x) can be used to avoid constatly specifing data frame nam, for example:
     - attach(df) / df[column_name==x,] instead of df[df$column_name==x,]
- as.yearmon(x) from the 'zoo' package can be used to extract date and month only from the string;
- paste(x,y,z) is used to concatenate strings together;
      
