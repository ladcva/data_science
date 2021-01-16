# Data Science Handbook
Data Science Handbook

# Data Science Handbook
# Table of contents
- [Table of contents](#table-of-contents)
- [Introduction](#introduction)
- [Data Preprocessing](#data-preprocessing)
  - [Import Dataset](#import-dataset)
  - [Select Data](#select-data)
    - [Using Index: iloc](#using-index-iloc)
  - [Numpy representation of DF](#numpy-representation-of-df)
  - [Handle Missing Data](#handle-missing-data)
  - [Encode Categorical Data](#encode-categorical-data)
    - [Encode Independent Variables](#encode-independent-variables)
    - [Encode Dependent Variables](#encode-dependent-variables)
    
- [Resources](#resources)


# Introduction 


[(Back to top)](#table-of-contents)

# Data Preprocessing 
## Import Dataset
```python
dataset = pd.read_csv("data.csv")

   Country   Age   Salary Purchased
0   France  44.0  72000.0        No
1    Spain  27.0  48000.0       Yes
2  Germany  30.0  54000.0        No
3    Spain  38.0  61000.0        No
4  Germany  40.0      NaN       Yes
5   France  35.0  58000.0       Yes
6    Spain   NaN  52000.0        No
7   France  48.0  79000.0       Yes
8  Germany  50.0  83000.0        No
9   France  37.0  67000.0       Yes
```
## Select Data
### Using Index iloc
- `.iloc[]` allowed inputs are:
  #### Selecting Rows
  - An integer, e.g. `dataset.iloc[0]` > return row 0 in `<class 'pandas.core.series.Series'>`
  ```
  Country      France
  Age              44
  Salary        72000
  Purchased        No
  ```
  - A list or array of integers, e.g.`dataset.iloc[[0]]` > return row 0 in DataFrame format
  ```
     Country   Age   Salary  Purchased
  0  France    44.0  72000.0        No
  ```
  - A slice object with ints, e.g. `dataset.iloc[:3]` > return row 0 up to row 3 in DataFrame format
  ```
       Country   Age   Salary Purchased
  0    France   44.0  72000.0        No
  1    Spain    27.0  48000.0       Yes
  2    Germany  30.0  54000.0        No
  ```
  #### Selecting Rows & Columns
  - Select First 3 Rows & up to Last Columns (not included) `X = dataset.iloc[:3, :-1]`
  ```
       Country   Age   Salary
  0   France  44.0  72000.0
  1    Spain  27.0  48000.0
  2  Germany  30.0  54000.0
  ```
### Numpy representation of DF
- `DataFrame.values`: Return a Numpy representation of the DataFrame (i.e: Only the values in the DataFrame will be returned, the axes labels will be removed)
- For ex: `X = dataset.iloc[:3, :-1].values`
```
[['France' 44.0 72000.0]
 ['Spain' 27.0 48000.0]
 ['Germany' 30.0 54000.0]]
```
[(Back to top)](#table-of-contents)

## Handle Missing Data
### SimpleImputer
-  sklearn.impute.`SimpleImputer(missing_values={should be set to np.nan} strategy={"mean",“median”, “most_frequent”, ..})`
- imputer.`fit(X[:, 1:3])`:	Fit the imputer on X.
- imputer.`transform(X[:, 1:3])`: 	Impute all missing values in X.

```Python
from sklearn.impute import SimpleImputer

#Create an instance of Class SimpleImputer: np.nan is the empty value in the dataset
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')

#Replace missing value from numerical Col 1 'Age', Col 2 'Salary'
imputer.fit(X[:, 1:3]) 

#transform will replace & return the new updated columns
X[:, 1:3] = imputer.transform(X[:, 1:3])
```

## Encode Categorical Data
### Encode Independent Variables
- Since for the independent variable, we will convert into vector of 0 & 1
- Using the `ColumnTransformer` class & 
- `OneHotEncoder`:  encoding technique for features are nominal(do not have any order)
![image](https://user-images.githubusercontent.com/64508435/104794298-a86e6f80-57e1-11eb-8ffc-aee2178762d1.png)

```Python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
```
- `transformers`: specify what kind of transformation, and which cols
- Tuple `('encoder' encoding transformation, instance of Class OneHotEncoder, [cols to transform])`
- `remainder ="passthrough"` > to keep the cols which not be transformed. Otherwise, the remaining cols will not be included 
```Python
ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(), [0])] , remainder="passthrough" )
```
- Fit and Transform with input = X in the Instance `ct` of class `ColumnTransformer`
```
#fit and transform with input = X
#np.array: need to convert output of fit_transform() from matrix to np.array
X = np.array(ct.fit_transform(X))
```
- Before converting categorical column [0] `Country`
```
   Country   Age   Salary Purchased
0   France  44.0  72000.0        No
1    Spain  27.0  48000.0       Yes
```
- After converting, France = [1.0, 0, 0] vector
```
[[1.0 0.0 0.0 44.0 72000.0]
 [0.0 0.0 1.0 27.0 48000.0]
 [0.0 1.0 0.0 30.0 54000.0]
```

### Encode Dependent Variables
- For the dependent variable, since it is the Label > we use `Label Encoder`
```Python
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
#output of fit_transform of Label Encoder is already a Numpy Array
y = le.fit_transform(y)

#y = [0 1 0 0 1 1 0 1 0 1]
```


# Resources:
### Podcast:
https://www.superdatascience.com/podcast/sds-041-inspiring-journey-totally-different-background-data-science




