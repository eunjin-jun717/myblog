---
title: "[ML] Imbalanced Data 처리(2)"
date: 2021-05-13
tags: python
---
# 불균형 데이터(Imbalanced Data) 처리(2)
![imbalanced data](hueman_images/python/ml/imbalanced_data.png)

## 2. 독립변수에 따른 불균형 데이터 
> **High Cardinality에 대한 Encoding 방법**

- sklearn의 *Category Encoders package* 사용
- category encoder은 nominal 혹은 ordinal 데이터일 때 사용

> 1) **nominal(명목) data**
> - 통계에서 quantitative 값을 제공하지 않고 변수에 label을 지정하는데 사용되는 데이터이다. 
> - ordinal 데이터와 다르게 순서를 매길 수도 측정할 수도 없다. 
> - ex) gender(성별), 혈액형
      - Encoding: Onehot, Hashing, LeaveOneOut, Target
      - high cardinality 인 열과 decision tree-based 알고리즘은 Onehot encoding을 피해야한다. 

> 2) **ordinal(서수) data**
> - 각각의 값들이 이산적이며 그 값들 사이에 순서가 존재
> - 가장 큰 특징은 데이터 값 간의 차이를 알 수 없거나 의미가 없는 것
> - inteval 혹은 ratio 데이터와 달리 수학적인 연산자를 사용할 수 없다.
> - 따라서 Ordinal data를 포함하는 dataset의 중심 경향을 측정할 수 있는 유일한 척도는 '중앙값'이다.
      - Encoding: Ordinal(Integer), Binary, OneHot, LeaveOneOut, Target
      - Helmert, Sum, BackwardDifference, Polynomial 은 시간적 여유가 있거나 이것을 시도할 이론적인 여유가 있다면 시도해보도록 한다.

      >> regression 문제에서는 Target, LeaveOneOut은 아마 잘 적용되지 않을 것이다. 

### 2-1. Ordinal Encoder
- 각 문자열 값을 정수로 변환
       열의 첫번째 unique한 값은 1, 두번째는 2,...
- Encoding 이전의 실제 값은 OrdinalEncoder로 fit_transform할 때 값에 영향을 주지는 않는다.
- 열에 Nominal data(명목형)가 포함된 경우 OrdinalEncoder를 사용하는 것은 좋지 않다. 
- 머신러닝 알고리즘은 변수를 연속형으로 처리하고 값이 의미있는 척도에 있다고 가정한다. 
- 반대로 열이 Ordinal data(서수형)이면 각 값에 할당된 정수가 의미가 있음을 의미한다. 






```python
!pip install category_encoders
```


```python
import numpy as np
import pandas as pd
import category_encoders as ce
from sklearn.preprocessing import LabelEncoder

pd.options.display.float_format = '{:.2f}'.format

df = pd.DataFrame({
    'color':['a','c','a','a','b','b'],
    'outcome':[1,2,0,0,0,1]
})

X= df.drop('outcome',axis=1)
y= df.drop('color', axis=1)
```


