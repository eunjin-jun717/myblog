- [First look](#first_look)
    - [(1) Check data for NA](#check_na)
    
- [EDA(Exploratory Data Analysis)](#eda)
    - [(1) Survived](#survived)
    - [(2) Age](#age)
    - [(3) Cabin](#cabin)
    - [(3) Family](#family)
    - [(3) Pclass](#pclass)
    - [(3) Sex](#sex)
    - [(3) Embarked](#embarked)
    - [(3) Fare](#fare)



<a id="eda"></a>
## EDA(Exploratory Data Analysis)
> *references*
> [https://www.kaggle.com/demidova/titanic-eda-tutorial](https://www.kaggle.com/demidova/titanic-eda-tutorial)
> [https://www.kaggle.com/demidova/titanic-logistic-regression-random-forest-xgboost?scriptVersionId=46567425]

![titanic](https://ww.namu.la/s/1cc50931b5875401a9465ba06eaaf3d357ebfeabdf50346cd03636ab60ef0a9783a060b2c9cc7808148cd2a075699fa0f094e7f34df4a69fd5fdeb31137a37ef6e3a6c57a0b629606097a954052b7abba6a51a1a32ed5be9a92174b2ada23080602e6277fd7f0a200ddffcbd5b581746)
- [https://namu.wiki/jump/9AGb4mj%2Bgar2D116rRySHULPcuF9aQA9dU1%2FKaQlJabHnX1Bwo7dW3QKZZU5EDX7tyS7%2BeKInzFlBX0PyH2gvmr0xlEeT19AQhYRU4yv8erx25eqVyS5NlWU2pDAk3mhBaO4i%2BaABck5vAWwFaAE0g%3D%3D](https://namu.wiki/jump/9AGb4mj%2Bgar2D116rRySHULPcuF9aQA9dU1%2FKaQlJabHnX1Bwo7dW3QKZZU5EDX7tyS7%2BeKInzFlBX0PyH2gvmr0xlEeT19AQhYRU4yv8erx25eqVyS5NlWU2pDAk3mhBaO4i%2BaABck5vAWwFaAE0g%3D%3D)

<a id='data_import'></a>
### (1) Data Import


```python
import pandas as pd
import matplotlib
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sb
import os

print("Version Pandas", pd.__version__)
print("Version Matplotlib", matplotlib.__version__)
print("Version Numpy", np.__version__)
print("Version Seaborn", sb.__version__)

os.listdir('../input/tabular-playground-series-apr-2021/')
```

    Version Pandas 1.2.3
    Version Matplotlib 3.4.1
    Version Numpy 1.19.5
    Version Seaborn 0.11.1
    




    ['sample_submission.csv', 'train.csv', 'test.csv']




```python
BASE_DIR = '../input/tabular-playground-series-apr-2021/'
train = pd.read_csv(BASE_DIR + 'train.csv')
test = pd.read_csv(BASE_DIR + 'test.csv')
sample_submission = pd.read_csv(BASE_DIR + 'sample_submission.csv')

train.shape, test.shape, sample_submission.shape
```




    ((100000, 12), (100000, 11), (100000, 2))




```python
train.head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>Oconnor, Frankie</td>
      <td>male</td>
      <td>NaN</td>
      <td>2</td>
      <td>0</td>
      <td>209245</td>
      <td>27.14</td>
      <td>C12239</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Bryan, Drew</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>27323</td>
      <td>13.35</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>Owens, Kenneth</td>
      <td>male</td>
      <td>0.33</td>
      <td>1</td>
      <td>2</td>
      <td>CA 457703</td>
      <td>71.29</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>Kramer, James</td>
      <td>male</td>
      <td>19.00</td>
      <td>0</td>
      <td>0</td>
      <td>A. 10866</td>
      <td>13.04</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>Bond, Michael</td>
      <td>male</td>
      <td>25.00</td>
      <td>0</td>
      <td>0</td>
      <td>427635</td>
      <td>7.76</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
test.head()
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
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100000</td>
      <td>3</td>
      <td>Holliday, Daniel</td>
      <td>male</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>24745</td>
      <td>63.01</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>100001</td>
      <td>3</td>
      <td>Nguyen, Lorraine</td>
      <td>female</td>
      <td>53.0</td>
      <td>0</td>
      <td>0</td>
      <td>13264</td>
      <td>5.81</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>100002</td>
      <td>1</td>
      <td>Harris, Heather</td>
      <td>female</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>25990</td>
      <td>38.91</td>
      <td>B15315</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>100003</td>
      <td>2</td>
      <td>Larsen, Eric</td>
      <td>male</td>
      <td>25.0</td>
      <td>0</td>
      <td>0</td>
      <td>314011</td>
      <td>12.93</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>100004</td>
      <td>1</td>
      <td>Cleary, Sarah</td>
      <td>female</td>
      <td>17.0</td>
      <td>0</td>
      <td>2</td>
      <td>26203</td>
      <td>26.89</td>
      <td>B22515</td>
      <td>C</td>
    </tr>
  </tbody>
</table>
</div>




```python
sample_submission.head()
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
      <th>PassengerId</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>100001</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>100002</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>100003</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>100004</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
frames= [train, test]
total_df=pd.concat(frames, sort=False)
print('total data shape: ', total_df.shape)
total_df.head()
```

    total data shape:  (200000, 12)
    




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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1.0</td>
      <td>1</td>
      <td>Oconnor, Frankie</td>
      <td>male</td>
      <td>NaN</td>
      <td>2</td>
      <td>0</td>
      <td>209245</td>
      <td>27.14</td>
      <td>C12239</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
      <td>Bryan, Drew</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>27323</td>
      <td>13.35</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.0</td>
      <td>3</td>
      <td>Owens, Kenneth</td>
      <td>male</td>
      <td>0.33</td>
      <td>1</td>
      <td>2</td>
      <td>CA 457703</td>
      <td>71.29</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.0</td>
      <td>3</td>
      <td>Kramer, James</td>
      <td>male</td>
      <td>19.00</td>
      <td>0</td>
      <td>0</td>
      <td>A. 10866</td>
      <td>13.04</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1.0</td>
      <td>3</td>
      <td>Bond, Michael</td>
      <td>male</td>
      <td>25.00</td>
      <td>0</td>
      <td>0</td>
      <td>427635</td>
      <td>7.76</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
total_df.describe(include=[object])
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
      <th>Name</th>
      <th>Sex</th>
      <th>Ticket</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>200000</td>
      <td>200000</td>
      <td>190196</td>
      <td>61303</td>
      <td>199473</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>174854</td>
      <td>2</td>
      <td>132613</td>
      <td>45442</td>
      <td>3</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Smith, James</td>
      <td>male</td>
      <td>A/5</td>
      <td>C11139</td>
      <td>S</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>61</td>
      <td>125871</td>
      <td>646</td>
      <td>7</td>
      <td>140981</td>
    </tr>
  </tbody>
</table>
</div>




```python
total_df.describe(include=[object])
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
      <th>Name</th>
      <th>Sex</th>
      <th>Ticket</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>200000</td>
      <td>200000</td>
      <td>190196</td>
      <td>61303</td>
      <td>199473</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>174854</td>
      <td>2</td>
      <td>132613</td>
      <td>45442</td>
      <td>3</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Smith, James</td>
      <td>male</td>
      <td>A/5</td>
      <td>C11139</td>
      <td>S</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>61</td>
      <td>125871</td>
      <td>646</td>
      <td>7</td>
      <td>140981</td>
    </tr>
  </tbody>
</table>
</div>



<a id="check_na"></a>
### (2) Check data for NA
- dataset의 feature들을 살펴보고, null data의 여부를 체크해보자

> 종속변수
> - **Survived(생존여부)**: target label (1,0) -> integer 

> 독립변수
> - **PassengerId**: 10000명
> - **Pclass(티켓의 클래스)**: Upper(1), Middle(2), Lower(3) -> categorical -> integer
> - **Name(이름)**: 탑승자 성명들 
> - **Sex(성별)**: Male, Female -> binary -> string
> - **Age(나이)**: continuous -> integer
> - **SibSp(함께 탑승한 형제와 배우자의 수)**: quantitative -> integer 
> - **Parch(함께 탑승한 부모, 아이의 수)**: quantitative -> integer
> - **Ticket(티켓 번호)**: alphabet + integer -> string
> - **Fare(탑승료)**: continous -> float
> - **Cabin(객실 번호)**: alphabet + integer -> string
> - **Embarked(탑승항구)**: C(Cherbourg), Q(Queenstown), S(Southhampton) -> string

*references*
- [https://kaggle-kr.tistory.com/17](https://kaggle-kr.tistory.com/17)


```python
total_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 200000 entries, 0 to 99999
    Data columns (total 12 columns):
     #   Column       Non-Null Count   Dtype  
    ---  ------       --------------   -----  
     0   PassengerId  200000 non-null  int64  
     1   Survived     100000 non-null  float64
     2   Pclass       200000 non-null  int64  
     3   Name         200000 non-null  object 
     4   Sex          200000 non-null  object 
     5   Age          193221 non-null  float64
     6   SibSp        200000 non-null  int64  
     7   Parch        200000 non-null  int64  
     8   Ticket       190196 non-null  object 
     9   Fare         199733 non-null  float64
     10  Cabin        61303 non-null   object 
     11  Embarked     199473 non-null  object 
    dtypes: float64(3), int64(4), object(5)
    memory usage: 19.8+ MB
    

Age, Fare -> numeric variables\
Pclass -> integer but in fact 'categorical variable'


```python
total_df_na=total_df.isna().sum()
train_na=train.isna().sum()
test_na=test.isna().sum()

pd.concat([train_na, test_na, total_df_na], axis=1, sort=False, keys=['Train NA','Test NA','Total NA'])
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
      <th>Train NA</th>
      <th>Test NA</th>
      <th>Total NA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>PassengerId</th>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Survived</th>
      <td>0</td>
      <td>NaN</td>
      <td>100000</td>
    </tr>
    <tr>
      <th>Pclass</th>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Name</th>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Sex</th>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Age</th>
      <td>3292</td>
      <td>3487.0</td>
      <td>6779</td>
    </tr>
    <tr>
      <th>SibSp</th>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Parch</th>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Ticket</th>
      <td>4623</td>
      <td>5181.0</td>
      <td>9804</td>
    </tr>
    <tr>
      <th>Fare</th>
      <td>134</td>
      <td>133.0</td>
      <td>267</td>
    </tr>
    <tr>
      <th>Cabin</th>
      <td>67866</td>
      <td>70831.0</td>
      <td>138697</td>
    </tr>
    <tr>
      <th>Embarked</th>
      <td>250</td>
      <td>277.0</td>
      <td>527</td>
    </tr>
  </tbody>
</table>
</div>



missing data를 handling하기 위해서 EDA에서는 dataset을 합쳤지만, ML에서는 'data leakage'를 피하기 위해서 오직 train data set만 사용할 것이다. 


```python
total_df.describe()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>200000.000000</td>
      <td>100000.000000</td>
      <td>200000.000000</td>
      <td>193221.000000</td>
      <td>200000.000000</td>
      <td>200000.000000</td>
      <td>199733.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>99999.500000</td>
      <td>0.427740</td>
      <td>2.237920</td>
      <td>34.464565</td>
      <td>0.442120</td>
      <td>0.473695</td>
      <td>44.652071</td>
    </tr>
    <tr>
      <th>std</th>
      <td>57735.171256</td>
      <td>0.494753</td>
      <td>0.868273</td>
      <td>16.783847</td>
      <td>0.819392</td>
      <td>0.937125</td>
      <td>67.436104</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.080000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.050000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>49999.750000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>22.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>10.080000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>99999.500000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>31.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>20.250000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>149999.250000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>48.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>34.850000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>199999.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>87.000000</td>
      <td>8.000000</td>
      <td>9.000000</td>
      <td>744.660000</td>
    </tr>
  </tbody>
</table>
</div>



<a id="survived"></a>
### (1) Survived
- train set에서 survived의 0,1 분포가 어떤지 확인해보겠습니다. 
- 분포에 따라 모델의 평가 방법이 달라질 수 있습니다. 


```python
plt.figure(figsize=(6, 4.5))

ax= sb.countplot(x='Survived', data=total_df, palette=['#4287f5','#7cd91e'])

plt.xticks(np.arange(2), ['Drowned','Survived'])
plt.title('Overall survival', fontsize=14)
plt.xlabel('Survived vs Drowned')
plt.ylabel('Number of Passendgers')

labels=(total_df['Survived'].value_counts())

for i,v in enumerate(labels):
    ax.text(i, v-40, str(v), horizontalalignment='center', size=14, color='w', fontweight='bold')
    
plt.show()
```


    
![Survived/Drowned](/hueman_images/python/kaggle/output_19_0.png)
    



```python
total_df['Survived'].value_counts(normalize=True)
```




    0.0    0.57226
    1.0    0.42774
    Name: Survived, dtype: float64



<a id="independent_variables"></a>
### (2) Independent Variables 
> *references*
> - [https://wikidocs.net/75068](https://wikidocs.net/75068)

#### 1) Age
6779 : age missing values
- 3292 : train dataset
- 3487 : test dataset


```python
plt.figure(figsize=(15,3))

sb.distplot(total_df[(total_df['Age']>0)].Age, kde_kws={'lw':3}, bins=50)

plt.title('Distribution of passengers age (total data)', fontsize=14)
plt.xlabel('Age')
plt.ylabel('Frequency')

plt.tight_layout()
```

    /opt/conda/lib/python3.7/site-packages/seaborn/distributions.py:2557: FutureWarning: `distplot` is a deprecated function and will be removed in a future version. Please adapt your code to use either `displot` (a figure-level function with similar flexibility) or `histplot` (an axes-level function for histograms).
      warnings.warn(msg, FutureWarning)
    


    
![age distribution](/hueman_images/python/kaggle/output_23_1.png)
    



```python
age_distr= pd.DataFrame(total_df['Age'].describe())
age_distr.transpose()
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Age</th>
      <td>193221.0</td>
      <td>34.464565</td>
      <td>16.783847</td>
      <td>0.08</td>
      <td>22.0</td>
      <td>31.0</td>
      <td>48.0</td>
      <td>87.0</td>
    </tr>
  </tbody>
</table>
</div>



0.08세 ~ 87세까지 다양하게 나이대가 있으며 mean=34.46세 이다. 

### 1-1) Age by surviving status


```python
plt.figure(figsize=(15,3))

sb.boxplot(y='Survived', x='Age', data=train, palette=['#4287f5','#7cd91e'], fliersize=0, orient='h')

sb.stripplot(y='Survived',x='Age', data=train, linewidth=0.6, palette=['#4287f5','#7cd91e'], orient='h')
plt.yticks(np.arange(2), ['Drowned','Survived'])
plt.title('Age distribution grouped by survivng status (train data)', fontsize=14)
plt.ylabel('Passengers status after the tragedy')
plt.tight_layout()
```


    
![age](/hueman_images/python/kaggle/output_27_0.png)
    



```python
pd.DataFrame(total_df.groupby('Survived')['Age'].describe())
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>Survived</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>55290.0</td>
      <td>36.708695</td>
      <td>17.809058</td>
      <td>0.08</td>
      <td>24.0</td>
      <td>36.0</td>
      <td>52.0</td>
      <td>83.0</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>41418.0</td>
      <td>40.553799</td>
      <td>18.742172</td>
      <td>0.08</td>
      <td>27.0</td>
      <td>43.0</td>
      <td>55.0</td>
      <td>87.0</td>
    </tr>
  </tbody>
</table>
</div>



### 1-2) Age by Pclass


```python
plt.figure(figsize=(20,6))

palette=sb.cubehelix_palette(5, start=3)

plt.subplot(1,2,1)
sb.boxplot(x='Pclass', y='Age', data=total_df, palette=palette, fliersize=0)

plt.xticks(np.arange(3), ['1st class','2nd class','3rd class'])
plt.title('Age distribution grouped by ticket class (total data)', fontsize=16)
plt.xlabel('Ticket class')

plt.subplot(1,2,2)

age_1_class = total_df[(total_df['Age']>0)&(total_df['Pclass']==1)]
age_2_class = total_df[(total_df['Age']>0)&(total_df['Pclass']==2)]
age_3_class = total_df[(total_df['Age']>0)&(total_df['Pclass']==3)]

# Ploting the 3 variables that we create
sb.kdeplot(age_1_class["Age"], shade=True, color='#eed4d0', label = '1st class')
sb.kdeplot(age_2_class["Age"], shade=True,  color='#cda0aa', label = '2nd class')
sb.kdeplot(age_3_class["Age"], shade=True,color='#a2708e', label = '3rd class')
plt.title('Age distribution grouped by ticket class (total data)',fontsize= 16)
plt.xlabel('Age')
plt.xlim(0, 90)
plt.tight_layout()
plt.show()
```


    
![](/hueman_images/python/kaggle/output_30_0.png)
    



```python
pd.DataFrame(total_df.groupby('Pclass')['Age'].describe())
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>55365.0</td>
      <td>40.672757</td>
      <td>14.715634</td>
      <td>0.08</td>
      <td>28.0</td>
      <td>41.0</td>
      <td>52.0</td>
      <td>83.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>36606.0</td>
      <td>36.855067</td>
      <td>18.470623</td>
      <td>0.08</td>
      <td>23.0</td>
      <td>35.0</td>
      <td>52.0</td>
      <td>87.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>101250.0</td>
      <td>30.205570</td>
      <td>15.954521</td>
      <td>0.08</td>
      <td>20.0</td>
      <td>26.0</td>
      <td>41.0</td>
      <td>85.0</td>
    </tr>
  </tbody>
</table>
</div>



2nd 클래스는 1st, 3rd 클래스에 비해 더 넓은 분포를 가진다. 또한 거의 대칭 적이다.\
가장 나이가 적은 passenger은 1,2,3 등급 동일한 나이인 0.08세이다. \
가장 나이가 많은 passenger은 2nd 클래스의 87세이다.

3rd 클래스 mean age= 30.2세\
2nd 클래스 mean age= 36.9세\
1st 클래스 mean age= 40.7세

### 1-3) Age vs Pclass vs Sex


```python
plt.figure(figsize=(20, 5))
palette = "Set3"

plt.subplot(1, 3, 1)
sb.boxplot(x = 'Sex', y = 'Age', data = age_1_class,
     palette = palette, fliersize = 0)
#sb.stripplot(x = 'Sex', y = 'Age', data = age_1_class,linewidth = 0.6, palette = palette)
plt.title('1st class Age distribution by Sex',fontsize= 14)
plt.ylim(-5, 80)

plt.subplot(1, 3, 2)
sb.boxplot(x = 'Sex', y = 'Age', data = age_2_class,
     palette = palette, fliersize = 0)
#sb.stripplot(x = 'Sex', y = 'Age', data = age_2_class,linewidth = 0.6, palette = palette)
plt.title('2nd class Age distribution by Sex',fontsize= 14)
plt.ylim(-5, 80)

plt.subplot(1, 3, 3)
sb.boxplot(x = 'Sex', y = 'Age',  data = age_3_class,
     order = ['female', 'male'], palette = palette, fliersize = 0)
#sb.stripplot(x = 'Sex', y = 'Age', data = age_3_class,order = ['female', 'male'], linewidth = 0.6, palette = palette)
plt.title('3rd class Age distribution by Sex',fontsize= 14)
plt.ylim(-5, 80)

plt.show()
```


    
![](/hueman_images/python/kaggle/output_34_0.png)
    



```python
age_1_class_stat = pd.DataFrame(age_1_class.groupby('Sex')['Age'].describe())
age_2_class_stat = pd.DataFrame(age_2_class.groupby('Sex')['Age'].describe())
age_3_class_stat = pd.DataFrame(age_3_class.groupby('Sex')['Age'].describe())

pd.concat([age_1_class_stat, age_2_class_stat, age_3_class_stat], axis=0, sort = False, keys = ['1st', '2nd', '3rd'])
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
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th></th>
      <th>Sex</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">1st</th>
      <th>female</th>
      <td>25790.0</td>
      <td>41.974173</td>
      <td>15.253798</td>
      <td>0.08</td>
      <td>29.0</td>
      <td>43.0</td>
      <td>54.0</td>
      <td>83.0</td>
    </tr>
    <tr>
      <th>male</th>
      <td>29575.0</td>
      <td>39.537896</td>
      <td>14.132517</td>
      <td>0.08</td>
      <td>28.0</td>
      <td>39.0</td>
      <td>51.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2nd</th>
      <th>female</th>
      <td>17554.0</td>
      <td>37.283031</td>
      <td>20.558883</td>
      <td>0.08</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>55.0</td>
      <td>87.0</td>
    </tr>
    <tr>
      <th>male</th>
      <td>19052.0</td>
      <td>36.460753</td>
      <td>16.302224</td>
      <td>0.08</td>
      <td>24.0</td>
      <td>34.0</td>
      <td>50.0</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">3rd</th>
      <th>female</th>
      <td>28349.0</td>
      <td>27.222629</td>
      <td>17.974800</td>
      <td>0.08</td>
      <td>15.0</td>
      <td>24.0</td>
      <td>39.0</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>male</th>
      <td>72901.0</td>
      <td>31.365545</td>
      <td>14.936175</td>
      <td>0.08</td>
      <td>22.0</td>
      <td>27.0</td>
      <td>41.0</td>
      <td>83.0</td>
    </tr>
  </tbody>
</table>
</div>



#### 2) Cabin
- 첫번째 코드만 추출함
- A: lst class
- B
- C: 3rd class
- D: walking area
- E: 1st and 2nd class
- F: 2nd class, 2rd class
- G: boiler room
- T: boat deck
- U: Unknown


```python
total_df['Cabin']=total_df['Cabin'].str.split('',expand=True)[1]
total_df.loc[total_df['Cabin'].isna(), 'Cabin']='X'
```


```python
fig = plt.figure(figsize=(20, 5))

ax1 = fig.add_subplot(131)
sb.countplot(x = 'Cabin', data = total_df, palette = "hls", order = total_df['Cabin'].value_counts().index, ax = ax1)
plt.title('Passengers distribution by Cabin',fontsize= 16)
plt.ylabel('Number of passengers')

ax2 = fig.add_subplot(132)
Cabin_by_class = total_df.groupby('Cabin')['Pclass'].value_counts(normalize = True).unstack()
Cabin_by_class.plot(kind='bar', stacked='True',color = ['#eed4d0', '#cda0aa', '#a2708e'], ax = ax2)
plt.legend(('1st class', '2nd class', '3rd class'), loc=(1.04,0))
plt.title('Proportion of classes on each Cabin',fontsize= 16)
plt.xticks(rotation = False)

ax3 = fig.add_subplot(133)
Cabin_by_survived = total_df.groupby('Cabin')['Survived'].value_counts(normalize = True).unstack()
Cabin_by_survived = Cabin_by_survived.sort_values(by = 1, ascending = False)
Cabin_by_survived.plot(kind='bar', stacked='True', color=["#3f3e6fd1", "#85c6a9"], ax = ax3)
plt.title('Proportion of survived/drowned passengers by Cabin',fontsize= 16)
plt.legend(( 'Drowned', 'Survived'), loc=(1.04,0))
plt.xticks(rotation = False)
plt.tight_layout()

plt.show()

```

    /opt/conda/lib/python3.7/site-packages/pandas/plotting/_matplotlib/tools.py:400: MatplotlibDeprecationWarning: 
    The is_first_col function was deprecated in Matplotlib 3.4 and will be removed two minor releases later. Use ax.get_subplotspec().is_first_col() instead.
      if ax.is_first_col():
    


    
![](/hueman_images/python/kaggle/output_38_1.png)
    


- 대부분의 passengers는 Cabin code가 없다. 
- Cabin code가 나와있는 승객들 중 가장 많은 수를 차지하는 deck은 'C'이며 lst class ticket이다. 'C' deck은 살아남은 승객들 중 4번째이다. 
- 가장 많은 생존률을 가진 deck은 'F'이다. 
- 'A' deck은 lifeboats와 가장 가까운 deck이였지만 생존률은 가장 낮은 확률을 보이고 있다. 

### 3) Family
- Family size = Sib + Parch +1


```python
total_df['Family_size']=total_df['SibSp']+total_df['Parch']+1
family_size=total_df['Family_size'].value_counts()
print('Family size and number of passengers:')
print(family_size)
```

    Family size and number of passengers:
    1     116448
    2      30139
    3      25289
    4      19724
    5       4151
    6       2021
    7       1184
    10       397
    11       242
    8        162
    9        157
    14        46
    12        26
    13         7
    18         4
    15         3
    Name: Family_size, dtype: int64
    


```python
fig = plt.figure(figsize = (12,4))

ax1 = fig.add_subplot(121)
ax = sb.countplot(total_df['Family_size'], ax = ax1)

# calculate passengers for each category
labels = (total_df['Family_size'].value_counts())
# add result numbers on barchart
for i, v in enumerate(labels):
    ax.text(i, v+6, str(v), horizontalalignment = 'center', size = 10, color = 'black')
    
plt.title('Passengers distribution by family size')
plt.ylabel('Number of passengers')

ax2 = fig.add_subplot(122)
d = total_df.groupby('Family_size')['Survived'].value_counts(normalize = True).unstack()
d.plot(kind='bar', color=["#3f3e6fd1", "#85c6a9"], stacked='True', ax = ax2)
plt.title('Proportion of survived/drowned passengers by family size (train data)')
plt.legend(( 'Drowned', 'Survived'), loc=(1.04,0))
plt.xticks(rotation = False)

plt.tight_layout()
```

    /opt/conda/lib/python3.7/site-packages/seaborn/_decorators.py:43: FutureWarning: Pass the following variable as a keyword arg: x. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      FutureWarning
    /opt/conda/lib/python3.7/site-packages/pandas/plotting/_matplotlib/tools.py:400: MatplotlibDeprecationWarning: 
    The is_first_col function was deprecated in Matplotlib 3.4 and will be removed two minor releases later. Use ax.get_subplotspec().is_first_col() instead.
      if ax.is_first_col():
    


    
![](/hueman_images/python/kaggle/output_42_1.png)
    


- family size가 15명인 그룹은 모두 살아남지 못하였다. 
- 대부분은 혼자 여행하는 사람들이였고, 생존율은 40% 정도 이다. 
- 가장 높은 생존율을 보이는 family size는 2,3 정도이다. 
- 4개의 category로 family size group을 나누어보겠다.
- single
- usual(sizes 2,3,4,5)
- big(6,7,8,9)
- large(all bigger then 10)


```python
total_df['Family_size_group']=total_df['Family_size'].map(lambda x: 'f_single' if x ==1
                                                         else('f_usual' if 6>x>=2
                                                             else('f_big' if 10>x>=6
                                                                 else('f_large'))))
```


```python
fig = plt.figure(figsize = (14,5))

ax1 = fig.add_subplot(121)
d = total_df.groupby('Family_size_group')['Survived'].value_counts(normalize = True).unstack()
d = d.sort_values(by = 1, ascending = False)
d.plot(kind='bar', stacked='True', color = ["#3f3e6fd1", "#85c6a9"], ax = ax1)
plt.title('Proportion of survived/drowned passengers by family size')
plt.legend(( 'Drowned', 'Survived'), loc=(1.04,0))
_ = plt.xticks(rotation=False)


ax2 = fig.add_subplot(122)
d2 = total_df.groupby('Family_size_group')['Pclass'].value_counts(normalize = True).unstack()
d2 = d2.sort_values(by = 1, ascending = False)
d2.plot(kind='bar', stacked='True', color = ['#eed4d0', '#cda0aa', '#a2708e'], ax = ax2)
plt.legend(('1st class', '2nd class', '3rd class'), loc=(1.04,0))
plt.title('Proportion of 1st/2nd/3rd ticket class in family group size')
_ = plt.xticks(rotation=False)

plt.tight_layout()
```


    
![](/hueman_images/python/kaggle/output_45_0.png)
    


#### 4) Pclass


```python
ax = sb.countplot(total_df['Pclass'], palette = ['#eed4d0', '#cda0aa', '#a2708e'])
# calculate passengers for each category
labels = (total_df['Pclass'].value_counts(sort = False))
# add result numbers on barchart
for i, v in enumerate(labels):
    ax.text(i, v+2, str(v), horizontalalignment = 'center', size = 12, color = 'black', fontweight = 'bold')
    
    
plt.title('Passengers distribution by Pclass')
plt.ylabel('Number of passengers')
plt.tight_layout()
```

    /opt/conda/lib/python3.7/site-packages/seaborn/_decorators.py:43: FutureWarning: Pass the following variable as a keyword arg: x. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      FutureWarning
    


    
![](/hueman_images/python/kaggle/output_47_1.png)
    



```python
fig = plt.figure(figsize=(14, 5))

ax1 = fig.add_subplot(121)
sb.countplot(x = 'Pclass', hue = 'Survived', data = total_df, palette=["#3f3e6fd1", "#85c6a9"], ax = ax1)
plt.title('Number of survived/drowned passengers by class (train data)')
plt.ylabel('Number of passengers')
plt.legend(( 'Drowned', 'Survived'), loc=(1.04,0))
_ = plt.xticks(rotation=False)

ax2 = fig.add_subplot(122)
d = total_df.groupby('Pclass')['Survived'].value_counts(normalize = True).unstack()
d.plot(kind='bar', stacked='True', ax = ax2, color =["#3f3e6fd1", "#85c6a9"])
plt.title('Proportion of survived/drowned passengers by class (train data)')
plt.legend(( 'Drowned', 'Survived'), loc=(1.04,0))
_ = plt.xticks(rotation=False)

plt.tight_layout()
```

    /opt/conda/lib/python3.7/site-packages/pandas/plotting/_matplotlib/tools.py:400: MatplotlibDeprecationWarning: 
    The is_first_col function was deprecated in Matplotlib 3.4 and will be removed two minor releases later. Use ax.get_subplotspec().is_first_col() instead.
      if ax.is_first_col():
    


    
![](/hueman_images/python/kaggle/output_48_1.png)
    


- 가장 많은 승객이 탄 3등급 임에도 불구하고 생존율은 가장 적은 승객이 탑승한 1등급에 비해 더 적은 생존율을 보인다. 

### 4-1) Pclass vs Surviving vs Sex


```python
sb.catplot(x = 'Pclass', hue = 'Survived', col = 'Sex', kind = 'count', data = total_df , palette=["#3f3e6fd1", "#85c6a9"])

plt.tight_layout()
```


    
![](/hueman_images/python/kaggle/output_51_0.png)
    


- 1등급 클래스의 남성 승객들의 대부분은 살아남지 못하였고 여성들은 대부분 살아남았다. 
- 3등급 클래스의 여성의 절반 이상은 살아남았다. 

### 5) Embarked


```python
fig = plt.figure(figsize = (15,4))

ax1 = fig.add_subplot(131)
palette = sb.cubehelix_palette(5, start = 2)
ax = sb.countplot(total_df['Embarked'], palette = palette, order = ['C', 'Q', 'S'], ax = ax1)
plt.title('Number of passengers by Embarked')
plt.ylabel('Number of passengers')

# calculate passengers for each category
labels = (total_df['Embarked'].value_counts())
labels = labels.sort_index()
# add result numbers on barchart
for i, v in enumerate(labels):
    ax.text(i, v+10, str(v), horizontalalignment = 'center', size = 10, color = 'black')
    

ax2 = fig.add_subplot(132)
surv_by_emb = total_df.groupby('Embarked')['Survived'].value_counts(normalize = True)
surv_by_emb = surv_by_emb.unstack().sort_index()
surv_by_emb.plot(kind='bar', stacked='True', color=["#3f3e6fd1", "#85c6a9"], ax = ax2)
plt.title('Proportion of survived/drowned passengers by Embarked (train data)')
plt.legend(( 'Drowned', 'Survived'), loc=(1.04,0))
_ = plt.xticks(rotation=False)


ax3 = fig.add_subplot(133)
class_by_emb = total_df.groupby('Embarked')['Pclass'].value_counts(normalize = True)
class_by_emb = class_by_emb.unstack().sort_index()
class_by_emb.plot(kind='bar', stacked='True', color = ['#eed4d0', '#cda0aa', '#a2708e'], ax = ax3)
plt.legend(('1st class', '2nd class', '3rd class'), loc=(1.04,0))
plt.title('Proportion of clases by Embarked')
_ = plt.xticks(rotation=False)

plt.tight_layout()
```

    /opt/conda/lib/python3.7/site-packages/seaborn/_decorators.py:43: FutureWarning: Pass the following variable as a keyword arg: x. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      FutureWarning
    /opt/conda/lib/python3.7/site-packages/pandas/plotting/_matplotlib/tools.py:400: MatplotlibDeprecationWarning: 
    The is_first_col function was deprecated in Matplotlib 3.4 and will be removed two minor releases later. Use ax.get_subplotspec().is_first_col() instead.
      if ax.is_first_col():
    


    
![](/hueman_images/python/kaggle/output_54_1.png)
    


- 대부분의 승객(140981)들은 S 항구에서 출발하였고 S 항구에서 출발한 승객들의 생존율은 가장 낮았다. 또한 3등급 클래스 사람들이 대부분이다. 
- C항구에서 출발한 승객들은 75% 이상의 생존율을 보인다. 
- 가장 적은 승객들이 탑승한 Q항구에는 가장 많은 l등급 클래스의 승객들이 탑승하였다. 


```python
sb.catplot(x="Embarked", y="Fare", kind="violin", inner=None,
            data=total_df, height = 6, palette = palette, order = ['C', 'Q', 'S'])
plt.title('Distribution of Fare by Embarked')
plt.tight_layout()
```


    
![](/hueman_images/python/kaggle/output_56_0.png)
    



```python
pd.DataFrame(total_df.groupby('Embarked')['Fare'].describe())
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>Embarked</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>C</th>
      <td>44440.0</td>
      <td>73.673693</td>
      <td>87.985577</td>
      <td>1.51</td>
      <td>13.54</td>
      <td>31.42</td>
      <td>89.5025</td>
      <td>744.46</td>
    </tr>
    <tr>
      <th>Q</th>
      <td>13977.0</td>
      <td>78.479737</td>
      <td>92.941969</td>
      <td>2.29</td>
      <td>10.81</td>
      <td>28.45</td>
      <td>115.6700</td>
      <td>744.66</td>
    </tr>
    <tr>
      <th>S</th>
      <td>140791.0</td>
      <td>32.125407</td>
      <td>50.934735</td>
      <td>0.05</td>
      <td>9.50</td>
      <td>13.24</td>
      <td>29.9900</td>
      <td>727.65</td>
    </tr>
  </tbody>
</table>
</div>



### 6) Fare


```python
fig, ax = plt.subplots(1, 1, figsize=(8, 8))
g = sb.distplot(total_df['Fare'], color='r', label='Skewness : {:.2f}'.format(total_df['Fare'].skew()), ax=ax)
g = g.legend(loc='best')
```

    /opt/conda/lib/python3.7/site-packages/seaborn/distributions.py:2557: FutureWarning: `distplot` is a deprecated function and will be removed in a future version. Please adapt your code to use either `displot` (a figure-level function with similar flexibility) or `histplot` (an axes-level function for histograms).
      warnings.warn(msg, FutureWarning)
    


    
![](/hueman_images/python/kaggle/output_59_1.png)
    



```python
fare_map = total_df[['Fare', 'Pclass']].dropna().groupby('Pclass').median().to_dict()
total_df['Fare'] = total_df['Fare'].fillna(total_df['Pclass'].map(fare_map['Fare']))

total_df['Fare'] = total_df['Fare'].map(lambda i: np.log(i) if i > 0 else 0)
fig, ax = plt.subplots(1, 1, figsize=(8, 8))
g = sb.distplot(total_df['Fare'], color='b', label='Skewness : {:.2f}'.format(total_df['Fare'].skew()), ax=ax)
g = g.legend(loc='best')
```

    /opt/conda/lib/python3.7/site-packages/seaborn/distributions.py:2557: FutureWarning: `distplot` is a deprecated function and will be removed in a future version. Please adapt your code to use either `displot` (a figure-level function with similar flexibility) or `histplot` (an axes-level function for histograms).
      warnings.warn(msg, FutureWarning)
    


    
![](/hueman_images/python/kaggle/output_60_1.png)
    


# Feature Engineering

- Null values 확인


```python
total_df
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>Family_size</th>
      <th>Family_size_group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1.0</td>
      <td>1</td>
      <td>Oconnor, Frankie</td>
      <td>male</td>
      <td>NaN</td>
      <td>2</td>
      <td>0</td>
      <td>209245</td>
      <td>3.301009</td>
      <td>C</td>
      <td>S</td>
      <td>3</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
      <td>Bryan, Drew</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>27323</td>
      <td>2.591516</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.0</td>
      <td>3</td>
      <td>Owens, Kenneth</td>
      <td>male</td>
      <td>0.33</td>
      <td>1</td>
      <td>2</td>
      <td>CA 457703</td>
      <td>4.266756</td>
      <td>X</td>
      <td>S</td>
      <td>4</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.0</td>
      <td>3</td>
      <td>Kramer, James</td>
      <td>male</td>
      <td>19.00</td>
      <td>0</td>
      <td>0</td>
      <td>A. 10866</td>
      <td>2.568022</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1.0</td>
      <td>3</td>
      <td>Bond, Michael</td>
      <td>male</td>
      <td>25.00</td>
      <td>0</td>
      <td>0</td>
      <td>427635</td>
      <td>2.048982</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>99995</th>
      <td>199995</td>
      <td>NaN</td>
      <td>3</td>
      <td>Cash, Cheryle</td>
      <td>female</td>
      <td>27.00</td>
      <td>0</td>
      <td>0</td>
      <td>7686</td>
      <td>2.314514</td>
      <td>X</td>
      <td>Q</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>99996</th>
      <td>199996</td>
      <td>NaN</td>
      <td>1</td>
      <td>Brown, Howard</td>
      <td>male</td>
      <td>59.00</td>
      <td>1</td>
      <td>0</td>
      <td>13004</td>
      <td>4.224056</td>
      <td>X</td>
      <td>S</td>
      <td>2</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>99997</th>
      <td>199997</td>
      <td>NaN</td>
      <td>3</td>
      <td>Lightfoot, Cameron</td>
      <td>male</td>
      <td>47.00</td>
      <td>0</td>
      <td>0</td>
      <td>4383317</td>
      <td>2.386007</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>99998</th>
      <td>199998</td>
      <td>NaN</td>
      <td>1</td>
      <td>Jacobsen, Margaret</td>
      <td>female</td>
      <td>49.00</td>
      <td>1</td>
      <td>2</td>
      <td>PC 26988</td>
      <td>3.390473</td>
      <td>B</td>
      <td>C</td>
      <td>4</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>99999</th>
      <td>199999</td>
      <td>NaN</td>
      <td>1</td>
      <td>Fishback, Joanna</td>
      <td>female</td>
      <td>41.00</td>
      <td>0</td>
      <td>2</td>
      <td>PC 41824</td>
      <td>5.275100</td>
      <td>E</td>
      <td>C</td>
      <td>3</td>
      <td>f_usual</td>
    </tr>
  </tbody>
</table>
<p>200000 rows × 14 columns</p>
</div>




```python
total_df.isna().sum()
```




    PassengerId               0
    Survived             100000
    Pclass                    0
    Name                      0
    Sex                       0
    Age                    6779
    SibSp                     0
    Parch                     0
    Ticket                 9804
    Fare                      0
    Cabin                     0
    Embarked                527
    Family_size               0
    Family_size_group         0
    dtype: int64



## 1) Data Correlation


```python
fig, ax=plt.subplots(1, 3, figsize=(17,5))
feature_lst=['Pclass','Age','Fare','Sex','Family_size']

corr=total_df[feature_lst].corr()

mask=np.zeros_like(corr, dtype=np.bool)
mask[np.triu_indices_from(mask)]=True

for idx, method in enumerate(['pearson','kendall','spearman']):
    sb.heatmap(total_df[feature_lst].corr(method=method), ax=ax[idx],
              square=True, annot=True, fmt='.2f', center=0, linewidth=2,
              cbar=False, cmap=sb.diverging_palette(240, 10, as_cmap=True),
        mask=mask)
    ax[idx].set_title(f'{method.capitalize()} Correlation', loc='left', fontweight='bold')
    
plt.show()
```


    
![](/hueman_images/python/kaggle/output_66_0.png)
    


### 2) Age
- 각 클래스마다 나이의 평균을 각 클래스마다의 null 값에 넣어주었다. 


```python
age_map= total_df[['Age','Pclass']].dropna().groupby('Pclass').median().to_dict()
total_df['Age']=total_df['Age'].fillna(total_df['Pclass'].map(age_map['Age']))
```

### 3) Embarked


```python
print('Embarked has ', sum(total_df['Embarked'].isnull()), ' Null values')
```

    Embarked has  527  Null values
    


```python
total_df['Embarked'] = total_df['Embarked'].fillna('S')
```


```python
total_df
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>Family_size</th>
      <th>Family_size_group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1.0</td>
      <td>1</td>
      <td>Oconnor, Frankie</td>
      <td>male</td>
      <td>41.00</td>
      <td>2</td>
      <td>0</td>
      <td>209245</td>
      <td>3.301009</td>
      <td>C</td>
      <td>S</td>
      <td>3</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
      <td>Bryan, Drew</td>
      <td>male</td>
      <td>26.00</td>
      <td>0</td>
      <td>0</td>
      <td>27323</td>
      <td>2.591516</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.0</td>
      <td>3</td>
      <td>Owens, Kenneth</td>
      <td>male</td>
      <td>0.33</td>
      <td>1</td>
      <td>2</td>
      <td>CA 457703</td>
      <td>4.266756</td>
      <td>X</td>
      <td>S</td>
      <td>4</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.0</td>
      <td>3</td>
      <td>Kramer, James</td>
      <td>male</td>
      <td>19.00</td>
      <td>0</td>
      <td>0</td>
      <td>A. 10866</td>
      <td>2.568022</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1.0</td>
      <td>3</td>
      <td>Bond, Michael</td>
      <td>male</td>
      <td>25.00</td>
      <td>0</td>
      <td>0</td>
      <td>427635</td>
      <td>2.048982</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>99995</th>
      <td>199995</td>
      <td>NaN</td>
      <td>3</td>
      <td>Cash, Cheryle</td>
      <td>female</td>
      <td>27.00</td>
      <td>0</td>
      <td>0</td>
      <td>7686</td>
      <td>2.314514</td>
      <td>X</td>
      <td>Q</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>99996</th>
      <td>199996</td>
      <td>NaN</td>
      <td>1</td>
      <td>Brown, Howard</td>
      <td>male</td>
      <td>59.00</td>
      <td>1</td>
      <td>0</td>
      <td>13004</td>
      <td>4.224056</td>
      <td>X</td>
      <td>S</td>
      <td>2</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>99997</th>
      <td>199997</td>
      <td>NaN</td>
      <td>3</td>
      <td>Lightfoot, Cameron</td>
      <td>male</td>
      <td>47.00</td>
      <td>0</td>
      <td>0</td>
      <td>4383317</td>
      <td>2.386007</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>99998</th>
      <td>199998</td>
      <td>NaN</td>
      <td>1</td>
      <td>Jacobsen, Margaret</td>
      <td>female</td>
      <td>49.00</td>
      <td>1</td>
      <td>2</td>
      <td>PC 26988</td>
      <td>3.390473</td>
      <td>B</td>
      <td>C</td>
      <td>4</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>99999</th>
      <td>199999</td>
      <td>NaN</td>
      <td>1</td>
      <td>Fishback, Joanna</td>
      <td>female</td>
      <td>41.00</td>
      <td>0</td>
      <td>2</td>
      <td>PC 41824</td>
      <td>5.275100</td>
      <td>E</td>
      <td>C</td>
      <td>3</td>
      <td>f_usual</td>
    </tr>
  </tbody>
</table>
<p>200000 rows × 14 columns</p>
</div>



### 4) Name


```python
total_df['Name'] = total_df['Name'].map(lambda x: x.split(',')[0])
```

### 5) Ticket


```python
total_df['Ticket'] = total_df['Ticket'].fillna('X').map(lambda x:str(x).split()[0] if len(str(x).split()) > 1 else 'X')
```

### 6) Drop
- PassengerId, Name, SibSp, Parch, Cabin


```python
total_df
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>Family_size</th>
      <th>Family_size_group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1.0</td>
      <td>1</td>
      <td>Oconnor</td>
      <td>male</td>
      <td>41.00</td>
      <td>2</td>
      <td>0</td>
      <td>X</td>
      <td>3.301009</td>
      <td>C</td>
      <td>S</td>
      <td>3</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
      <td>Bryan</td>
      <td>male</td>
      <td>26.00</td>
      <td>0</td>
      <td>0</td>
      <td>X</td>
      <td>2.591516</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.0</td>
      <td>3</td>
      <td>Owens</td>
      <td>male</td>
      <td>0.33</td>
      <td>1</td>
      <td>2</td>
      <td>CA</td>
      <td>4.266756</td>
      <td>X</td>
      <td>S</td>
      <td>4</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.0</td>
      <td>3</td>
      <td>Kramer</td>
      <td>male</td>
      <td>19.00</td>
      <td>0</td>
      <td>0</td>
      <td>A.</td>
      <td>2.568022</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1.0</td>
      <td>3</td>
      <td>Bond</td>
      <td>male</td>
      <td>25.00</td>
      <td>0</td>
      <td>0</td>
      <td>X</td>
      <td>2.048982</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>99995</th>
      <td>199995</td>
      <td>NaN</td>
      <td>3</td>
      <td>Cash</td>
      <td>female</td>
      <td>27.00</td>
      <td>0</td>
      <td>0</td>
      <td>X</td>
      <td>2.314514</td>
      <td>X</td>
      <td>Q</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>99996</th>
      <td>199996</td>
      <td>NaN</td>
      <td>1</td>
      <td>Brown</td>
      <td>male</td>
      <td>59.00</td>
      <td>1</td>
      <td>0</td>
      <td>X</td>
      <td>4.224056</td>
      <td>X</td>
      <td>S</td>
      <td>2</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>99997</th>
      <td>199997</td>
      <td>NaN</td>
      <td>3</td>
      <td>Lightfoot</td>
      <td>male</td>
      <td>47.00</td>
      <td>0</td>
      <td>0</td>
      <td>X</td>
      <td>2.386007</td>
      <td>X</td>
      <td>S</td>
      <td>1</td>
      <td>f_single</td>
    </tr>
    <tr>
      <th>99998</th>
      <td>199998</td>
      <td>NaN</td>
      <td>1</td>
      <td>Jacobsen</td>
      <td>female</td>
      <td>49.00</td>
      <td>1</td>
      <td>2</td>
      <td>PC</td>
      <td>3.390473</td>
      <td>B</td>
      <td>C</td>
      <td>4</td>
      <td>f_usual</td>
    </tr>
    <tr>
      <th>99999</th>
      <td>199999</td>
      <td>NaN</td>
      <td>1</td>
      <td>Fishback</td>
      <td>female</td>
      <td>41.00</td>
      <td>0</td>
      <td>2</td>
      <td>PC</td>
      <td>5.275100</td>
      <td>E</td>
      <td>C</td>
      <td>3</td>
      <td>f_usual</td>
    </tr>
  </tbody>
</table>
<p>200000 rows × 14 columns</p>
</div>




```python
total_df.drop(['PassengerId','Name','Family_size_group','Family_size'], axis=1, inplace=True)
total_df.shape
```




    (200000, 10)




```python
total_df['Sex']=total_df['Sex'].map({'female':0, 'male':1})
total_df=pd.get_dummies(total_df, columns=['Embarked'], prefix='Embarked')
total_df=pd.get_dummies(total_df, columns=['Cabin'], prefix='Cabin')
total_df=pd.get_dummies(total_df, columns=['Ticket'], prefix='Ticket')
#total_df=pd.get_dummies(total_df, columns=['Family_size_group'], prefix='Family_size_group')
```


```python
total_df
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>Embarked_C</th>
      <th>Embarked_Q</th>
      <th>Embarked_S</th>
      <th>...</th>
      <th>Ticket_SOTON/OQ</th>
      <th>Ticket_STON/O</th>
      <th>Ticket_STON/O2.</th>
      <th>Ticket_STON/OQ.</th>
      <th>Ticket_SW/PP</th>
      <th>Ticket_W./C.</th>
      <th>Ticket_W.E.P.</th>
      <th>Ticket_W/C</th>
      <th>Ticket_WE/P</th>
      <th>Ticket_X</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
      <td>41.00</td>
      <td>2</td>
      <td>0</td>
      <td>3.301009</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>3</td>
      <td>1</td>
      <td>26.00</td>
      <td>0</td>
      <td>0</td>
      <td>2.591516</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>3</td>
      <td>1</td>
      <td>0.33</td>
      <td>1</td>
      <td>2</td>
      <td>4.266756</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>3</td>
      <td>1</td>
      <td>19.00</td>
      <td>0</td>
      <td>0</td>
      <td>2.568022</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.0</td>
      <td>3</td>
      <td>1</td>
      <td>25.00</td>
      <td>0</td>
      <td>0</td>
      <td>2.048982</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>99995</th>
      <td>NaN</td>
      <td>3</td>
      <td>0</td>
      <td>27.00</td>
      <td>0</td>
      <td>0</td>
      <td>2.314514</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99996</th>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>59.00</td>
      <td>1</td>
      <td>0</td>
      <td>4.224056</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99997</th>
      <td>NaN</td>
      <td>3</td>
      <td>1</td>
      <td>47.00</td>
      <td>0</td>
      <td>0</td>
      <td>2.386007</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99998</th>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>49.00</td>
      <td>1</td>
      <td>2</td>
      <td>3.390473</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>99999</th>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>41.00</td>
      <td>0</td>
      <td>2</td>
      <td>5.275100</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>200000 rows × 69 columns</p>
</div>



## Split data


```python
X = total_df[:train.shape[0]]
print("X Shape is:", X.shape)
y = X['Survived']
X.drop(['Survived'], axis=1, inplace=True)
test_data = total_df[train.shape[0]:].drop(columns=['Survived'])
test_data.info()
```

    X Shape is: (100000, 69)
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 100000 entries, 0 to 99999
    Data columns (total 68 columns):
     #   Column             Non-Null Count   Dtype  
    ---  ------             --------------   -----  
     0   Pclass             100000 non-null  int64  
     1   Sex                100000 non-null  int64  
     2   Age                100000 non-null  float64
     3   SibSp              100000 non-null  int64  
     4   Parch              100000 non-null  int64  
     5   Fare               100000 non-null  float64
     6   Embarked_C         100000 non-null  uint8  
     7   Embarked_Q         100000 non-null  uint8  
     8   Embarked_S         100000 non-null  uint8  
     9   Cabin_A            100000 non-null  uint8  
     10  Cabin_B            100000 non-null  uint8  
     11  Cabin_C            100000 non-null  uint8  
     12  Cabin_D            100000 non-null  uint8  
     13  Cabin_E            100000 non-null  uint8  
     14  Cabin_F            100000 non-null  uint8  
     15  Cabin_G            100000 non-null  uint8  
     16  Cabin_T            100000 non-null  uint8  
     17  Cabin_X            100000 non-null  uint8  
     18  Ticket_A.          100000 non-null  uint8  
     19  Ticket_A./5.       100000 non-null  uint8  
     20  Ticket_A.5.        100000 non-null  uint8  
     21  Ticket_A/4         100000 non-null  uint8  
     22  Ticket_A/4.        100000 non-null  uint8  
     23  Ticket_A/5         100000 non-null  uint8  
     24  Ticket_A/5.        100000 non-null  uint8  
     25  Ticket_A/S         100000 non-null  uint8  
     26  Ticket_A4.         100000 non-null  uint8  
     27  Ticket_AQ/3.       100000 non-null  uint8  
     28  Ticket_AQ/4        100000 non-null  uint8  
     29  Ticket_C           100000 non-null  uint8  
     30  Ticket_C.A.        100000 non-null  uint8  
     31  Ticket_C.A./SOTON  100000 non-null  uint8  
     32  Ticket_CA          100000 non-null  uint8  
     33  Ticket_CA.         100000 non-null  uint8  
     34  Ticket_F.C.        100000 non-null  uint8  
     35  Ticket_F.C.C.      100000 non-null  uint8  
     36  Ticket_Fa          100000 non-null  uint8  
     37  Ticket_LP          100000 non-null  uint8  
     38  Ticket_P/PP        100000 non-null  uint8  
     39  Ticket_PC          100000 non-null  uint8  
     40  Ticket_PP          100000 non-null  uint8  
     41  Ticket_S.C./A.4.   100000 non-null  uint8  
     42  Ticket_S.C./PARIS  100000 non-null  uint8  
     43  Ticket_S.O./P.P.   100000 non-null  uint8  
     44  Ticket_S.O.C.      100000 non-null  uint8  
     45  Ticket_S.O.P.      100000 non-null  uint8  
     46  Ticket_S.P.        100000 non-null  uint8  
     47  Ticket_S.W./PP     100000 non-null  uint8  
     48  Ticket_SC          100000 non-null  uint8  
     49  Ticket_SC/A.3      100000 non-null  uint8  
     50  Ticket_SC/A4       100000 non-null  uint8  
     51  Ticket_SC/AH       100000 non-null  uint8  
     52  Ticket_SC/PARIS    100000 non-null  uint8  
     53  Ticket_SC/Paris    100000 non-null  uint8  
     54  Ticket_SCO/W       100000 non-null  uint8  
     55  Ticket_SO/C        100000 non-null  uint8  
     56  Ticket_SOTON/O.Q.  100000 non-null  uint8  
     57  Ticket_SOTON/O2    100000 non-null  uint8  
     58  Ticket_SOTON/OQ    100000 non-null  uint8  
     59  Ticket_STON/O      100000 non-null  uint8  
     60  Ticket_STON/O2.    100000 non-null  uint8  
     61  Ticket_STON/OQ.    100000 non-null  uint8  
     62  Ticket_SW/PP       100000 non-null  uint8  
     63  Ticket_W./C.       100000 non-null  uint8  
     64  Ticket_W.E.P.      100000 non-null  uint8  
     65  Ticket_W/C         100000 non-null  uint8  
     66  Ticket_WE/P        100000 non-null  uint8  
     67  Ticket_X           100000 non-null  uint8  
    dtypes: float64(2), int64(4), uint8(62)
    memory usage: 11.3 MB
    

    /opt/conda/lib/python3.7/site-packages/pandas/core/frame.py:4315: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      errors=errors,
    


```python
from sklearn.model_selection import train_test_split

#X_train, X_valid, y_train, y_valid = train_test_split(X, y, test_size = 0.3, random_state=42)
X_train, X_valid, y_train, y_valid = train_test_split(X, y, test_size = 0.3, stratify = X[['Pclass']], random_state=42)
X_train.shape, X_valid.shape, y_train.shape, y_valid.shape
```




    ((70000, 68), (30000, 68), (70000,), (30000,))



## Modeling
> *references*
> [https://www.kaggle.com/j2hoon85/tps-april-sklearn-pycaret-for-newbies](https://www.kaggle.com/j2hoon85/tps-april-sklearn-pycaret-for-newbies)

### XGBoost Shap


```python
import xgboost
import shap

# train an XGBoost model
xgb_model = xgboost.XGBClassifier().fit(X_train, y_train)

# explain the model's predictions using SHAP
# (same syntax works for LightGBM, CatBoost, scikit-learn, transformers, Spark, etc.)
explainer = shap.Explainer(xgb_model)
shap_values = explainer(X)

# visualize the first prediction's explanation
shap.plots.waterfall(shap_values[0])

shap.plots.bar(shap_values)
```

    The use of label encoder in XGBClassifier is deprecated and will be removed in a future release. To remove this warning, do the following: 1) Pass option use_label_encoder=False when constructing XGBClassifier object; and 2) Encode your labels (y) as integers starting with 0, i.e. 0, 1, 2, ..., [num_class - 1].
    

    [06:00:38] WARNING: ../src/learner.cc:1095: Starting in XGBoost 1.3.0, the default evaluation metric used with the objective 'binary:logistic' was changed from 'error' to 'logloss'. Explicitly set eval_metric if you'd like to restore the old behavior.
    

    ntree_limit is deprecated, use `iteration_range` or model slicing instead.
    


    
![](/hueman_images/python/kaggle/output_87_3.png)
    



    
![](/hueman_images/python/kaggle/output_87_4.png)
    


### Decision Tree


```python
from sklearn.metrics import accuracy_score
def acc_score(y_true, y_pred, **kwargs):
    return accuracy_score(y_true, (y_pred > 0.5).astype(int), **kwargs)
```


```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import roc_curve, roc_auc_score
from matplotlib import pyplot as plt

tree_model = DecisionTreeClassifier(max_depth=7)
tree_model.fit(X_train, y_train)
predictions = tree_model.predict_proba(X_valid)
AUC = roc_auc_score(y_valid, predictions[:,1])
ACC = acc_score(y_valid, predictions[:,1])
print("Model AUC:", AUC)
print("Model Accurarcy:", ACC)
print("\n")

fpr, tpr, _ = roc_curve(y_valid, predictions[:,1])

fig, ax = plt.subplots(figsize=(10, 6))

ax.plot(fpr, tpr)
ax.text(x = 0.3, 
        y = 0.4, 
        s = "Model AUC is {}\n\nModel Accuracy is {}".format(np.round(AUC, 2), np.round(ACC, 2)), 
        fontsize=16, bbox=dict(facecolor='gray', alpha=0.3))
ax.set_xlabel('FPR')
ax.set_ylabel('TPR')
ax.set_title('ROC curve')

plt.show()
```

    Model AUC: 0.8505605139706627
    Model Accurarcy: 0.7833333333333333
    
    
    


    
![](/hueman_images/python/kaggle/output_90_1.png)
    



```python
from sklearn import tree
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import roc_curve, roc_auc_score, accuracy_score, confusion_matrix
from matplotlib import pyplot as plt

SEED = 0

class sk_helper(object):
    def __init__(self, model, seed=0, params={}):
        params['random_state']=seed
        self.model=model(**params)
        self.model_name=str(model).split('.')[-1][:-2]
        
    def train(self, X_train, y_train):
        self.model.fit(X_train, y_train)
        
    def predict(self, y_valid):
        return self.model.predict(y_valid)
    
    def fit(self, x, y):
        return self.model.fit(x,y)
    
    # feature importance
    def feature_importances(self, X_train, y_train):
        return self.model.fit(X_train, y_train).feature_importances_
        
    # roc_curve
    def roc_curve_graph(self, X_train, y_train, X_valid, y_valid):
        self.model.fit(X_train, y_train)
        
        print("model_name:", self.model_name)
        model_name = self.model_name
        preds_proba = self.model.predict_proba(X_valid)
        preds = (preds_proba[:, 1] > 0.5).astype(int)
        auc = roc_auc_score(y_valid, preds_proba[:, 1])
        acc = accuracy_score(y_valid, preds)
        confusion = confusion_matrix(y_valid, preds)
        print('Confusion Matrix')
        print(confusion)
        print("Model AUC: {0:.3f}, Model Accuracy: {1:.3f}\n".format(auc, acc))
        fpr, tpr, _ = roc_curve(y_valid, preds_proba[:,1])
        fig, ax = plt.subplots(figsize=(10, 6))

        ax.plot(fpr, tpr)
        ax.text(x = 0.3, 
                y = 0.4, 
                s = "Model AUC is {}\n\nModel Accuracy is {}".format(np.round(auc, 2), np.round(acc, 2)), 
                fontsize=16, bbox=dict(facecolor='gray', alpha=0.3))
        ax.set_xlabel('FPR')
        ax.set_ylabel('TPR')
        ax.set_title('ROC curve of {}'.format(model_name), fontsize=16)

        plt.show()
```


```python
%%time

from sklearn.ensemble import RandomForestClassifier

rf_params = {
    'n_jobs': -1,
    'n_estimators': 500,
     'warm_start': True, 
     #'max_features': 0.2,
    'max_depth': 10,
    'min_samples_leaf': 2,
    'max_features' : 'sqrt',
    'verbose': 1
}

rf_model=sk_helper(model=RandomForestClassifier, seed=SEED, params=rf_params)
rf_model.roc_curve_graph(X_train, y_train, X_valid, y_valid)
```

    [Parallel(n_jobs=-1)]: Using backend ThreadingBackend with 4 concurrent workers.
    [Parallel(n_jobs=-1)]: Done  42 tasks      | elapsed:    0.8s
    [Parallel(n_jobs=-1)]: Done 192 tasks      | elapsed:    3.5s
    [Parallel(n_jobs=-1)]: Done 442 tasks      | elapsed:    8.2s
    [Parallel(n_jobs=-1)]: Done 500 out of 500 | elapsed:    9.3s finished
    [Parallel(n_jobs=4)]: Using backend ThreadingBackend with 4 concurrent workers.
    [Parallel(n_jobs=4)]: Done  42 tasks      | elapsed:    0.1s
    

    model_name: RandomForestClassifier
    

    [Parallel(n_jobs=4)]: Done 192 tasks      | elapsed:    0.3s
    [Parallel(n_jobs=4)]: Done 442 tasks      | elapsed:    0.6s
    [Parallel(n_jobs=4)]: Done 500 out of 500 | elapsed:    0.7s finished
    

    Confusion Matrix
    [[14356  2879]
     [ 3582  9183]]
    Model AUC: 0.855, Model Accuracy: 0.785
    
    


    
![](/hueman_images/python/kaggle/output_92_4.png)
    


    CPU times: user 38.2 s, sys: 250 ms, total: 38.4 s
    Wall time: 10.5 s
    


```python
%%time

import lightgbm as lgb

lgb_params = {
    'metric': 'binary_logloss',
    'n_estimators': 1000,
    'objective': 'binary',
    'random_state': 2021,
    'learning_rate': 0.01,
    'min_child_samples': 150,
    'reg_alpha': 3e-5,
    'reg_lambda': 9e-2,
    'num_leaves': 20,
    'max_depth': 16,
    'colsample_bytree': 0.8,
    'subsample': 0.8,
    'subsample_freq': 2,
    'max_bin': 240
}

lgb_model=sk_helper(model=lgb.LGBMClassifier, seed=SEED, params=rf_params)
lgb_model.roc_curve_graph(X_train, y_train, X_valid, y_valid)
```


<style type='text/css'>
.datatable table.frame { margin-bottom: 0; }
.datatable table.frame thead { border-bottom: none; }
.datatable table.frame tr.coltypes td {  color: #FFFFFF;  line-height: 6px;  padding: 0 0.5em;}
.datatable .bool    { background: #DDDD99; }
.datatable .object  { background: #565656; }
.datatable .int     { background: #5D9E5D; }
.datatable .float   { background: #4040CC; }
.datatable .str     { background: #CC4040; }
.datatable .row_index {  background: var(--jp-border-color3);  border-right: 1px solid var(--jp-border-color0);  color: var(--jp-ui-font-color3);  font-size: 9px;}
.datatable .frame tr.coltypes .row_index {  background: var(--jp-border-color0);}
.datatable th:nth-child(2) { padding-left: 12px; }
.datatable .hellipsis {  color: var(--jp-cell-editor-border-color);}
.datatable .vellipsis {  background: var(--jp-layout-color0);  color: var(--jp-cell-editor-border-color);}
.datatable .na {  color: var(--jp-cell-editor-border-color);  font-size: 80%;}
.datatable .footer { font-size: 9px; }
.datatable .frame_dimensions {  background: var(--jp-border-color3);  border-top: 1px solid var(--jp-border-color0);  color: var(--jp-ui-font-color3);  display: inline-block;  opacity: 0.6;  padding: 1px 10px 1px 5px;}
</style>



    [LightGBM] [Warning] Unknown parameter: max_features
    [LightGBM] [Warning] Unknown parameter: min_samples_leaf
    [LightGBM] [Warning] Unknown parameter: warm_start
    [LightGBM] [Warning] Accuracy may be bad since you didn't explicitly set num_leaves OR 2^max_depth > num_leaves. (num_leaves=31).
    [LightGBM] [Warning] Unknown parameter: max_features
    [LightGBM] [Warning] Unknown parameter: min_samples_leaf
    [LightGBM] [Warning] Unknown parameter: warm_start
    [LightGBM] [Warning] Accuracy may be bad since you didn't explicitly set num_leaves OR 2^max_depth > num_leaves. (num_leaves=31).
    [LightGBM] [Info] Number of positive: 30009, number of negative: 39991
    [LightGBM] [Warning] Auto-choosing row-wise multi-threading, the overhead of testing was 0.020202 seconds.
    You can set `force_row_wise=true` to remove the overhead.
    And if memory is not enough, you can set `force_col_wise=true`.
    [LightGBM] [Info] Total Bins 543
    [LightGBM] [Info] Number of data points in the train set: 70000, number of used features: 65
    [LightGBM] [Info] [binary:BoostFromScore]: pavg=0.428700 -> initscore=-0.287157
    [LightGBM] [Info] Start training from score -0.287157
    [LightGBM] [Warning] No further splits with positive gain, best gain: -inf
    [LightGBM] [Warning] No further splits with positive gain, best gain: -inf
    [LightGBM] [Warning] No further splits with positive gain, best gain: -inf
    model_name: LGBMClassifier
    Confusion Matrix
    [[14065  3170]
     [ 3286  9479]]
    Model AUC: 0.855, Model Accuracy: 0.785
    
    


    
![](/hueman_images/python/kaggle/output_93_2.png)
    


    CPU times: user 5.81 s, sys: 134 ms, total: 5.94 s
    Wall time: 3.82 s
    


```python
!pip install catboost
```

    Requirement already satisfied: catboost in /opt/conda/lib/python3.7/site-packages (0.25.1)
    Requirement already satisfied: matplotlib in /opt/conda/lib/python3.7/site-packages (from catboost) (3.4.1)
    Requirement already satisfied: plotly in /opt/conda/lib/python3.7/site-packages (from catboost) (4.14.3)
    Requirement already satisfied: scipy in /opt/conda/lib/python3.7/site-packages (from catboost) (1.5.4)
    Requirement already satisfied: numpy>=1.16.0 in /opt/conda/lib/python3.7/site-packages (from catboost) (1.19.5)
    Requirement already satisfied: pandas>=0.24.0 in /opt/conda/lib/python3.7/site-packages (from catboost) (1.2.3)
    Requirement already satisfied: six in /opt/conda/lib/python3.7/site-packages (from catboost) (1.15.0)
    Requirement already satisfied: graphviz in /opt/conda/lib/python3.7/site-packages (from catboost) (0.8.4)
    Requirement already satisfied: python-dateutil>=2.7.3 in /opt/conda/lib/python3.7/site-packages (from pandas>=0.24.0->catboost) (2.8.1)
    Requirement already satisfied: pytz>=2017.3 in /opt/conda/lib/python3.7/site-packages (from pandas>=0.24.0->catboost) (2021.1)
    Requirement already satisfied: pillow>=6.2.0 in /opt/conda/lib/python3.7/site-packages (from matplotlib->catboost) (7.2.0)
    Requirement already satisfied: pyparsing>=2.2.1 in /opt/conda/lib/python3.7/site-packages (from matplotlib->catboost) (2.4.7)
    Requirement already satisfied: kiwisolver>=1.0.1 in /opt/conda/lib/python3.7/site-packages (from matplotlib->catboost) (1.3.1)
    Requirement already satisfied: cycler>=0.10 in /opt/conda/lib/python3.7/site-packages (from matplotlib->catboost) (0.10.0)
    Requirement already satisfied: retrying>=1.3.3 in /opt/conda/lib/python3.7/site-packages (from plotly->catboost) (1.3.3)
    


```python
%%time

from catboost import CatBoostClassifier

cb_params = {
    'max_depth': 8,
    'learning_rate': 0.01,
    'n_estimators': 1000,
    'max_bin': 280,
    'min_data_in_leaf': 64,
    'l2_leaf_reg': 0.01,
    'subsample': 0.8
}

cb_model=sk_helper(model=CatBoostClassifier, seed=SEED, params=cb_params)
cb_model.roc_curve_graph(X_train, y_train, X_valid, y_valid)
```

    0:	learn: 0.6884406	total: 80.5ms	remaining: 1m 20s
    1:	learn: 0.6838170	total: 104ms	remaining: 51.9s
    2:	learn: 0.6792866	total: 120ms	remaining: 40s
    3:	learn: 0.6749307	total: 144ms	remaining: 35.9s
    4:	learn: 0.6705501	total: 168ms	remaining: 33.4s
    5:	learn: 0.6662601	total: 192ms	remaining: 31.8s
    6:	learn: 0.6621476	total: 214ms	remaining: 30.4s
    7:	learn: 0.6580493	total: 238ms	remaining: 29.5s
    8:	learn: 0.6541011	total: 260ms	remaining: 28.6s
    9:	learn: 0.6502757	total: 276ms	remaining: 27.3s
    10:	learn: 0.6465377	total: 302ms	remaining: 27.1s
    11:	learn: 0.6427935	total: 325ms	remaining: 26.8s
    12:	learn: 0.6391145	total: 348ms	remaining: 26.4s
    13:	learn: 0.6356544	total: 369ms	remaining: 26s
    14:	learn: 0.6321140	total: 393ms	remaining: 25.8s
    15:	learn: 0.6287126	total: 417ms	remaining: 25.6s
    16:	learn: 0.6255104	total: 436ms	remaining: 25.2s
    17:	learn: 0.6222667	total: 458ms	remaining: 25s
    18:	learn: 0.6190328	total: 484ms	remaining: 25s
    19:	learn: 0.6160637	total: 498ms	remaining: 24.4s
    20:	learn: 0.6130081	total: 532ms	remaining: 24.8s
    21:	learn: 0.6101029	total: 553ms	remaining: 24.6s
    22:	learn: 0.6072655	total: 578ms	remaining: 24.6s
    23:	learn: 0.6043829	total: 602ms	remaining: 24.5s
    24:	learn: 0.6018266	total: 620ms	remaining: 24.2s
    25:	learn: 0.5990827	total: 644ms	remaining: 24.1s
    26:	learn: 0.5964782	total: 668ms	remaining: 24.1s
    27:	learn: 0.5939595	total: 688ms	remaining: 23.9s
    28:	learn: 0.5915331	total: 704ms	remaining: 23.6s
    29:	learn: 0.5889863	total: 728ms	remaining: 23.5s
    30:	learn: 0.5865869	total: 753ms	remaining: 23.5s
    31:	learn: 0.5842859	total: 776ms	remaining: 23.5s
    32:	learn: 0.5819020	total: 801ms	remaining: 23.5s
    33:	learn: 0.5797240	total: 825ms	remaining: 23.4s
    34:	learn: 0.5775389	total: 848ms	remaining: 23.4s
    35:	learn: 0.5755076	total: 864ms	remaining: 23.1s
    36:	learn: 0.5736566	total: 878ms	remaining: 22.9s
    37:	learn: 0.5717165	total: 902ms	remaining: 22.8s
    38:	learn: 0.5696527	total: 925ms	remaining: 22.8s
    39:	learn: 0.5677411	total: 947ms	remaining: 22.7s
    40:	learn: 0.5658559	total: 971ms	remaining: 22.7s
    41:	learn: 0.5639679	total: 994ms	remaining: 22.7s
    42:	learn: 0.5621573	total: 1.02s	remaining: 22.6s
    43:	learn: 0.5603957	total: 1.04s	remaining: 22.6s
    44:	learn: 0.5585892	total: 1.07s	remaining: 22.7s
    45:	learn: 0.5569204	total: 1.09s	remaining: 22.6s
    46:	learn: 0.5552463	total: 1.11s	remaining: 22.6s
    47:	learn: 0.5536250	total: 1.14s	remaining: 22.6s
    48:	learn: 0.5520718	total: 1.16s	remaining: 22.5s
    49:	learn: 0.5504821	total: 1.19s	remaining: 22.6s
    50:	learn: 0.5489653	total: 1.21s	remaining: 22.6s
    51:	learn: 0.5474109	total: 1.24s	remaining: 22.6s
    52:	learn: 0.5459763	total: 1.26s	remaining: 22.6s
    53:	learn: 0.5445562	total: 1.29s	remaining: 22.6s
    54:	learn: 0.5431438	total: 1.31s	remaining: 22.6s
    55:	learn: 0.5417719	total: 1.34s	remaining: 22.6s
    56:	learn: 0.5404477	total: 1.36s	remaining: 22.6s
    57:	learn: 0.5391074	total: 1.39s	remaining: 22.5s
    58:	learn: 0.5377986	total: 1.41s	remaining: 22.5s
    59:	learn: 0.5365282	total: 1.44s	remaining: 22.5s
    60:	learn: 0.5353029	total: 1.46s	remaining: 22.5s
    61:	learn: 0.5340683	total: 1.48s	remaining: 22.4s
    62:	learn: 0.5329104	total: 1.51s	remaining: 22.4s
    63:	learn: 0.5317566	total: 1.53s	remaining: 22.4s
    64:	learn: 0.5306147	total: 1.55s	remaining: 22.4s
    65:	learn: 0.5294422	total: 1.58s	remaining: 22.4s
    66:	learn: 0.5283472	total: 1.6s	remaining: 22.4s
    67:	learn: 0.5272628	total: 1.63s	remaining: 22.4s
    68:	learn: 0.5262269	total: 1.66s	remaining: 22.4s
    69:	learn: 0.5252543	total: 1.68s	remaining: 22.3s
    70:	learn: 0.5242217	total: 1.71s	remaining: 22.3s
    71:	learn: 0.5232597	total: 1.73s	remaining: 22.3s
    72:	learn: 0.5222463	total: 1.76s	remaining: 22.3s
    73:	learn: 0.5213520	total: 1.78s	remaining: 22.3s
    74:	learn: 0.5204568	total: 1.8s	remaining: 22.3s
    75:	learn: 0.5196299	total: 1.83s	remaining: 22.2s
    76:	learn: 0.5187603	total: 1.85s	remaining: 22.2s
    77:	learn: 0.5179326	total: 1.88s	remaining: 22.2s
    78:	learn: 0.5170868	total: 1.9s	remaining: 22.2s
    79:	learn: 0.5162588	total: 1.92s	remaining: 22.1s
    80:	learn: 0.5154756	total: 1.95s	remaining: 22.1s
    81:	learn: 0.5146912	total: 1.97s	remaining: 22.1s
    82:	learn: 0.5139581	total: 2s	remaining: 22.1s
    83:	learn: 0.5131996	total: 2.02s	remaining: 22.1s
    84:	learn: 0.5124481	total: 2.04s	remaining: 22s
    85:	learn: 0.5116752	total: 2.07s	remaining: 22s
    86:	learn: 0.5109482	total: 2.1s	remaining: 22s
    87:	learn: 0.5103112	total: 2.12s	remaining: 22s
    88:	learn: 0.5096581	total: 2.15s	remaining: 22s
    89:	learn: 0.5089431	total: 2.18s	remaining: 22s
    90:	learn: 0.5082522	total: 2.2s	remaining: 22s
    91:	learn: 0.5076517	total: 2.23s	remaining: 22s
    92:	learn: 0.5070308	total: 2.25s	remaining: 21.9s
    93:	learn: 0.5063997	total: 2.27s	remaining: 21.9s
    94:	learn: 0.5058001	total: 2.3s	remaining: 21.9s
    95:	learn: 0.5051906	total: 2.33s	remaining: 21.9s
    96:	learn: 0.5046129	total: 2.35s	remaining: 21.9s
    97:	learn: 0.5040508	total: 2.38s	remaining: 21.9s
    98:	learn: 0.5034705	total: 2.4s	remaining: 21.9s
    99:	learn: 0.5029049	total: 2.42s	remaining: 21.8s
    100:	learn: 0.5023410	total: 2.45s	remaining: 21.8s
    101:	learn: 0.5018054	total: 2.47s	remaining: 21.8s
    102:	learn: 0.5012927	total: 2.5s	remaining: 21.8s
    103:	learn: 0.5007620	total: 2.53s	remaining: 21.8s
    104:	learn: 0.5002364	total: 2.55s	remaining: 21.8s
    105:	learn: 0.4997446	total: 2.58s	remaining: 21.7s
    106:	learn: 0.4992382	total: 2.6s	remaining: 21.7s
    107:	learn: 0.4987651	total: 2.63s	remaining: 21.7s
    108:	learn: 0.4983881	total: 2.64s	remaining: 21.6s
    109:	learn: 0.4979742	total: 2.67s	remaining: 21.6s
    110:	learn: 0.4975065	total: 2.7s	remaining: 21.6s
    111:	learn: 0.4970813	total: 2.73s	remaining: 21.6s
    112:	learn: 0.4966415	total: 2.75s	remaining: 21.6s
    113:	learn: 0.4962375	total: 2.77s	remaining: 21.6s
    114:	learn: 0.4958272	total: 2.8s	remaining: 21.5s
    115:	learn: 0.4954165	total: 2.82s	remaining: 21.5s
    116:	learn: 0.4950403	total: 2.85s	remaining: 21.5s
    117:	learn: 0.4946756	total: 2.87s	remaining: 21.5s
    118:	learn: 0.4943074	total: 2.9s	remaining: 21.4s
    119:	learn: 0.4939117	total: 2.92s	remaining: 21.4s
    120:	learn: 0.4935388	total: 2.95s	remaining: 21.4s
    121:	learn: 0.4932137	total: 2.97s	remaining: 21.4s
    122:	learn: 0.4928860	total: 3s	remaining: 21.4s
    123:	learn: 0.4925254	total: 3.02s	remaining: 21.4s
    124:	learn: 0.4922541	total: 3.04s	remaining: 21.3s
    125:	learn: 0.4919255	total: 3.06s	remaining: 21.2s
    126:	learn: 0.4915802	total: 3.08s	remaining: 21.2s
    127:	learn: 0.4912600	total: 3.11s	remaining: 21.2s
    128:	learn: 0.4909621	total: 3.13s	remaining: 21.2s
    129:	learn: 0.4906550	total: 3.16s	remaining: 21.2s
    130:	learn: 0.4903320	total: 3.19s	remaining: 21.1s
    131:	learn: 0.4900083	total: 3.21s	remaining: 21.1s
    132:	learn: 0.4896774	total: 3.24s	remaining: 21.1s
    133:	learn: 0.4893788	total: 3.26s	remaining: 21.1s
    134:	learn: 0.4890677	total: 3.29s	remaining: 21.1s
    135:	learn: 0.4887482	total: 3.31s	remaining: 21s
    136:	learn: 0.4884599	total: 3.34s	remaining: 21s
    137:	learn: 0.4881724	total: 3.37s	remaining: 21s
    138:	learn: 0.4878845	total: 3.39s	remaining: 21s
    139:	learn: 0.4876288	total: 3.42s	remaining: 21s
    140:	learn: 0.4873738	total: 3.44s	remaining: 21s
    141:	learn: 0.4871044	total: 3.47s	remaining: 20.9s
    142:	learn: 0.4868465	total: 3.5s	remaining: 21s
    143:	learn: 0.4865916	total: 3.53s	remaining: 21s
    144:	learn: 0.4863548	total: 3.57s	remaining: 21.1s
    145:	learn: 0.4860985	total: 3.6s	remaining: 21.1s
    146:	learn: 0.4858511	total: 3.63s	remaining: 21.1s
    147:	learn: 0.4856274	total: 3.65s	remaining: 21s
    148:	learn: 0.4853693	total: 3.68s	remaining: 21s
    149:	learn: 0.4851541	total: 3.7s	remaining: 21s
    150:	learn: 0.4849915	total: 3.72s	remaining: 20.9s
    151:	learn: 0.4847782	total: 3.74s	remaining: 20.9s
    152:	learn: 0.4845270	total: 3.77s	remaining: 20.9s
    153:	learn: 0.4842832	total: 3.8s	remaining: 20.9s
    154:	learn: 0.4840638	total: 3.82s	remaining: 20.8s
    155:	learn: 0.4838815	total: 3.85s	remaining: 20.8s
    156:	learn: 0.4836920	total: 3.87s	remaining: 20.8s
    157:	learn: 0.4835030	total: 3.9s	remaining: 20.8s
    158:	learn: 0.4832918	total: 3.92s	remaining: 20.7s
    159:	learn: 0.4831511	total: 3.93s	remaining: 20.7s
    160:	learn: 0.4829497	total: 3.96s	remaining: 20.6s
    161:	learn: 0.4827754	total: 3.98s	remaining: 20.6s
    162:	learn: 0.4825804	total: 4.01s	remaining: 20.6s
    163:	learn: 0.4823916	total: 4.04s	remaining: 20.6s
    164:	learn: 0.4822085	total: 4.06s	remaining: 20.5s
    165:	learn: 0.4820429	total: 4.08s	remaining: 20.5s
    166:	learn: 0.4818731	total: 4.11s	remaining: 20.5s
    167:	learn: 0.4817004	total: 4.13s	remaining: 20.5s
    168:	learn: 0.4815285	total: 4.16s	remaining: 20.4s
    169:	learn: 0.4813635	total: 4.18s	remaining: 20.4s
    170:	learn: 0.4812079	total: 4.2s	remaining: 20.4s
    171:	learn: 0.4810382	total: 4.23s	remaining: 20.4s
    172:	learn: 0.4808926	total: 4.25s	remaining: 20.3s
    173:	learn: 0.4807381	total: 4.28s	remaining: 20.3s
    174:	learn: 0.4805700	total: 4.3s	remaining: 20.3s
    175:	learn: 0.4804400	total: 4.32s	remaining: 20.2s
    176:	learn: 0.4802812	total: 4.35s	remaining: 20.2s
    177:	learn: 0.4801280	total: 4.37s	remaining: 20.2s
    178:	learn: 0.4799842	total: 4.39s	remaining: 20.2s
    179:	learn: 0.4798398	total: 4.42s	remaining: 20.1s
    180:	learn: 0.4797125	total: 4.44s	remaining: 20.1s
    181:	learn: 0.4795689	total: 4.47s	remaining: 20.1s
    182:	learn: 0.4794479	total: 4.49s	remaining: 20s
    183:	learn: 0.4792997	total: 4.51s	remaining: 20s
    184:	learn: 0.4791614	total: 4.54s	remaining: 20s
    185:	learn: 0.4790325	total: 4.56s	remaining: 20s
    186:	learn: 0.4789103	total: 4.59s	remaining: 19.9s
    187:	learn: 0.4787746	total: 4.61s	remaining: 19.9s
    188:	learn: 0.4786240	total: 4.64s	remaining: 19.9s
    189:	learn: 0.4784893	total: 4.66s	remaining: 19.9s
    190:	learn: 0.4783515	total: 4.69s	remaining: 19.9s
    191:	learn: 0.4782205	total: 4.71s	remaining: 19.8s
    192:	learn: 0.4780959	total: 4.74s	remaining: 19.8s
    193:	learn: 0.4779926	total: 4.76s	remaining: 19.8s
    194:	learn: 0.4778871	total: 4.78s	remaining: 19.7s
    195:	learn: 0.4777654	total: 4.81s	remaining: 19.7s
    196:	learn: 0.4776578	total: 4.83s	remaining: 19.7s
    197:	learn: 0.4775341	total: 4.85s	remaining: 19.7s
    198:	learn: 0.4773985	total: 4.88s	remaining: 19.6s
    199:	learn: 0.4772849	total: 4.91s	remaining: 19.6s
    200:	learn: 0.4771735	total: 4.93s	remaining: 19.6s
    201:	learn: 0.4770525	total: 4.95s	remaining: 19.6s
    202:	learn: 0.4769315	total: 4.98s	remaining: 19.5s
    203:	learn: 0.4768213	total: 5s	remaining: 19.5s
    204:	learn: 0.4767196	total: 5.03s	remaining: 19.5s
    205:	learn: 0.4766273	total: 5.05s	remaining: 19.5s
    206:	learn: 0.4765335	total: 5.07s	remaining: 19.4s
    207:	learn: 0.4764282	total: 5.1s	remaining: 19.4s
    208:	learn: 0.4763482	total: 5.12s	remaining: 19.4s
    209:	learn: 0.4762623	total: 5.15s	remaining: 19.4s
    210:	learn: 0.4761735	total: 5.17s	remaining: 19.3s
    211:	learn: 0.4760733	total: 5.19s	remaining: 19.3s
    212:	learn: 0.4759979	total: 5.22s	remaining: 19.3s
    213:	learn: 0.4758779	total: 5.24s	remaining: 19.3s
    214:	learn: 0.4757827	total: 5.26s	remaining: 19.2s
    215:	learn: 0.4757053	total: 5.29s	remaining: 19.2s
    216:	learn: 0.4756008	total: 5.32s	remaining: 19.2s
    217:	learn: 0.4755150	total: 5.34s	remaining: 19.2s
    218:	learn: 0.4754208	total: 5.36s	remaining: 19.1s
    219:	learn: 0.4753210	total: 5.39s	remaining: 19.1s
    220:	learn: 0.4752463	total: 5.41s	remaining: 19.1s
    221:	learn: 0.4751665	total: 5.44s	remaining: 19.1s
    222:	learn: 0.4750788	total: 5.46s	remaining: 19s
    223:	learn: 0.4749864	total: 5.49s	remaining: 19s
    224:	learn: 0.4748725	total: 5.51s	remaining: 19s
    225:	learn: 0.4747832	total: 5.54s	remaining: 19s
    226:	learn: 0.4747065	total: 5.56s	remaining: 18.9s
    227:	learn: 0.4746132	total: 5.59s	remaining: 18.9s
    228:	learn: 0.4745160	total: 5.61s	remaining: 18.9s
    229:	learn: 0.4744390	total: 5.64s	remaining: 18.9s
    230:	learn: 0.4743604	total: 5.66s	remaining: 18.9s
    231:	learn: 0.4742583	total: 5.69s	remaining: 18.8s
    232:	learn: 0.4741847	total: 5.71s	remaining: 18.8s
    233:	learn: 0.4740959	total: 5.74s	remaining: 18.8s
    234:	learn: 0.4740094	total: 5.76s	remaining: 18.8s
    235:	learn: 0.4739407	total: 5.79s	remaining: 18.7s
    236:	learn: 0.4738660	total: 5.81s	remaining: 18.7s
    237:	learn: 0.4737837	total: 5.84s	remaining: 18.7s
    238:	learn: 0.4737104	total: 5.86s	remaining: 18.7s
    239:	learn: 0.4736311	total: 5.89s	remaining: 18.6s
    240:	learn: 0.4735539	total: 5.91s	remaining: 18.6s
    241:	learn: 0.4734807	total: 5.94s	remaining: 18.6s
    242:	learn: 0.4734141	total: 5.96s	remaining: 18.6s
    243:	learn: 0.4733445	total: 5.99s	remaining: 18.6s
    244:	learn: 0.4732736	total: 6.01s	remaining: 18.5s
    245:	learn: 0.4731923	total: 6.04s	remaining: 18.5s
    246:	learn: 0.4731118	total: 6.06s	remaining: 18.5s
    247:	learn: 0.4730604	total: 6.08s	remaining: 18.5s
    248:	learn: 0.4729942	total: 6.13s	remaining: 18.5s
    249:	learn: 0.4729268	total: 6.23s	remaining: 18.7s
    250:	learn: 0.4728592	total: 6.29s	remaining: 18.8s
    251:	learn: 0.4727932	total: 6.32s	remaining: 18.7s
    252:	learn: 0.4727464	total: 6.34s	remaining: 18.7s
    253:	learn: 0.4726803	total: 6.37s	remaining: 18.7s
    254:	learn: 0.4726170	total: 6.39s	remaining: 18.7s
    255:	learn: 0.4725995	total: 6.4s	remaining: 18.6s
    256:	learn: 0.4725542	total: 6.43s	remaining: 18.6s
    257:	learn: 0.4724986	total: 6.45s	remaining: 18.6s
    258:	learn: 0.4724363	total: 6.48s	remaining: 18.5s
    259:	learn: 0.4723640	total: 6.5s	remaining: 18.5s
    260:	learn: 0.4723009	total: 6.53s	remaining: 18.5s
    261:	learn: 0.4722426	total: 6.55s	remaining: 18.5s
    262:	learn: 0.4721912	total: 6.58s	remaining: 18.4s
    263:	learn: 0.4721549	total: 6.6s	remaining: 18.4s
    264:	learn: 0.4720826	total: 6.63s	remaining: 18.4s
    265:	learn: 0.4720172	total: 6.65s	remaining: 18.3s
    266:	learn: 0.4719535	total: 6.67s	remaining: 18.3s
    267:	learn: 0.4719023	total: 6.7s	remaining: 18.3s
    268:	learn: 0.4718534	total: 6.72s	remaining: 18.3s
    269:	learn: 0.4717885	total: 6.74s	remaining: 18.2s
    270:	learn: 0.4717176	total: 6.77s	remaining: 18.2s
    271:	learn: 0.4716617	total: 6.79s	remaining: 18.2s
    272:	learn: 0.4715830	total: 6.82s	remaining: 18.2s
    273:	learn: 0.4715357	total: 6.84s	remaining: 18.1s
    274:	learn: 0.4714868	total: 6.87s	remaining: 18.1s
    275:	learn: 0.4714388	total: 6.89s	remaining: 18.1s
    276:	learn: 0.4713894	total: 6.91s	remaining: 18s
    277:	learn: 0.4713289	total: 6.94s	remaining: 18s
    278:	learn: 0.4712782	total: 6.96s	remaining: 18s
    279:	learn: 0.4712199	total: 7s	remaining: 18s
    280:	learn: 0.4711798	total: 7.03s	remaining: 18s
    281:	learn: 0.4711284	total: 7.06s	remaining: 18s
    282:	learn: 0.4710726	total: 7.09s	remaining: 18s
    283:	learn: 0.4710250	total: 7.13s	remaining: 18s
    284:	learn: 0.4709614	total: 7.16s	remaining: 18s
    285:	learn: 0.4709017	total: 7.19s	remaining: 18s
    286:	learn: 0.4708453	total: 7.22s	remaining: 17.9s
    287:	learn: 0.4707988	total: 7.25s	remaining: 17.9s
    288:	learn: 0.4707513	total: 7.28s	remaining: 17.9s
    289:	learn: 0.4706960	total: 7.32s	remaining: 17.9s
    290:	learn: 0.4706629	total: 7.35s	remaining: 17.9s
    291:	learn: 0.4706212	total: 7.38s	remaining: 17.9s
    292:	learn: 0.4705679	total: 7.41s	remaining: 17.9s
    293:	learn: 0.4705116	total: 7.45s	remaining: 17.9s
    294:	learn: 0.4704656	total: 7.48s	remaining: 17.9s
    295:	learn: 0.4704126	total: 7.51s	remaining: 17.9s
    296:	learn: 0.4703708	total: 7.54s	remaining: 17.9s
    297:	learn: 0.4703217	total: 7.57s	remaining: 17.8s
    298:	learn: 0.4702804	total: 7.6s	remaining: 17.8s
    299:	learn: 0.4702280	total: 7.63s	remaining: 17.8s
    300:	learn: 0.4701886	total: 7.66s	remaining: 17.8s
    301:	learn: 0.4701525	total: 7.69s	remaining: 17.8s
    302:	learn: 0.4701154	total: 7.71s	remaining: 17.7s
    303:	learn: 0.4700790	total: 7.75s	remaining: 17.7s
    304:	learn: 0.4700324	total: 7.78s	remaining: 17.7s
    305:	learn: 0.4699747	total: 7.81s	remaining: 17.7s
    306:	learn: 0.4699302	total: 7.83s	remaining: 17.7s
    307:	learn: 0.4698923	total: 7.86s	remaining: 17.7s
    308:	learn: 0.4698518	total: 7.89s	remaining: 17.6s
    309:	learn: 0.4698233	total: 7.92s	remaining: 17.6s
    310:	learn: 0.4697744	total: 7.95s	remaining: 17.6s
    311:	learn: 0.4697332	total: 7.98s	remaining: 17.6s
    312:	learn: 0.4696965	total: 8.01s	remaining: 17.6s
    313:	learn: 0.4696554	total: 8.03s	remaining: 17.6s
    314:	learn: 0.4696063	total: 8.06s	remaining: 17.5s
    315:	learn: 0.4695670	total: 8.09s	remaining: 17.5s
    316:	learn: 0.4695291	total: 8.12s	remaining: 17.5s
    317:	learn: 0.4694878	total: 8.15s	remaining: 17.5s
    318:	learn: 0.4694437	total: 8.17s	remaining: 17.4s
    319:	learn: 0.4694075	total: 8.2s	remaining: 17.4s
    320:	learn: 0.4693653	total: 8.22s	remaining: 17.4s
    321:	learn: 0.4693186	total: 8.24s	remaining: 17.4s
    322:	learn: 0.4692856	total: 8.27s	remaining: 17.3s
    323:	learn: 0.4692345	total: 8.29s	remaining: 17.3s
    324:	learn: 0.4692009	total: 8.31s	remaining: 17.3s
    325:	learn: 0.4691615	total: 8.34s	remaining: 17.2s
    326:	learn: 0.4691261	total: 8.36s	remaining: 17.2s
    327:	learn: 0.4690797	total: 8.38s	remaining: 17.2s
    328:	learn: 0.4690424	total: 8.41s	remaining: 17.1s
    329:	learn: 0.4689856	total: 8.43s	remaining: 17.1s
    330:	learn: 0.4689386	total: 8.45s	remaining: 17.1s
    331:	learn: 0.4689094	total: 8.47s	remaining: 17.1s
    332:	learn: 0.4688729	total: 8.5s	remaining: 17s
    333:	learn: 0.4688315	total: 8.52s	remaining: 17s
    334:	learn: 0.4687920	total: 8.55s	remaining: 17s
    335:	learn: 0.4687552	total: 8.57s	remaining: 16.9s
    336:	learn: 0.4687154	total: 8.6s	remaining: 16.9s
    337:	learn: 0.4686667	total: 8.62s	remaining: 16.9s
    338:	learn: 0.4686390	total: 8.65s	remaining: 16.9s
    339:	learn: 0.4685975	total: 8.67s	remaining: 16.8s
    340:	learn: 0.4685549	total: 8.69s	remaining: 16.8s
    341:	learn: 0.4685488	total: 8.7s	remaining: 16.7s
    342:	learn: 0.4685116	total: 8.73s	remaining: 16.7s
    343:	learn: 0.4684789	total: 8.75s	remaining: 16.7s
    344:	learn: 0.4684390	total: 8.77s	remaining: 16.7s
    345:	learn: 0.4683998	total: 8.8s	remaining: 16.6s
    346:	learn: 0.4683660	total: 8.82s	remaining: 16.6s
    347:	learn: 0.4683321	total: 8.85s	remaining: 16.6s
    348:	learn: 0.4682982	total: 8.87s	remaining: 16.5s
    349:	learn: 0.4682504	total: 8.89s	remaining: 16.5s
    350:	learn: 0.4682185	total: 8.91s	remaining: 16.5s
    351:	learn: 0.4681818	total: 8.94s	remaining: 16.5s
    352:	learn: 0.4681308	total: 8.96s	remaining: 16.4s
    353:	learn: 0.4680877	total: 8.99s	remaining: 16.4s
    354:	learn: 0.4680679	total: 9.01s	remaining: 16.4s
    355:	learn: 0.4680274	total: 9.03s	remaining: 16.3s
    356:	learn: 0.4679898	total: 9.05s	remaining: 16.3s
    357:	learn: 0.4679634	total: 9.08s	remaining: 16.3s
    358:	learn: 0.4679364	total: 9.1s	remaining: 16.3s
    359:	learn: 0.4679083	total: 9.13s	remaining: 16.2s
    360:	learn: 0.4678759	total: 9.15s	remaining: 16.2s
    361:	learn: 0.4678355	total: 9.17s	remaining: 16.2s
    362:	learn: 0.4677858	total: 9.2s	remaining: 16.1s
    363:	learn: 0.4677275	total: 9.22s	remaining: 16.1s
    364:	learn: 0.4676922	total: 9.25s	remaining: 16.1s
    365:	learn: 0.4676488	total: 9.27s	remaining: 16.1s
    366:	learn: 0.4676148	total: 9.29s	remaining: 16s
    367:	learn: 0.4675839	total: 9.32s	remaining: 16s
    368:	learn: 0.4675372	total: 9.34s	remaining: 16s
    369:	learn: 0.4675135	total: 9.36s	remaining: 15.9s
    370:	learn: 0.4674817	total: 9.39s	remaining: 15.9s
    371:	learn: 0.4674287	total: 9.41s	remaining: 15.9s
    372:	learn: 0.4674151	total: 9.43s	remaining: 15.9s
    373:	learn: 0.4673884	total: 9.45s	remaining: 15.8s
    374:	learn: 0.4673525	total: 9.48s	remaining: 15.8s
    375:	learn: 0.4673205	total: 9.5s	remaining: 15.8s
    376:	learn: 0.4672887	total: 9.52s	remaining: 15.7s
    377:	learn: 0.4672546	total: 9.55s	remaining: 15.7s
    378:	learn: 0.4672294	total: 9.57s	remaining: 15.7s
    379:	learn: 0.4671776	total: 9.6s	remaining: 15.7s
    380:	learn: 0.4671438	total: 9.62s	remaining: 15.6s
    381:	learn: 0.4670930	total: 9.65s	remaining: 15.6s
    382:	learn: 0.4670640	total: 9.67s	remaining: 15.6s
    383:	learn: 0.4670421	total: 9.69s	remaining: 15.5s
    384:	learn: 0.4670226	total: 9.71s	remaining: 15.5s
    385:	learn: 0.4669924	total: 9.74s	remaining: 15.5s
    386:	learn: 0.4669590	total: 9.76s	remaining: 15.5s
    387:	learn: 0.4669249	total: 9.78s	remaining: 15.4s
    388:	learn: 0.4668914	total: 9.81s	remaining: 15.4s
    389:	learn: 0.4668551	total: 9.84s	remaining: 15.4s
    390:	learn: 0.4667978	total: 9.86s	remaining: 15.4s
    391:	learn: 0.4667640	total: 9.88s	remaining: 15.3s
    392:	learn: 0.4667328	total: 9.91s	remaining: 15.3s
    393:	learn: 0.4667057	total: 9.93s	remaining: 15.3s
    394:	learn: 0.4666729	total: 9.96s	remaining: 15.2s
    395:	learn: 0.4666403	total: 9.98s	remaining: 15.2s
    396:	learn: 0.4666114	total: 10s	remaining: 15.2s
    397:	learn: 0.4665753	total: 10s	remaining: 15.2s
    398:	learn: 0.4665430	total: 10.1s	remaining: 15.1s
    399:	learn: 0.4665105	total: 10.1s	remaining: 15.1s
    400:	learn: 0.4664574	total: 10.1s	remaining: 15.1s
    401:	learn: 0.4664287	total: 10.1s	remaining: 15.1s
    402:	learn: 0.4664050	total: 10.1s	remaining: 15s
    403:	learn: 0.4663821	total: 10.2s	remaining: 15s
    404:	learn: 0.4663643	total: 10.2s	remaining: 15s
    405:	learn: 0.4663232	total: 10.2s	remaining: 14.9s
    406:	learn: 0.4662958	total: 10.2s	remaining: 14.9s
    407:	learn: 0.4662706	total: 10.3s	remaining: 14.9s
    408:	learn: 0.4662425	total: 10.3s	remaining: 14.9s
    409:	learn: 0.4661981	total: 10.3s	remaining: 14.8s
    410:	learn: 0.4661695	total: 10.3s	remaining: 14.8s
    411:	learn: 0.4661311	total: 10.4s	remaining: 14.8s
    412:	learn: 0.4661092	total: 10.4s	remaining: 14.7s
    413:	learn: 0.4660792	total: 10.4s	remaining: 14.7s
    414:	learn: 0.4660495	total: 10.4s	remaining: 14.7s
    415:	learn: 0.4660280	total: 10.4s	remaining: 14.7s
    416:	learn: 0.4659799	total: 10.5s	remaining: 14.6s
    417:	learn: 0.4659521	total: 10.5s	remaining: 14.6s
    418:	learn: 0.4659136	total: 10.5s	remaining: 14.6s
    419:	learn: 0.4658956	total: 10.5s	remaining: 14.6s
    420:	learn: 0.4658665	total: 10.6s	remaining: 14.5s
    421:	learn: 0.4658280	total: 10.6s	remaining: 14.5s
    422:	learn: 0.4657990	total: 10.6s	remaining: 14.5s
    423:	learn: 0.4657658	total: 10.6s	remaining: 14.4s
    424:	learn: 0.4657396	total: 10.7s	remaining: 14.4s
    425:	learn: 0.4657056	total: 10.7s	remaining: 14.4s
    426:	learn: 0.4656819	total: 10.7s	remaining: 14.4s
    427:	learn: 0.4656492	total: 10.7s	remaining: 14.3s
    428:	learn: 0.4656146	total: 10.8s	remaining: 14.3s
    429:	learn: 0.4656093	total: 10.8s	remaining: 14.3s
    430:	learn: 0.4655778	total: 10.8s	remaining: 14.2s
    431:	learn: 0.4655458	total: 10.8s	remaining: 14.2s
    432:	learn: 0.4655261	total: 10.8s	remaining: 14.2s
    433:	learn: 0.4655002	total: 10.9s	remaining: 14.2s
    434:	learn: 0.4654702	total: 10.9s	remaining: 14.1s
    435:	learn: 0.4654481	total: 10.9s	remaining: 14.1s
    436:	learn: 0.4654259	total: 10.9s	remaining: 14.1s
    437:	learn: 0.4654023	total: 10.9s	remaining: 14s
    438:	learn: 0.4653630	total: 11s	remaining: 14s
    439:	learn: 0.4653424	total: 11s	remaining: 14s
    440:	learn: 0.4653026	total: 11s	remaining: 14s
    441:	learn: 0.4652672	total: 11s	remaining: 13.9s
    442:	learn: 0.4652472	total: 11.1s	remaining: 13.9s
    443:	learn: 0.4652149	total: 11.1s	remaining: 13.9s
    444:	learn: 0.4651717	total: 11.1s	remaining: 13.9s
    445:	learn: 0.4651446	total: 11.1s	remaining: 13.8s
    446:	learn: 0.4651118	total: 11.2s	remaining: 13.8s
    447:	learn: 0.4650817	total: 11.2s	remaining: 13.8s
    448:	learn: 0.4650518	total: 11.2s	remaining: 13.7s
    449:	learn: 0.4650259	total: 11.2s	remaining: 13.7s
    450:	learn: 0.4649924	total: 11.3s	remaining: 13.7s
    451:	learn: 0.4649556	total: 11.3s	remaining: 13.7s
    452:	learn: 0.4649371	total: 11.3s	remaining: 13.6s
    453:	learn: 0.4649141	total: 11.3s	remaining: 13.6s
    454:	learn: 0.4648867	total: 11.3s	remaining: 13.6s
    455:	learn: 0.4648588	total: 11.4s	remaining: 13.6s
    456:	learn: 0.4648250	total: 11.4s	remaining: 13.5s
    457:	learn: 0.4647940	total: 11.4s	remaining: 13.5s
    458:	learn: 0.4647546	total: 11.4s	remaining: 13.5s
    459:	learn: 0.4647189	total: 11.5s	remaining: 13.5s
    460:	learn: 0.4646901	total: 11.5s	remaining: 13.4s
    461:	learn: 0.4646688	total: 11.5s	remaining: 13.4s
    462:	learn: 0.4646478	total: 11.5s	remaining: 13.4s
    463:	learn: 0.4646157	total: 11.6s	remaining: 13.3s
    464:	learn: 0.4645898	total: 11.6s	remaining: 13.3s
    465:	learn: 0.4645564	total: 11.6s	remaining: 13.3s
    466:	learn: 0.4645328	total: 11.6s	remaining: 13.3s
    467:	learn: 0.4645155	total: 11.6s	remaining: 13.2s
    468:	learn: 0.4644996	total: 11.7s	remaining: 13.2s
    469:	learn: 0.4644666	total: 11.7s	remaining: 13.2s
    470:	learn: 0.4644513	total: 11.7s	remaining: 13.2s
    471:	learn: 0.4644258	total: 11.7s	remaining: 13.1s
    472:	learn: 0.4644093	total: 11.8s	remaining: 13.1s
    473:	learn: 0.4643853	total: 11.8s	remaining: 13.1s
    474:	learn: 0.4643599	total: 11.8s	remaining: 13.1s
    475:	learn: 0.4643203	total: 11.8s	remaining: 13s
    476:	learn: 0.4642956	total: 11.9s	remaining: 13s
    477:	learn: 0.4642900	total: 11.9s	remaining: 13s
    478:	learn: 0.4642673	total: 11.9s	remaining: 12.9s
    479:	learn: 0.4642508	total: 11.9s	remaining: 12.9s
    480:	learn: 0.4642142	total: 11.9s	remaining: 12.9s
    481:	learn: 0.4641946	total: 12s	remaining: 12.9s
    482:	learn: 0.4641754	total: 12s	remaining: 12.8s
    483:	learn: 0.4641545	total: 12s	remaining: 12.8s
    484:	learn: 0.4641318	total: 12s	remaining: 12.8s
    485:	learn: 0.4641079	total: 12.1s	remaining: 12.7s
    486:	learn: 0.4640660	total: 12.1s	remaining: 12.7s
    487:	learn: 0.4640376	total: 12.1s	remaining: 12.7s
    488:	learn: 0.4640081	total: 12.1s	remaining: 12.7s
    489:	learn: 0.4639916	total: 12.1s	remaining: 12.6s
    490:	learn: 0.4639589	total: 12.2s	remaining: 12.6s
    491:	learn: 0.4639303	total: 12.2s	remaining: 12.6s
    492:	learn: 0.4639081	total: 12.2s	remaining: 12.6s
    493:	learn: 0.4638912	total: 12.2s	remaining: 12.5s
    494:	learn: 0.4638710	total: 12.3s	remaining: 12.5s
    495:	learn: 0.4638476	total: 12.3s	remaining: 12.5s
    496:	learn: 0.4638259	total: 12.3s	remaining: 12.5s
    497:	learn: 0.4637943	total: 12.3s	remaining: 12.4s
    498:	learn: 0.4637668	total: 12.4s	remaining: 12.4s
    499:	learn: 0.4637471	total: 12.4s	remaining: 12.4s
    500:	learn: 0.4637232	total: 12.4s	remaining: 12.4s
    501:	learn: 0.4637067	total: 12.4s	remaining: 12.3s
    502:	learn: 0.4636740	total: 12.4s	remaining: 12.3s
    503:	learn: 0.4636452	total: 12.5s	remaining: 12.3s
    504:	learn: 0.4636246	total: 12.5s	remaining: 12.2s
    505:	learn: 0.4636054	total: 12.5s	remaining: 12.2s
    506:	learn: 0.4635891	total: 12.5s	remaining: 12.2s
    507:	learn: 0.4635682	total: 12.6s	remaining: 12.2s
    508:	learn: 0.4635449	total: 12.6s	remaining: 12.1s
    509:	learn: 0.4635268	total: 12.6s	remaining: 12.1s
    510:	learn: 0.4635089	total: 12.6s	remaining: 12.1s
    511:	learn: 0.4634711	total: 12.6s	remaining: 12.1s
    512:	learn: 0.4634321	total: 12.7s	remaining: 12s
    513:	learn: 0.4633932	total: 12.7s	remaining: 12s
    514:	learn: 0.4633642	total: 12.7s	remaining: 12s
    515:	learn: 0.4633316	total: 12.7s	remaining: 12s
    516:	learn: 0.4633065	total: 12.8s	remaining: 11.9s
    517:	learn: 0.4632862	total: 12.8s	remaining: 11.9s
    518:	learn: 0.4632698	total: 12.8s	remaining: 11.9s
    519:	learn: 0.4632225	total: 12.8s	remaining: 11.8s
    520:	learn: 0.4632012	total: 12.9s	remaining: 11.8s
    521:	learn: 0.4631620	total: 12.9s	remaining: 11.8s
    522:	learn: 0.4631414	total: 12.9s	remaining: 11.8s
    523:	learn: 0.4631194	total: 12.9s	remaining: 11.7s
    524:	learn: 0.4630930	total: 12.9s	remaining: 11.7s
    525:	learn: 0.4630583	total: 13s	remaining: 11.7s
    526:	learn: 0.4630285	total: 13s	remaining: 11.7s
    527:	learn: 0.4630087	total: 13s	remaining: 11.6s
    528:	learn: 0.4629701	total: 13s	remaining: 11.6s
    529:	learn: 0.4629498	total: 13.1s	remaining: 11.6s
    530:	learn: 0.4629261	total: 13.1s	remaining: 11.6s
    531:	learn: 0.4629016	total: 13.1s	remaining: 11.5s
    532:	learn: 0.4628997	total: 13.1s	remaining: 11.5s
    533:	learn: 0.4628804	total: 13.1s	remaining: 11.5s
    534:	learn: 0.4628440	total: 13.2s	remaining: 11.4s
    535:	learn: 0.4628268	total: 13.2s	remaining: 11.4s
    536:	learn: 0.4628064	total: 13.2s	remaining: 11.4s
    537:	learn: 0.4627887	total: 13.2s	remaining: 11.4s
    538:	learn: 0.4627678	total: 13.3s	remaining: 11.3s
    539:	learn: 0.4627416	total: 13.3s	remaining: 11.3s
    540:	learn: 0.4627175	total: 13.3s	remaining: 11.3s
    541:	learn: 0.4626954	total: 13.3s	remaining: 11.3s
    542:	learn: 0.4626756	total: 13.3s	remaining: 11.2s
    543:	learn: 0.4626457	total: 13.4s	remaining: 11.2s
    544:	learn: 0.4626263	total: 13.4s	remaining: 11.2s
    545:	learn: 0.4625989	total: 13.4s	remaining: 11.2s
    546:	learn: 0.4625737	total: 13.5s	remaining: 11.1s
    547:	learn: 0.4625547	total: 13.5s	remaining: 11.1s
    548:	learn: 0.4625313	total: 13.5s	remaining: 11.1s
    549:	learn: 0.4625298	total: 13.5s	remaining: 11.1s
    550:	learn: 0.4625013	total: 13.5s	remaining: 11s
    551:	learn: 0.4624840	total: 13.6s	remaining: 11s
    552:	learn: 0.4624610	total: 13.6s	remaining: 11s
    553:	learn: 0.4624321	total: 13.6s	remaining: 11s
    554:	learn: 0.4623955	total: 13.6s	remaining: 10.9s
    555:	learn: 0.4623654	total: 13.7s	remaining: 10.9s
    556:	learn: 0.4623306	total: 13.7s	remaining: 10.9s
    557:	learn: 0.4623076	total: 13.7s	remaining: 10.9s
    558:	learn: 0.4622805	total: 13.7s	remaining: 10.8s
    559:	learn: 0.4622622	total: 13.8s	remaining: 10.8s
    560:	learn: 0.4622486	total: 13.8s	remaining: 10.8s
    561:	learn: 0.4622117	total: 13.8s	remaining: 10.8s
    562:	learn: 0.4621923	total: 13.8s	remaining: 10.7s
    563:	learn: 0.4621611	total: 13.8s	remaining: 10.7s
    564:	learn: 0.4621252	total: 13.9s	remaining: 10.7s
    565:	learn: 0.4621024	total: 13.9s	remaining: 10.7s
    566:	learn: 0.4620826	total: 13.9s	remaining: 10.6s
    567:	learn: 0.4620599	total: 13.9s	remaining: 10.6s
    568:	learn: 0.4620290	total: 14s	remaining: 10.6s
    569:	learn: 0.4620077	total: 14s	remaining: 10.6s
    570:	learn: 0.4619892	total: 14s	remaining: 10.5s
    571:	learn: 0.4619696	total: 14s	remaining: 10.5s
    572:	learn: 0.4619295	total: 14.1s	remaining: 10.5s
    573:	learn: 0.4619020	total: 14.1s	remaining: 10.5s
    574:	learn: 0.4618889	total: 14.1s	remaining: 10.4s
    575:	learn: 0.4618631	total: 14.1s	remaining: 10.4s
    576:	learn: 0.4618511	total: 14.1s	remaining: 10.4s
    577:	learn: 0.4618254	total: 14.2s	remaining: 10.3s
    578:	learn: 0.4617986	total: 14.2s	remaining: 10.3s
    579:	learn: 0.4617769	total: 14.2s	remaining: 10.3s
    580:	learn: 0.4617501	total: 14.2s	remaining: 10.3s
    581:	learn: 0.4617394	total: 14.3s	remaining: 10.2s
    582:	learn: 0.4617182	total: 14.3s	remaining: 10.2s
    583:	learn: 0.4616982	total: 14.3s	remaining: 10.2s
    584:	learn: 0.4616772	total: 14.3s	remaining: 10.2s
    585:	learn: 0.4616512	total: 14.4s	remaining: 10.1s
    586:	learn: 0.4616288	total: 14.4s	remaining: 10.1s
    587:	learn: 0.4616152	total: 14.4s	remaining: 10.1s
    588:	learn: 0.4615934	total: 14.5s	remaining: 10.1s
    589:	learn: 0.4615703	total: 14.5s	remaining: 10.1s
    590:	learn: 0.4615505	total: 14.5s	remaining: 10s
    591:	learn: 0.4615333	total: 14.5s	remaining: 10s
    592:	learn: 0.4615128	total: 14.6s	remaining: 10s
    593:	learn: 0.4614878	total: 14.6s	remaining: 9.97s
    594:	learn: 0.4614633	total: 14.6s	remaining: 9.95s
    595:	learn: 0.4614457	total: 14.6s	remaining: 9.92s
    596:	learn: 0.4614247	total: 14.7s	remaining: 9.89s
    597:	learn: 0.4614077	total: 14.7s	remaining: 9.87s
    598:	learn: 0.4613902	total: 14.7s	remaining: 9.84s
    599:	learn: 0.4613635	total: 14.7s	remaining: 9.82s
    600:	learn: 0.4613463	total: 14.7s	remaining: 9.79s
    601:	learn: 0.4613203	total: 14.8s	remaining: 9.77s
    602:	learn: 0.4613012	total: 14.8s	remaining: 9.74s
    603:	learn: 0.4612828	total: 14.8s	remaining: 9.71s
    604:	learn: 0.4612645	total: 14.8s	remaining: 9.69s
    605:	learn: 0.4612359	total: 14.9s	remaining: 9.66s
    606:	learn: 0.4612195	total: 14.9s	remaining: 9.63s
    607:	learn: 0.4612010	total: 14.9s	remaining: 9.61s
    608:	learn: 0.4611876	total: 14.9s	remaining: 9.58s
    609:	learn: 0.4611520	total: 15s	remaining: 9.56s
    610:	learn: 0.4611268	total: 15s	remaining: 9.53s
    611:	learn: 0.4611067	total: 15s	remaining: 9.51s
    612:	learn: 0.4610767	total: 15s	remaining: 9.48s
    613:	learn: 0.4610560	total: 15s	remaining: 9.46s
    614:	learn: 0.4610269	total: 15.1s	remaining: 9.43s
    615:	learn: 0.4610179	total: 15.1s	remaining: 9.4s
    616:	learn: 0.4609943	total: 15.1s	remaining: 9.38s
    617:	learn: 0.4609806	total: 15.1s	remaining: 9.35s
    618:	learn: 0.4609726	total: 15.1s	remaining: 9.32s
    619:	learn: 0.4609495	total: 15.2s	remaining: 9.3s
    620:	learn: 0.4609341	total: 15.2s	remaining: 9.27s
    621:	learn: 0.4609171	total: 15.2s	remaining: 9.24s
    622:	learn: 0.4608912	total: 15.2s	remaining: 9.22s
    623:	learn: 0.4608739	total: 15.3s	remaining: 9.2s
    624:	learn: 0.4608477	total: 15.3s	remaining: 9.17s
    625:	learn: 0.4608232	total: 15.3s	remaining: 9.14s
    626:	learn: 0.4607958	total: 15.3s	remaining: 9.12s
    627:	learn: 0.4607737	total: 15.4s	remaining: 9.1s
    628:	learn: 0.4607536	total: 15.4s	remaining: 9.07s
    629:	learn: 0.4607321	total: 15.4s	remaining: 9.04s
    630:	learn: 0.4606962	total: 15.4s	remaining: 9.02s
    631:	learn: 0.4606798	total: 15.4s	remaining: 8.99s
    632:	learn: 0.4606626	total: 15.5s	remaining: 8.97s
    633:	learn: 0.4606421	total: 15.5s	remaining: 8.94s
    634:	learn: 0.4606160	total: 15.5s	remaining: 8.92s
    635:	learn: 0.4606055	total: 15.5s	remaining: 8.89s
    636:	learn: 0.4605804	total: 15.6s	remaining: 8.87s
    637:	learn: 0.4605600	total: 15.6s	remaining: 8.84s
    638:	learn: 0.4605430	total: 15.6s	remaining: 8.81s
    639:	learn: 0.4604852	total: 15.6s	remaining: 8.79s
    640:	learn: 0.4604722	total: 15.7s	remaining: 8.77s
    641:	learn: 0.4604579	total: 15.7s	remaining: 8.74s
    642:	learn: 0.4604315	total: 15.7s	remaining: 8.72s
    643:	learn: 0.4604011	total: 15.7s	remaining: 8.69s
    644:	learn: 0.4603772	total: 15.7s	remaining: 8.67s
    645:	learn: 0.4603554	total: 15.8s	remaining: 8.64s
    646:	learn: 0.4603449	total: 15.8s	remaining: 8.62s
    647:	learn: 0.4603269	total: 15.8s	remaining: 8.59s
    648:	learn: 0.4603107	total: 15.8s	remaining: 8.56s
    649:	learn: 0.4602941	total: 15.9s	remaining: 8.54s
    650:	learn: 0.4602784	total: 15.9s	remaining: 8.51s
    651:	learn: 0.4602536	total: 15.9s	remaining: 8.49s
    652:	learn: 0.4602419	total: 15.9s	remaining: 8.46s
    653:	learn: 0.4602243	total: 15.9s	remaining: 8.44s
    654:	learn: 0.4602097	total: 16s	remaining: 8.41s
    655:	learn: 0.4601953	total: 16s	remaining: 8.39s
    656:	learn: 0.4601696	total: 16s	remaining: 8.36s
    657:	learn: 0.4601489	total: 16s	remaining: 8.34s
    658:	learn: 0.4601240	total: 16.1s	remaining: 8.31s
    659:	learn: 0.4601054	total: 16.1s	remaining: 8.29s
    660:	learn: 0.4600876	total: 16.1s	remaining: 8.26s
    661:	learn: 0.4600683	total: 16.1s	remaining: 8.23s
    662:	learn: 0.4600450	total: 16.2s	remaining: 8.21s
    663:	learn: 0.4600040	total: 16.2s	remaining: 8.19s
    664:	learn: 0.4599875	total: 16.2s	remaining: 8.16s
    665:	learn: 0.4599516	total: 16.2s	remaining: 8.14s
    666:	learn: 0.4599331	total: 16.2s	remaining: 8.11s
    667:	learn: 0.4599103	total: 16.3s	remaining: 8.09s
    668:	learn: 0.4598994	total: 16.3s	remaining: 8.06s
    669:	learn: 0.4598961	total: 16.3s	remaining: 8.03s
    670:	learn: 0.4598790	total: 16.3s	remaining: 8.01s
    671:	learn: 0.4598607	total: 16.4s	remaining: 7.98s
    672:	learn: 0.4598472	total: 16.4s	remaining: 7.96s
    673:	learn: 0.4598206	total: 16.4s	remaining: 7.93s
    674:	learn: 0.4597874	total: 16.4s	remaining: 7.91s
    675:	learn: 0.4597532	total: 16.4s	remaining: 7.88s
    676:	learn: 0.4597350	total: 16.5s	remaining: 7.86s
    677:	learn: 0.4597119	total: 16.5s	remaining: 7.83s
    678:	learn: 0.4596926	total: 16.5s	remaining: 7.81s
    679:	learn: 0.4596621	total: 16.5s	remaining: 7.78s
    680:	learn: 0.4596408	total: 16.6s	remaining: 7.76s
    681:	learn: 0.4596258	total: 16.6s	remaining: 7.73s
    682:	learn: 0.4595994	total: 16.6s	remaining: 7.71s
    683:	learn: 0.4595768	total: 16.6s	remaining: 7.68s
    684:	learn: 0.4595568	total: 16.7s	remaining: 7.66s
    685:	learn: 0.4595370	total: 16.7s	remaining: 7.63s
    686:	learn: 0.4595153	total: 16.7s	remaining: 7.61s
    687:	learn: 0.4594958	total: 16.7s	remaining: 7.58s
    688:	learn: 0.4594774	total: 16.8s	remaining: 7.56s
    689:	learn: 0.4594549	total: 16.8s	remaining: 7.54s
    690:	learn: 0.4594402	total: 16.8s	remaining: 7.51s
    691:	learn: 0.4594128	total: 16.8s	remaining: 7.49s
    692:	learn: 0.4593982	total: 16.8s	remaining: 7.46s
    693:	learn: 0.4593871	total: 16.9s	remaining: 7.43s
    694:	learn: 0.4593710	total: 16.9s	remaining: 7.41s
    695:	learn: 0.4593510	total: 16.9s	remaining: 7.38s
    696:	learn: 0.4593407	total: 16.9s	remaining: 7.36s
    697:	learn: 0.4593223	total: 17s	remaining: 7.34s
    698:	learn: 0.4593092	total: 17s	remaining: 7.31s
    699:	learn: 0.4592780	total: 17s	remaining: 7.29s
    700:	learn: 0.4592626	total: 17s	remaining: 7.26s
    701:	learn: 0.4592435	total: 17s	remaining: 7.24s
    702:	learn: 0.4592295	total: 17.1s	remaining: 7.21s
    703:	learn: 0.4592147	total: 17.1s	remaining: 7.19s
    704:	learn: 0.4592007	total: 17.1s	remaining: 7.16s
    705:	learn: 0.4591784	total: 17.1s	remaining: 7.14s
    706:	learn: 0.4591636	total: 17.2s	remaining: 7.11s
    707:	learn: 0.4591430	total: 17.2s	remaining: 7.09s
    708:	learn: 0.4591243	total: 17.2s	remaining: 7.06s
    709:	learn: 0.4590984	total: 17.2s	remaining: 7.04s
    710:	learn: 0.4590806	total: 17.3s	remaining: 7.01s
    711:	learn: 0.4590781	total: 17.3s	remaining: 6.98s
    712:	learn: 0.4590593	total: 17.3s	remaining: 6.96s
    713:	learn: 0.4590411	total: 17.3s	remaining: 6.93s
    714:	learn: 0.4590188	total: 17.3s	remaining: 6.91s
    715:	learn: 0.4590011	total: 17.4s	remaining: 6.88s
    716:	learn: 0.4589881	total: 17.4s	remaining: 6.86s
    717:	learn: 0.4589662	total: 17.4s	remaining: 6.83s
    718:	learn: 0.4589442	total: 17.4s	remaining: 6.81s
    719:	learn: 0.4589192	total: 17.5s	remaining: 6.79s
    720:	learn: 0.4588769	total: 17.5s	remaining: 6.76s
    721:	learn: 0.4588527	total: 17.5s	remaining: 6.74s
    722:	learn: 0.4588199	total: 17.5s	remaining: 6.71s
    723:	learn: 0.4587977	total: 17.5s	remaining: 6.69s
    724:	learn: 0.4587864	total: 17.6s	remaining: 6.67s
    725:	learn: 0.4587536	total: 17.6s	remaining: 6.64s
    726:	learn: 0.4587309	total: 17.6s	remaining: 6.62s
    727:	learn: 0.4587084	total: 17.6s	remaining: 6.59s
    728:	learn: 0.4586970	total: 17.7s	remaining: 6.57s
    729:	learn: 0.4586717	total: 17.7s	remaining: 6.54s
    730:	learn: 0.4586615	total: 17.7s	remaining: 6.51s
    731:	learn: 0.4586393	total: 17.7s	remaining: 6.49s
    732:	learn: 0.4586023	total: 17.8s	remaining: 6.47s
    733:	learn: 0.4585754	total: 17.8s	remaining: 6.44s
    734:	learn: 0.4585648	total: 17.8s	remaining: 6.42s
    735:	learn: 0.4585352	total: 17.8s	remaining: 6.39s
    736:	learn: 0.4585120	total: 17.8s	remaining: 6.37s
    737:	learn: 0.4584882	total: 17.9s	remaining: 6.35s
    738:	learn: 0.4584717	total: 17.9s	remaining: 6.32s
    739:	learn: 0.4584506	total: 17.9s	remaining: 6.3s
    740:	learn: 0.4584400	total: 17.9s	remaining: 6.27s
    741:	learn: 0.4584231	total: 18s	remaining: 6.25s
    742:	learn: 0.4583861	total: 18s	remaining: 6.22s
    743:	learn: 0.4583646	total: 18s	remaining: 6.2s
    744:	learn: 0.4583315	total: 18s	remaining: 6.17s
    745:	learn: 0.4583167	total: 18.1s	remaining: 6.15s
    746:	learn: 0.4582962	total: 18.1s	remaining: 6.13s
    747:	learn: 0.4582765	total: 18.1s	remaining: 6.1s
    748:	learn: 0.4582646	total: 18.1s	remaining: 6.08s
    749:	learn: 0.4582608	total: 18.1s	remaining: 6.05s
    750:	learn: 0.4582458	total: 18.2s	remaining: 6.02s
    751:	learn: 0.4582240	total: 18.2s	remaining: 6s
    752:	learn: 0.4582099	total: 18.2s	remaining: 5.97s
    753:	learn: 0.4581939	total: 18.2s	remaining: 5.95s
    754:	learn: 0.4581842	total: 18.3s	remaining: 5.92s
    755:	learn: 0.4581604	total: 18.3s	remaining: 5.9s
    756:	learn: 0.4581397	total: 18.3s	remaining: 5.87s
    757:	learn: 0.4581280	total: 18.3s	remaining: 5.85s
    758:	learn: 0.4581143	total: 18.3s	remaining: 5.82s
    759:	learn: 0.4580891	total: 18.4s	remaining: 5.8s
    760:	learn: 0.4580627	total: 18.4s	remaining: 5.77s
    761:	learn: 0.4580526	total: 18.4s	remaining: 5.75s
    762:	learn: 0.4580375	total: 18.4s	remaining: 5.72s
    763:	learn: 0.4580195	total: 18.5s	remaining: 5.7s
    764:	learn: 0.4580057	total: 18.5s	remaining: 5.67s
    765:	learn: 0.4579887	total: 18.5s	remaining: 5.65s
    766:	learn: 0.4579696	total: 18.5s	remaining: 5.63s
    767:	learn: 0.4579509	total: 18.5s	remaining: 5.6s
    768:	learn: 0.4579495	total: 18.6s	remaining: 5.57s
    769:	learn: 0.4579257	total: 18.6s	remaining: 5.55s
    770:	learn: 0.4579099	total: 18.6s	remaining: 5.53s
    771:	learn: 0.4578826	total: 18.6s	remaining: 5.5s
    772:	learn: 0.4578682	total: 18.6s	remaining: 5.48s
    773:	learn: 0.4578556	total: 18.7s	remaining: 5.45s
    774:	learn: 0.4578421	total: 18.7s	remaining: 5.43s
    775:	learn: 0.4578141	total: 18.7s	remaining: 5.4s
    776:	learn: 0.4577988	total: 18.7s	remaining: 5.38s
    777:	learn: 0.4577819	total: 18.8s	remaining: 5.35s
    778:	learn: 0.4577701	total: 18.8s	remaining: 5.33s
    779:	learn: 0.4577531	total: 18.8s	remaining: 5.3s
    780:	learn: 0.4577352	total: 18.8s	remaining: 5.28s
    781:	learn: 0.4577211	total: 18.8s	remaining: 5.25s
    782:	learn: 0.4577111	total: 18.9s	remaining: 5.23s
    783:	learn: 0.4576968	total: 18.9s	remaining: 5.21s
    784:	learn: 0.4576787	total: 18.9s	remaining: 5.18s
    785:	learn: 0.4576650	total: 18.9s	remaining: 5.16s
    786:	learn: 0.4576469	total: 19s	remaining: 5.13s
    787:	learn: 0.4576377	total: 19s	remaining: 5.11s
    788:	learn: 0.4576243	total: 19s	remaining: 5.08s
    789:	learn: 0.4576076	total: 19s	remaining: 5.06s
    790:	learn: 0.4575942	total: 19s	remaining: 5.03s
    791:	learn: 0.4575747	total: 19.1s	remaining: 5.01s
    792:	learn: 0.4575623	total: 19.1s	remaining: 4.98s
    793:	learn: 0.4575533	total: 19.1s	remaining: 4.96s
    794:	learn: 0.4575277	total: 19.1s	remaining: 4.94s
    795:	learn: 0.4575093	total: 19.2s	remaining: 4.91s
    796:	learn: 0.4574888	total: 19.2s	remaining: 4.89s
    797:	learn: 0.4574696	total: 19.2s	remaining: 4.86s
    798:	learn: 0.4574404	total: 19.2s	remaining: 4.84s
    799:	learn: 0.4574324	total: 19.3s	remaining: 4.81s
    800:	learn: 0.4574140	total: 19.3s	remaining: 4.79s
    801:	learn: 0.4573900	total: 19.3s	remaining: 4.77s
    802:	learn: 0.4573717	total: 19.3s	remaining: 4.74s
    803:	learn: 0.4573504	total: 19.4s	remaining: 4.72s
    804:	learn: 0.4573377	total: 19.4s	remaining: 4.69s
    805:	learn: 0.4573195	total: 19.4s	remaining: 4.67s
    806:	learn: 0.4573076	total: 19.4s	remaining: 4.64s
    807:	learn: 0.4572822	total: 19.4s	remaining: 4.62s
    808:	learn: 0.4572714	total: 19.5s	remaining: 4.6s
    809:	learn: 0.4572412	total: 19.5s	remaining: 4.57s
    810:	learn: 0.4572213	total: 19.5s	remaining: 4.55s
    811:	learn: 0.4572136	total: 19.5s	remaining: 4.52s
    812:	learn: 0.4571862	total: 19.6s	remaining: 4.5s
    813:	learn: 0.4571635	total: 19.6s	remaining: 4.47s
    814:	learn: 0.4571452	total: 19.6s	remaining: 4.45s
    815:	learn: 0.4571295	total: 19.6s	remaining: 4.43s
    816:	learn: 0.4571178	total: 19.7s	remaining: 4.4s
    817:	learn: 0.4571013	total: 19.7s	remaining: 4.38s
    818:	learn: 0.4570851	total: 19.7s	remaining: 4.35s
    819:	learn: 0.4570771	total: 19.7s	remaining: 4.33s
    820:	learn: 0.4570603	total: 19.7s	remaining: 4.3s
    821:	learn: 0.4570368	total: 19.8s	remaining: 4.28s
    822:	learn: 0.4570364	total: 19.8s	remaining: 4.25s
    823:	learn: 0.4570189	total: 19.8s	remaining: 4.23s
    824:	learn: 0.4569980	total: 19.8s	remaining: 4.21s
    825:	learn: 0.4569802	total: 19.9s	remaining: 4.18s
    826:	learn: 0.4569542	total: 19.9s	remaining: 4.16s
    827:	learn: 0.4569360	total: 19.9s	remaining: 4.13s
    828:	learn: 0.4569153	total: 19.9s	remaining: 4.11s
    829:	learn: 0.4568951	total: 19.9s	remaining: 4.09s
    830:	learn: 0.4568824	total: 20s	remaining: 4.06s
    831:	learn: 0.4568659	total: 20s	remaining: 4.04s
    832:	learn: 0.4568395	total: 20s	remaining: 4.01s
    833:	learn: 0.4568130	total: 20s	remaining: 3.99s
    834:	learn: 0.4567972	total: 20.1s	remaining: 3.96s
    835:	learn: 0.4567794	total: 20.1s	remaining: 3.94s
    836:	learn: 0.4567642	total: 20.1s	remaining: 3.92s
    837:	learn: 0.4567562	total: 20.1s	remaining: 3.89s
    838:	learn: 0.4567364	total: 20.2s	remaining: 3.87s
    839:	learn: 0.4567156	total: 20.2s	remaining: 3.84s
    840:	learn: 0.4566981	total: 20.2s	remaining: 3.82s
    841:	learn: 0.4566868	total: 20.2s	remaining: 3.79s
    842:	learn: 0.4566664	total: 20.2s	remaining: 3.77s
    843:	learn: 0.4566496	total: 20.3s	remaining: 3.75s
    844:	learn: 0.4566306	total: 20.3s	remaining: 3.72s
    845:	learn: 0.4566150	total: 20.3s	remaining: 3.7s
    846:	learn: 0.4565976	total: 20.3s	remaining: 3.67s
    847:	learn: 0.4565772	total: 20.4s	remaining: 3.65s
    848:	learn: 0.4565593	total: 20.4s	remaining: 3.63s
    849:	learn: 0.4565394	total: 20.4s	remaining: 3.6s
    850:	learn: 0.4565181	total: 20.4s	remaining: 3.58s
    851:	learn: 0.4565031	total: 20.5s	remaining: 3.56s
    852:	learn: 0.4564851	total: 20.5s	remaining: 3.53s
    853:	learn: 0.4564641	total: 20.5s	remaining: 3.51s
    854:	learn: 0.4564482	total: 20.5s	remaining: 3.48s
    855:	learn: 0.4564319	total: 20.6s	remaining: 3.46s
    856:	learn: 0.4564187	total: 20.6s	remaining: 3.44s
    857:	learn: 0.4564000	total: 20.6s	remaining: 3.41s
    858:	learn: 0.4563870	total: 20.6s	remaining: 3.39s
    859:	learn: 0.4563727	total: 20.7s	remaining: 3.36s
    860:	learn: 0.4563622	total: 20.7s	remaining: 3.34s
    861:	learn: 0.4563425	total: 20.7s	remaining: 3.31s
    862:	learn: 0.4563246	total: 20.7s	remaining: 3.29s
    863:	learn: 0.4562975	total: 20.7s	remaining: 3.27s
    864:	learn: 0.4562817	total: 20.8s	remaining: 3.24s
    865:	learn: 0.4562610	total: 20.8s	remaining: 3.22s
    866:	learn: 0.4562399	total: 20.8s	remaining: 3.19s
    867:	learn: 0.4562167	total: 20.8s	remaining: 3.17s
    868:	learn: 0.4562029	total: 20.9s	remaining: 3.15s
    869:	learn: 0.4561811	total: 20.9s	remaining: 3.12s
    870:	learn: 0.4561683	total: 20.9s	remaining: 3.1s
    871:	learn: 0.4561510	total: 20.9s	remaining: 3.07s
    872:	learn: 0.4561413	total: 20.9s	remaining: 3.05s
    873:	learn: 0.4561190	total: 21s	remaining: 3.02s
    874:	learn: 0.4560982	total: 21s	remaining: 3s
    875:	learn: 0.4560580	total: 21s	remaining: 2.98s
    876:	learn: 0.4560453	total: 21s	remaining: 2.95s
    877:	learn: 0.4560332	total: 21.1s	remaining: 2.93s
    878:	learn: 0.4560155	total: 21.1s	remaining: 2.9s
    879:	learn: 0.4559969	total: 21.1s	remaining: 2.88s
    880:	learn: 0.4559668	total: 21.1s	remaining: 2.85s
    881:	learn: 0.4559490	total: 21.2s	remaining: 2.83s
    882:	learn: 0.4559280	total: 21.2s	remaining: 2.81s
    883:	learn: 0.4559081	total: 21.2s	remaining: 2.78s
    884:	learn: 0.4558973	total: 21.2s	remaining: 2.76s
    885:	learn: 0.4558706	total: 21.3s	remaining: 2.73s
    886:	learn: 0.4558539	total: 21.3s	remaining: 2.71s
    887:	learn: 0.4558312	total: 21.3s	remaining: 2.69s
    888:	learn: 0.4558055	total: 21.3s	remaining: 2.66s
    889:	learn: 0.4557857	total: 21.3s	remaining: 2.64s
    890:	learn: 0.4557686	total: 21.4s	remaining: 2.61s
    891:	learn: 0.4557392	total: 21.4s	remaining: 2.59s
    892:	learn: 0.4557197	total: 21.4s	remaining: 2.57s
    893:	learn: 0.4557035	total: 21.4s	remaining: 2.54s
    894:	learn: 0.4556835	total: 21.5s	remaining: 2.52s
    895:	learn: 0.4556707	total: 21.5s	remaining: 2.49s
    896:	learn: 0.4556560	total: 21.5s	remaining: 2.47s
    897:	learn: 0.4556452	total: 21.5s	remaining: 2.45s
    898:	learn: 0.4556277	total: 21.6s	remaining: 2.42s
    899:	learn: 0.4556122	total: 21.6s	remaining: 2.4s
    900:	learn: 0.4555786	total: 21.6s	remaining: 2.37s
    901:	learn: 0.4555640	total: 21.6s	remaining: 2.35s
    902:	learn: 0.4555446	total: 21.7s	remaining: 2.33s
    903:	learn: 0.4555261	total: 21.7s	remaining: 2.3s
    904:	learn: 0.4555105	total: 21.7s	remaining: 2.28s
    905:	learn: 0.4554856	total: 21.7s	remaining: 2.25s
    906:	learn: 0.4554663	total: 21.7s	remaining: 2.23s
    907:	learn: 0.4554458	total: 21.8s	remaining: 2.21s
    908:	learn: 0.4554283	total: 21.8s	remaining: 2.18s
    909:	learn: 0.4554108	total: 21.8s	remaining: 2.16s
    910:	learn: 0.4553899	total: 21.8s	remaining: 2.13s
    911:	learn: 0.4553896	total: 21.9s	remaining: 2.11s
    912:	learn: 0.4553777	total: 21.9s	remaining: 2.08s
    913:	learn: 0.4553589	total: 21.9s	remaining: 2.06s
    914:	learn: 0.4553303	total: 21.9s	remaining: 2.04s
    915:	learn: 0.4553188	total: 21.9s	remaining: 2.01s
    916:	learn: 0.4553021	total: 22s	remaining: 1.99s
    917:	learn: 0.4552775	total: 22s	remaining: 1.96s
    918:	learn: 0.4552582	total: 22s	remaining: 1.94s
    919:	learn: 0.4552456	total: 22s	remaining: 1.92s
    920:	learn: 0.4552353	total: 22.1s	remaining: 1.89s
    921:	learn: 0.4552037	total: 22.1s	remaining: 1.87s
    922:	learn: 0.4551853	total: 22.1s	remaining: 1.84s
    923:	learn: 0.4551758	total: 22.1s	remaining: 1.82s
    924:	learn: 0.4551646	total: 22.2s	remaining: 1.8s
    925:	learn: 0.4551315	total: 22.2s	remaining: 1.77s
    926:	learn: 0.4551300	total: 22.2s	remaining: 1.75s
    927:	learn: 0.4551201	total: 22.2s	remaining: 1.72s
    928:	learn: 0.4551077	total: 22.2s	remaining: 1.7s
    929:	learn: 0.4550858	total: 22.3s	remaining: 1.68s
    930:	learn: 0.4550616	total: 22.3s	remaining: 1.65s
    931:	learn: 0.4550351	total: 22.3s	remaining: 1.63s
    932:	learn: 0.4550163	total: 22.3s	remaining: 1.6s
    933:	learn: 0.4550073	total: 22.4s	remaining: 1.58s
    934:	learn: 0.4549719	total: 22.4s	remaining: 1.55s
    935:	learn: 0.4549516	total: 22.4s	remaining: 1.53s
    936:	learn: 0.4549308	total: 22.4s	remaining: 1.51s
    937:	learn: 0.4549130	total: 22.4s	remaining: 1.48s
    938:	learn: 0.4548999	total: 22.5s	remaining: 1.46s
    939:	learn: 0.4548913	total: 22.5s	remaining: 1.44s
    940:	learn: 0.4548726	total: 22.5s	remaining: 1.41s
    941:	learn: 0.4548671	total: 22.5s	remaining: 1.39s
    942:	learn: 0.4548387	total: 22.6s	remaining: 1.36s
    943:	learn: 0.4548268	total: 22.6s	remaining: 1.34s
    944:	learn: 0.4547866	total: 22.6s	remaining: 1.31s
    945:	learn: 0.4547752	total: 22.6s	remaining: 1.29s
    946:	learn: 0.4547542	total: 22.7s	remaining: 1.27s
    947:	learn: 0.4547439	total: 22.7s	remaining: 1.24s
    948:	learn: 0.4547325	total: 22.7s	remaining: 1.22s
    949:	learn: 0.4547208	total: 22.7s	remaining: 1.2s
    950:	learn: 0.4547003	total: 22.7s	remaining: 1.17s
    951:	learn: 0.4546867	total: 22.8s	remaining: 1.15s
    952:	learn: 0.4546711	total: 22.8s	remaining: 1.12s
    953:	learn: 0.4546565	total: 22.8s	remaining: 1.1s
    954:	learn: 0.4546364	total: 22.8s	remaining: 1.07s
    955:	learn: 0.4546195	total: 22.9s	remaining: 1.05s
    956:	learn: 0.4546043	total: 22.9s	remaining: 1.03s
    957:	learn: 0.4545783	total: 22.9s	remaining: 1s
    958:	learn: 0.4545592	total: 22.9s	remaining: 980ms
    959:	learn: 0.4545341	total: 22.9s	remaining: 956ms
    960:	learn: 0.4545170	total: 23s	remaining: 932ms
    961:	learn: 0.4544988	total: 23s	remaining: 908ms
    962:	learn: 0.4544688	total: 23s	remaining: 884ms
    963:	learn: 0.4544563	total: 23s	remaining: 860ms
    964:	learn: 0.4544383	total: 23.1s	remaining: 836ms
    965:	learn: 0.4544221	total: 23.1s	remaining: 813ms
    966:	learn: 0.4544077	total: 23.1s	remaining: 789ms
    967:	learn: 0.4543857	total: 23.1s	remaining: 765ms
    968:	learn: 0.4543678	total: 23.2s	remaining: 741ms
    969:	learn: 0.4543516	total: 23.2s	remaining: 717ms
    970:	learn: 0.4543406	total: 23.2s	remaining: 693ms
    971:	learn: 0.4543111	total: 23.2s	remaining: 669ms
    972:	learn: 0.4542974	total: 23.2s	remaining: 645ms
    973:	learn: 0.4542779	total: 23.3s	remaining: 621ms
    974:	learn: 0.4542674	total: 23.3s	remaining: 597ms
    975:	learn: 0.4542575	total: 23.3s	remaining: 573ms
    976:	learn: 0.4542426	total: 23.3s	remaining: 549ms
    977:	learn: 0.4542294	total: 23.4s	remaining: 526ms
    978:	learn: 0.4542146	total: 23.4s	remaining: 502ms
    979:	learn: 0.4541863	total: 23.4s	remaining: 478ms
    980:	learn: 0.4541696	total: 23.4s	remaining: 454ms
    981:	learn: 0.4541501	total: 23.5s	remaining: 430ms
    982:	learn: 0.4541334	total: 23.5s	remaining: 406ms
    983:	learn: 0.4541171	total: 23.5s	remaining: 382ms
    984:	learn: 0.4540999	total: 23.5s	remaining: 358ms
    985:	learn: 0.4540737	total: 23.5s	remaining: 334ms
    986:	learn: 0.4540537	total: 23.6s	remaining: 310ms
    987:	learn: 0.4540358	total: 23.6s	remaining: 287ms
    988:	learn: 0.4539991	total: 23.6s	remaining: 263ms
    989:	learn: 0.4539805	total: 23.6s	remaining: 239ms
    990:	learn: 0.4539649	total: 23.7s	remaining: 215ms
    991:	learn: 0.4539472	total: 23.7s	remaining: 191ms
    992:	learn: 0.4539314	total: 23.7s	remaining: 167ms
    993:	learn: 0.4539172	total: 23.7s	remaining: 143ms
    994:	learn: 0.4539031	total: 23.7s	remaining: 119ms
    995:	learn: 0.4538919	total: 23.8s	remaining: 95.5ms
    996:	learn: 0.4538690	total: 23.8s	remaining: 71.6ms
    997:	learn: 0.4538524	total: 23.8s	remaining: 47.7ms
    998:	learn: 0.4538285	total: 23.8s	remaining: 23.9ms
    999:	learn: 0.4538154	total: 23.9s	remaining: 0us
    model_name: CatBoostClassifier
    Confusion Matrix
    [[13973  3262]
     [ 3145  9620]]
    Model AUC: 0.858, Model Accuracy: 0.786
    
    


    
![](/hueman_images/python/kaggle/output_95_1.png)
    


    CPU times: user 1min 13s, sys: 8.63 s, total: 1min 22s
    Wall time: 24.8 s
    


```python
import numpy as np
from datetime import datetime

version = datetime.now().strftime("%d-%m-%Y %H-%M-%S")

def final_submission(model, data, version):
    final_preds = model.predict(data)
    binarizer = np.vectorize(lambda x: 1 if x >= .5 else 0)
    prediction_binarized = binarizer(final_preds)
    submission = pd.concat([sample_submission,pd.DataFrame(prediction_binarized)], axis=1).drop(columns=['Survived'])
    submission.columns = ['PassengerId', 'Survived']
    submission.to_csv('Sklearn of Submission.csv'.format(version), index=False)
    
final_submission(cb_model, test_data, version)
```


```python

```
