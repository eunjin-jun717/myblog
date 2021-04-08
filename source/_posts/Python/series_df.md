---
date: 2021-04-08
tags: python
---
# [Python] Series & Dataframe
- Pandas의 데이터 구조\
: Series, Dataframe, Panel 3가지가 있다.
![series and dataframe](/hueman_images/python/series_df.png)

## 1. Series 
- pandas의 1차원 자료구조


```python
import pandas as pd

ser= pd.Series([10,20,30,40],index=list('abcd')) # 10,20,30,40의 리스트를 만드는데 이때의 index는 a,b,c,d로 지정
ser
```




    a    10
    b    20
    c    30
    d    40
    dtype: int64




```python
print(ser[3],ser[2],ser[1],ser[0])
print(ser['d'],ser['c'],ser['b'],ser['a'])
```

    40 30 20 10
    40 30 20 10
    


```python
print(ser['a':'d']) #index 문자로 slicing 하면 마지막까지 포함됨
```

    a    10
    b    20
    c    30
    d    40
    dtype: int64
    


```python
print(ser[0:3]) #index 숫자로 slicing 했을 때는 마지막이 포함되지 않음
```

    a    10
    b    20
    c    30
    dtype: int64
    

## 2. Dataframe
- pandas의 2차원 자료구조
- 행과 열


```python
data={'name':['bella','charlie'],
      'age':[25,27],
      'birthyear':[97,95]
      }
df1 = pd.DataFrame(data, index=list('bc'))
print(df1)
print(df1.info())
```

          name  age  birthyear
    b    bella   25         97
    c  charlie   27         95
    <class 'pandas.core.frame.DataFrame'>
    Index: 2 entries, b to c
    Data columns (total 3 columns):
     #   Column     Non-Null Count  Dtype 
    ---  ------     --------------  ----- 
     0   name       2 non-null      object
     1   age        2 non-null      int64 
     2   birthyear  2 non-null      int64 
    dtypes: int64(2), object(1)
    memory usage: 64.0+ bytes
    None
    


```python
print(df1.dtypes) # column의 datatype을 반환
```

    name         object
    age           int64
    birthyear     int64
    dtype: object
    


```python
print(df1.T) # dataframe을 치환함, 행과 열의 데이터를 치환한다. 
# 2*3 -> 3*2
```

                   b        c
    name       bella  charlie
    age           25       27
    birthyear     97       95
    


```python
print(df1.values) # 값들을 반환
```

    [['bella' 25 97]
     ['charlie' 27 95]]
    


```python
print(df1.index) # index를 반환
```

    Index(['b', 'c'], dtype='object')
    


```python
print(df1.columns) # column 명을 반환
```

    Index(['name', 'age', 'birthyear'], dtype='object')
    


```python
data2={'color':['red','orange','yellow','green','blue'],
      'flower':['rose','daisy','lotus','tulips','calendula'],
      'season':['spring','spring','winter','summer','fall'],
       'month':[10,5,6,7,11]
      }
df2=pd.DataFrame(data2,index=list('abcde'))
print(df2)
```

        color     flower  season  month
    a     red       rose  spring     10
    b  orange      daisy  spring      5
    c  yellow      lotus  winter      6
    d   green     tulips  summer      7
    e    blue  calendula    fall     11
    


```python
df2.head(3) # 상위데이터 3개
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>flower</th>
      <th>season</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>red</td>
      <td>rose</td>
      <td>spring</td>
      <td>10</td>
    </tr>
    <tr>
      <th>b</th>
      <td>orange</td>
      <td>daisy</td>
      <td>spring</td>
      <td>5</td>
    </tr>
    <tr>
      <th>c</th>
      <td>yellow</td>
      <td>lotus</td>
      <td>winter</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.tail(2) # 하위데이터 2개
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>flower</th>
      <th>season</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>d</th>
      <td>green</td>
      <td>tulips</td>
      <td>summer</td>
      <td>7</td>
    </tr>
    <tr>
      <th>e</th>
      <td>blue</td>
      <td>calendula</td>
      <td>fall</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.sort_index(axis=0, ascending=False) #axis=0일 경우, index 이름 기준 정렬, 내림차순
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>flower</th>
      <th>season</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>e</th>
      <td>blue</td>
      <td>calendula</td>
      <td>fall</td>
      <td>11</td>
    </tr>
    <tr>
      <th>d</th>
      <td>green</td>
      <td>tulips</td>
      <td>summer</td>
      <td>7</td>
    </tr>
    <tr>
      <th>c</th>
      <td>yellow</td>
      <td>lotus</td>
      <td>winter</td>
      <td>6</td>
    </tr>
    <tr>
      <th>b</th>
      <td>orange</td>
      <td>daisy</td>
      <td>spring</td>
      <td>5</td>
    </tr>
    <tr>
      <th>a</th>
      <td>red</td>
      <td>rose</td>
      <td>spring</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.sort_index(axis=1) #axis=1일 경우, index의 column 이름 기준 정렬, 오름차순
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>flower</th>
      <th>month</th>
      <th>season</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>red</td>
      <td>rose</td>
      <td>10</td>
      <td>spring</td>
    </tr>
    <tr>
      <th>b</th>
      <td>orange</td>
      <td>daisy</td>
      <td>5</td>
      <td>spring</td>
    </tr>
    <tr>
      <th>c</th>
      <td>yellow</td>
      <td>lotus</td>
      <td>6</td>
      <td>winter</td>
    </tr>
    <tr>
      <th>d</th>
      <td>green</td>
      <td>tulips</td>
      <td>7</td>
      <td>summer</td>
    </tr>
    <tr>
      <th>e</th>
      <td>blue</td>
      <td>calendula</td>
      <td>11</td>
      <td>fall</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.sort_values('color') # index명이나 column명 입력하면 키값이 정렬됨 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>flower</th>
      <th>season</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>e</th>
      <td>blue</td>
      <td>calendula</td>
      <td>fall</td>
      <td>11</td>
    </tr>
    <tr>
      <th>d</th>
      <td>green</td>
      <td>tulips</td>
      <td>summer</td>
      <td>7</td>
    </tr>
    <tr>
      <th>b</th>
      <td>orange</td>
      <td>daisy</td>
      <td>spring</td>
      <td>5</td>
    </tr>
    <tr>
      <th>a</th>
      <td>red</td>
      <td>rose</td>
      <td>spring</td>
      <td>10</td>
    </tr>
    <tr>
      <th>c</th>
      <td>yellow</td>
      <td>lotus</td>
      <td>winter</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



## 참조
- [https://harryp.tistory.com/868](https://harryp.tistory.com/868)
