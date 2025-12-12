
data analysis and manipulation tool

**Series** , one-dimensional labeled array . axis labels are called *index*.
`s = pd.Series(data, index=index)`

*ndarray* -  n dimensional homogeneous array

**DataFrame** , two-dimensional labeled data structure. rows labels are *index* and column lables are *columns* .
`pd.DataFrame({'yes': [50,21], 'no': [131,2]},index=,columns=)`

|     | yes | no  |
| --- | --- | --- |
| 0   | 50  | 131 |
| 1   | 21  | 2   |
`animals = pd.read_csv("filename.csv",index_col=0)`
`animals.to_csv("filename.csv")`

`df["age"][0]` - select 0th row and "age" column
`df["age"]>20`

`df.iloc[ [0,1,2] , [2,3] ]`  - select rows 0,1,2 from columns 2,3
`df.iloc[:,1]` - select all rows of column 1

`df.loc[ : , ['country','name'] ]`  -  select all rows from columns 'country' & 'name'

arguments for loc,iloc can be lists,slices or boolean masks
loc is inclusive

`df.set_index("title")`  - set some column as a index & returns a new data frame
`df.set_index("name",drop=False,inplace=True)`

`reviews.country == 'italy'` - returns a boolean series
`reviews.loc[ reviews.country == 'italy' ]` - select all rows & columns where country is italy.

`reviews.loc[(reviews.country == 'italy') & (reviews.points >= 90)]`
`|`  - pipe/or operator

`reviews.loc[reviews.country.isin(['italy','france'])]`
`isnull()`
`notnull()`

`reviews['index'] = range(len(reviews):0:-1)`

summary functions 
`reviews.points.mean()`  `.describe()  .unique()  .value_counts()`

map functions
`reviews.points.map(lambda p : p - mean)` 

`lambda arguments : expression`   no statements , no loops

`df.apply(func,axis=0)`   apply func column-wise  axis=0 ,  row-wise  axis=1

`df.nlargest(n,column)`

`df.loc[(df.points / df.prices).idxmax()]` - returns index of row with max value


`df.groupby(['country','province'])`
`df.groupby(['country']).points.agg([len,max,min])`

multi-index

`df.reset_index()`
`df.sort_values(by=['len'] , ascending = False)`
`df.sort_index()`

*data types* 

`reviews.price.dtype`
`reviews.dtypes`
`reviews.price.astype('float64')`

*null*
`reviews[pd.isnull(reviews.country)]`
`reviews.region.fillna("unknown")`
`reviews.price.isnull().sum()`

`reviews.taster.replace("kevin","juicewrld")`


*renaming and combining*

`reviews.rename(columns = {'points':'score'})`
`reviews.rename(index={0:'first',1:'second'})`

`pd.concat(['reddit','twitter'])`

```python
left = canadian_youtube.set_index(['title', 'trending_date'])
right = british_youtube.set_index(['title', 'trending_date'])

left.join(right, lsuffix='_CAN', rsuffix='_UK')
```
