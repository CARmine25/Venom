Creating data¶
There are two core objects in pandas: the DataFrame and the Series.
1)#DataFrame
A DataFrame is a table. It contains an array of individual entries, each of which has a certain value. Each entry corresponds to a row (or record) and a column.
For example, consider the following simple DataFrame
import pandas as pd
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])

2)Series
A Series, by contrast, is a sequence of data values. If a DataFrame is a table, a Series is a list. And in fact you can create one with nothing more than a list:
A Series is, in essence, a single column of a DataFrameSo you can assign row labels to the Series the same way as before, using an index parameter. 
However, a Series does not have a column name, it only has one overall name
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A'

reading data
 now set aside our toy datasets and see what a real dataset looks like when we read it into a DataFrame. We'll use the pd.read_csv() function to read the data
 into a DataFrame. This goes thusly
>>>wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv")
>>>wine_reviews.shape  #gives  you total entries and columns
>>>wine_reviews.head()  #.head() is used to quickly preview the first few rows of a DataFrame (or Series). to  specify no. of rows put the no. inside eg: .head(10)
 
 
 we can see in a dataset that the CSV file has a built-in index, which pandas did not pick up on automatically. To make pandas use that column for
 the index (instead of creating a new one from scratch), we can specify an index_col.
here,✔ pandas didn’t create a new index
✔ It used the CSV’s first column as the index so that theres noconfusion for two indexes present

>>>wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv", index_col=0)
wine_reviews.head()

#write code to save this DataFrame to disk as a csv file with the name  (imp)
>>animals = pd.DataFrame({'Cows': [12, 20], 'Goats': [22, 19]}, index=['Year 1', 'Year 2'])
>>animals.to_csv("cows_and_goats.csv",index=False)
>>x=pd.read_csv("cows_and_goats.csv")
>>x.head()
