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

#(day2)indexing,selecting and assigning
to access a specific part of df or csv file of different countries eg reviews 
>>reviews['country'][0]

*Indexing in pandas*
    The indexing operator and attribute selection are nice because they work just like they do in the rest of the Python ecosystem. As a novice, this makes them easy to pick up and use. However, pandas has its own accessor operators, loc and iloc. For more advanced operations, these are the ones you're supposed to be using
  1)index-based selection: selecting data based on its numerical position in the data. iloc follows this paradigm.
-To select the first row of data in a DataFrame, we may use the following:
>>reviews.iloc[0]
-For example, to select the country column from just the first, second, and third row, we would do:
>>reviews.iloc[:3, 0]
-*It's also possible to pass a list:*
>>reviews.iloc[[0, 1, 2], 0]]
-in pandas the ive integar wont specify a certain value but till that no.
>>review.iloc[-5:]  #here it will show the last 5

#Label-based selection
For example, to get the first entry in reviews, we would now do the following:
>>reviews.loc[0, 'country']
Note:
*iloc is conceptually simpler than loc because it ignores the dataset's indices. When we use iloc we treat the dataset like a big matrix (a list of lists), one that we have to index into by position. loc, by contrast, uses the information in the indices to do its work. Since your dataset usually has meaningful indices, it's usually easier to do things using loc instead.*

Choosing between loc and iloc
1. Indexing scheme
a).iloc → Python-style indexing (exclusive at end)
df.iloc[0:10] → rows 0 to 9 (10 rows total).

b).loc → Inclusive indexing
df.loc[0:10] → rows 0 to 10 (11 rows total).

3. When to use
a).iloc → Use when selecting by row/column positions (integer index).
Example: df.iloc[2, 3] → 3rd row, 4th column.
b).loc → Use when selecting by labels (names/values in index).
Example:
df.loc['Apples':'Potatoes'] → Selects all rows from "Apples" through "Potatoes" (inclusive).

#for set a particular column of dataframe as index
>>reviews.set_index("country")

#CONDITIONAL SELECTION
For example, suppose that we're interested specifically in better-than-average wines produced in Italy.
We can start by checking if each wine is Italian or not:
>>reviews.country == 'Italy'
1)This operation produced a Series of True/False booleans based on the country of each record. This result can then be used inside of loc to select the relevant data:
>>reviews.loc[reviews.country == 'Italy']
This DataFrame has ~20,000 rows. The original had ~130,000. That means that around 15% of wines originate from Italy.

2)We also wanted to know which ones are better than average. Wines are reviewed on a 80-to-100 point scale, so this could mean wines that accrued at least 90 points.
#*We can use the ampersand (&) to bring the two questions together:*
>>reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]

3)Suppose we'll buy any wine that's made in Italy or which is rated above average. For this we use a pipe (|):
reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]

Operators used
& → AND
| → OR
~ → NOT (negation)

/Pandas comes with a few built-in conditional selectors, two of which we will highlight here.
1)isin: isin is lets you select data whose value "is in" a list of values. For example, here's how we can use it to select wines only from Italy or France:/
>>reviews.loc[reviews.country.isin(['Italy', 'France'])]
equivalent>>reviews.loc[(reviews.country == 'Italy') | (reviews.country == 'France')]

2)The second is isnull (and its companion notnull). These methods let you highlight values which are (or are not) empty (NaN). For example, to filter out wines lacking a price tag in the dataset, here's what we would do:
>>reviews.loc[reviews.price.notnull()]
➡️ Select all rows where the price column has a value (is not missing).

📌 Assigning Data in Pandas
1️⃣ Assigning a constant value to a column
>>reviews['critic'] = 'everyone'
This creates a new column critic in the reviews DataFrame.
Every row gets the same value 'everyone'.
✅ Use case: When you want a default value for all rows.

2️⃣ Assigning an iterable of values
>>reviews['index_backwards'] = range(len(reviews), 0, -1)
range(len(reviews), 0, -1) creates numbers from the length of the DataFrame down to 1.
len(reviews) = 129971 → first row gets 129971,last row gets 1
Assigning this iterable fills each row with a different value.
✅ Use case: When you want unique values per row, like IDs, indices, or calculated values.
3️⃣ Key rules
a)Length must match
The iterable you assign must have the same number of elements as the rows in the DataFrame.Otherwise, Pandas will throw a ValueError.
b)Overwrite or create new
If the column exists, this will overwrite it.If it doesn’t, Pandas will create a new column