```python
X
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>c</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b</td>
    </tr>
    <tr>
      <th>5</th>
      <td>b</td>
    </tr>
  </tbody>
</table>
</div>




```python
ce_ord = ce.OrdinalEncoder(cols=['color'])
ce_ord.fit_transform(X,y['outcome'])
```

    /usr/local/lib/python3.7/dist-packages/category_encoders/utils.py:21: FutureWarning: is_categorical is deprecated and will be removed in a future version.  Use is_categorical_dtype instead
      elif pd.api.types.is_categorical(cols):
    




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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



- 문자열의 unique값이 나오는 순서대로 정수를 mapping함
- 'a' -> 1, 'c'-> 2, 'b' -> 3

### 2-2. Onehot Encoding
- Nominal data(명목형)를 처리하는 고전적인 접근 방식
- Dummy Encoding, Indicator Encoding, Binary Encoding 이라고도 함
- 다른 모든 값과 비교할 각 값에 대해 하나의 열을 생성함
      각각의 새 열에 대해 행에 해당 열의 값이 포함되어 있으면 1을, 그렇지 않으면 0을 할당


```python
ce_one_hot=ce.OneHotEncoder(cols=['color'])
ce_one_hot.fit_transform(X,y)
```

    /usr/local/lib/python3.7/dist-packages/category_encoders/utils.py:21: FutureWarning: is_categorical is deprecated and will be removed in a future version.  Use is_categorical_dtype instead
      elif pd.api.types.is_categorical(cols):
    




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
      <th>color_1</th>
      <th>color_2</th>
      <th>color_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



- Onehot Encoder은 High Cardinality인 경우에는 심각한 메모리 문제를 일으킬 수 있다. 
      why? High Cardinality는 많은 범주가 있기 때문
- 또한 decision tree-based에서도 문제를 일으킬 수 있다.
 

### 2-3. Binary Encoding
- Onehot encoder와 Hashing Encoder의 하이브리드라 생각할 수 있다.
- Binary Encoding은 열에 있는 값의 일부 고유성은 유지하면서 Onehot encoder보다 적은 features를 생성한다. 
      다시말해, Onehot encoding할 때의 차원보다 더 적은 차원을 생성한다는 뜻
- 메모리 효율성이 더 좋다.
- 많은 차원이 있는 Ordinal data에서 더 적합함
> Binary Encoding 알고리즘
> - category가 숫자형식이 아니면 Ordinal Encoder에 의해 인코딩된다. 
> - Integer들은 Binary code로 변환된다.
>> 5-> 101, 10-> 1010
> - 그런 다음, 해당 이진 문자열의 숫자가 별도의 열로 분할 된다. 
>> Ordinal열에 4~7개의 값이 있는 경우 -> 3개의 새 열에 생성\
하나는 첫번째 비트, 하나는 두번째 비트, 다른 하나는 세번째 비트


```python
ce_bin= ce.BinaryEncoder(cols=['color'])
ce_bin.fit_transform(X,y)
```

    /usr/local/lib/python3.7/dist-packages/category_encoders/utils.py:21: FutureWarning: is_categorical is deprecated and will be removed in a future version.  Use is_categorical_dtype instead
      elif pd.api.types.is_categorical(cols):
    




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
      <th>color_0</th>
      <th>color_1</th>
      <th>color_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



- 첫번째 열에는 variance가 없으므로 모델에 아무런 도움이 되지 않는다. 
- 세가지 level만 있으면 포함된 정보는 혼란스러워진다. 
      따라서, 값이 몇개인 경우에는 열을 OneHot Encoding한다. 
> **결론적으로, Binary Encoding은 High Cardinality인 Ordinal data에 사용하자!**

### 2-4. BaseN
- BaseN의 base=1이면 Onehot Encoding과 같은 동작을 한다. 
- base=2 일 때는 Binary Encoding과 같은 역할을 한다. 
- 따라서 base가 3~8일 때 적합하다. 
- BaseN의 주요 특징은 Grid Search를 더 쉽게 만드는 것이다.



```python
ce_basen= ce.BaseNEncoder(cols =['color'])
ce_basen.fit_transform(X,y)
```

    /usr/local/lib/python3.7/dist-packages/category_encoders/utils.py:21: FutureWarning: is_categorical is deprecated and will be removed in a future version.  Use is_categorical_dtype instead
      elif pd.api.types.is_categorical(cols):
    




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
      <th>color_0</th>
      <th>color_1</th>
      <th>color_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



- BaseN Encoder의 Default base는 BinaryEncoder와 동일한 2이다. 

### 2-5. Hashing
- Onehot Encoding과 유사하지만 새로운 feature가 더 적고 충돌로 인해 정보가 손실될 가능성이 있다. 
> 겹치는 부분이 많지 않으면 충돌이 성능에 큰 영향을 주지는 않는다. 


```python
X
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>c</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b</td>
    </tr>
    <tr>
      <th>5</th>
      <td>b</td>
    </tr>
  </tbody>
</table>
</div>




```python
ce_hash = ce.HashingEncoder(cols=['color'])
ce_hash.fit_transform(X,y)
```

    /usr/local/lib/python3.7/dist-packages/category_encoders/utils.py:21: FutureWarning: is_categorical is deprecated and will be removed in a future version.  Use is_categorical_dtype instead
      elif pd.api.types.is_categorical(cols):
    




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
      <th>col_0</th>
      <th>col_1</th>
      <th>col_2</th>
      <th>col_3</th>
      <th>col_4</th>
      <th>col_5</th>
      <th>col_6</th>
      <th>col_7</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
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
      <th>5</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



- n_components: 확장 열 수를 제어하는 매개 변수
      - 기본 값: 3개의 열
      - 3개 값이 있는 예제 열에는 기본값은 0으로 가득 찬 5개의 열이 된다.
- n_components를 k보다 적게 설정하면 encoding된 데이터에서 제공하는 값이 약간 감소한다. 혹은 더 작은 feature 수를 가지게 된다.
> **결론적으로, HashingEncoder은 High Cardinality가 있는 경우에 Nominal, Ordinal 데이터에 대해 사용할 가치가 있다.**

*references*
- [https://towardsdatascience.com/smarter-ways-to-encode-categorical-data-for-machine-learning-part-1-of-3-6dca2f71b159](https://towardsdatascience.com/smarter-ways-to-encode-categorical-data-for-machine-learning-part-1-of-3-6dca2f71b159)
- [https://contrib.scikit-learn.org/category_encoders/#](https://contrib.scikit-learn.org/category_encoders/#)
