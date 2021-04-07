# 블로그 실습 따라하기
- [Spines&Grids practice](https://jehyunlee.github.io/2020/08/27/Python-DS-28-mpl_spines_grids/)

## 시각화 코드를 함수로 만들기


```python
def plot_example(ax, zorder=0): # zorder(레이어의 위치)
  ax.bar(tips_day["day"],tips_day["tip"],color="lightgray", zorder=zorder) #x축: day, y축: tip
  ax.set_title("tip (mean)", fontsize=16, pad=12)

  # values
  h_pad=0.1
  for i in range(len(ax.patches)): # axes에 들어오는 객체: 막대기 4개 즉, ax.patches 길이는 4
    fontweight="normal" #폰트 굵기
    color="k" # 폰트 색깔
    if i == 3: # 일요일
      fontweight="bold" 
      color="darkred"

    ax.text(i, tips_day["tip"].loc[i]+h_pad, f"{tips_day['tip'].loc[i]:0.2f}", # loc[i]: 요일별높이 설정해줌, f": 글자 출력
            horizontalalignment='center',fontsize=12, fontweight=fontweight, color=color)
    
    #Sunday # 4번째가 sunday
  ax.patches[3].set_facecolor("darkred") # bar채우기색
  ax.patches[3].set_edgecolor("black") # bar edge 색

  # set Range
  ax.set_ylim(0,4)
  return ax
```

- seaborn의 tips 데이터 가져오기


```python
import matplotlib.pyplot as plt
import seaborn as sns

tips = sns.load_dataset("tips")
tips.head()
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
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
tips_day=tips.groupby("day").mean().reset_index() # 요일별 평균 데이터 
tips_day
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
      <th>day</th>
      <th>total_bill</th>
      <th>tip</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Thur</td>
      <td>17.682742</td>
      <td>2.771452</td>
      <td>2.451613</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Fri</td>
      <td>17.151579</td>
      <td>2.734737</td>
      <td>2.105263</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sat</td>
      <td>20.441379</td>
      <td>2.993103</td>
      <td>2.517241</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sun</td>
      <td>21.410000</td>
      <td>3.255132</td>
      <td>2.842105</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax =plt.subplots() # figure, axes 객체 생성
ax=plot_example(ax)
```


    
![](/hueman_images/grid_spine/output_6_0.png)
    



```python
type(ax.spines) #dictionary의 일종
```




    collections.OrderedDict




```python
for k,v in ax.spines.items():
  print(f"spines[{k}]={v}")
```

    spines[left]=Spine
    spines[right]=Spine
    spines[bottom]=Spine
    spines[top]=Spine
    


```python
ax.spines.values() # spine은 patch의 subclass
```




    odict_values([<matplotlib.spines.Spine object at 0x7f0fd2d9f890>, <matplotlib.spines.Spine object at 0x7f0fd2d9f050>, <matplotlib.spines.Spine object at 0x7f0fd2d9f1d0>, <matplotlib.spines.Spine object at 0x7f0fd2d9fcd0>])




```python
fig, ax=plt.subplots()
ax = plot_example(ax)

# set_visible(False)를 함으로써 bottom 축만 남기고 다른 축들은 숨김
ax.spines['top'].set_visible(False)
ax.spines['left'].set_visible(False)
ax.spines['right'].set_visible(False)
```


    
![](/hueman_images/grid_spine/output_10_0.png)
    



```python
fig,ax = plt.subplots()
ax = plot_example(ax)

# spince의 일부 영역만 보고싶을 때
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_bounds(1,3) # set_bounds로 최소, 최대 영역 정해줌
```


    
![](/hueman_images/grid_spine/output_11_0.png)
    



```python
ax.spines['left'].get_position() # left spine이 밖으로 0만큼 나가있음
```




    ('outward', 0.0)




```python
fig, ax = plt.subplots()
ax = plot_example(ax)

ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_position(('outward',10))  # left spine을 바깥쪽으로 10만큼 움직임
```


    
![](/hueman_images/grid_spine/output_13_0.png)
    


### 그래프 3개 한꺼번에 그리기


```python
fig, ax = plt.subplots(ncols=3, figsize=(15,3))

for i in range(3): #3개의 그래프
  ax[i]=plot_example(ax[i])
  ax[i].spines['top'].set_visible(False)
  ax[i].spines['right'].set_visible(False)

# ax[0]: spine을 data영역에서 -50만큼 이동
ax[0].spines['left'].set_position(("outward",-50))

# ax[1]: spine을 axes의 0.3의 위치에 설정
ax[1].spines['left'].set_position(('axes',0.3))

# ax[2]: spine을 data의 2.5의 위치에 설정
ax[2].spines['left'].set_position(('data',2.5))
```


    
![](/hueman_images/grid_spine/output_15_0.png)
    


### sin 그래프 그리기


```python
import numpy as np

## data
x= np.linspace(-np.pi, np.pi,100)
y=2*np.sin(x)

fig, ax = plt.subplots(ncols=3, figsize=(12,4)) # 그래프 3개 만듦

## normal plot
ax[0].plot(x,y)
ax[0].set_title("normal plot", pad=12)

## textbook(1)
ax[1].plot(x,y)
ax[1].set_title("textbook (1)", pad=12)
ax[1].spines['left'].set_visible(False)
ax[1].spines['top'].set_visible(False)
ax[1].spines['right'].set_position(('data',0)) # (0,0) 지나가게 함
ax[1].spines['bottom'].set_position(('data',0))

## textbook(2)
ax[2].plot(x,y)
ax[2].set_title("textbook (2)", pad=12)
ax[2].spines['left'].set_visible(False)
ax[2].spines['top'].set_visible(False)
ax[2].spines['right'].set_position(('data',0))
ax[2].spines['bottom'].set_position(('data',0))
ax[2].plot(1,0,'>k',transform=ax[2].get_yaxis_transform(),clip_on=False) # >k 화살표 모양
# get_yaxis_transform이면 x축 기준으로 1만큼 y기준 움직이지 않음
ax[2].plot(0,1,'^k',transform=ax[2].get_xaxis_transform(),clip_on=False) # ^k 위쪽 화살표 
# get_xaxis_transform이면 y축 기준으로 x는 움직이지 않고 y는 1만큼 움직임

plt.show()
```


    
![](/hueman_images/grid_spine/output_17_0.png)
    


- set_position(('axes',0.5)) = set_position('center')
- set_position(('data',0)) = set_position('zero')


```python
fig, ax = plt.subplots(ncols=3, figsize=(15,3))

for i in range(3):
  ax[i]=plot_example(ax[i])
  ax[i].spines['top'].set_visible(False)
  ax[i].spines['right'].set_visible(False)
  ax[i].spines['left'].set_position(('outward',10))

# ax[0]: x,y 둘다
ax[0].grid(axis='both')

# ax[1]: x축만
ax[1].grid(axis='x')

# ax[2]: y축만
ax[2].grid(axis='y')
```


    
![](/hueman_images/grid_spine/output_19_0.png)
    



```python
# major, minor tick 설정
from matplotlib.ticker import (MultipleLocator, AutoMinorLocator, StrMethodFormatter)
fig, ax = plt.subplots()
ax=plot_example(ax)

ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_visible(False)

# y축 tick 설정

ax.yaxis.set_major_locator(MultipleLocator(1)) # y축 major tick을 1단위로 설정
ax.yaxis.set_major_formatter('{x:0.5f}')
ax.yaxis.set_minor_locator(MultipleLocator(0.5)) # 0.5마다 minor tick 그림

plt.plot()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-180-84d5866b2f60> in <module>()
         11 
         12 ax.yaxis.set_major_locator(MultipleLocator(1)) # y축 major tick을 1단위로 설정
    ---> 13 ax.yaxis.set_major_formatter('{x:0.5f}')
         14 ax.yaxis.set_minor_locator(MultipleLocator(0.5)) # 0.5마다 minor tick 그림
         15 
    

    /usr/local/lib/python3.7/dist-packages/matplotlib/axis.py in set_major_formatter(self, formatter)
       1626         formatter : `~matplotlib.ticker.Formatter`
       1627         """
    -> 1628         cbook._check_isinstance(mticker.Formatter, formatter=formatter)
       1629         self.isDefault_majfmt = False
       1630         self.major.formatter = formatter
    

    /usr/local/lib/python3.7/dist-packages/matplotlib/cbook/__init__.py in _check_isinstance(_types, **kwargs)
       2126                     ", ".join(names[:-1]) + " or " + names[-1]
       2127                     if len(names) > 1 else names[0],
    -> 2128                     type_name(type(v))))
       2129 
       2130 
    

    TypeError: 'formatter' must be an instance of matplotlib.ticker.Formatter, not a str



    
![](/hueman_images/grid_spine/output_20_1.png)
    


- 오류 상황\
  : ax.yaxis.set_major_formatter('{x:0.5f}') 에서 Type Error가 발생하였다. 
  
 *TypeError: 'formatter' must be an instance of matplotlib.ticker.Formatter, not a str*

- 해결 방법\
  :  string을 입력했을 때, 내부적으로 StrMethodFormatter이 autogenerate되서 str로 교체된다.\
  그러나 Error를 보면 자동으로 동작하지 않았기 때문에 Type Error가 뜬 것으로 보인다.
  따라서, StrMethodFormatter 의 함수를 입력하면 된다.\
  from matplotlib.ticker import (MultipleLocator, AutoMinorLocator, **StrMethodFormatter**) 처럼 StrMethodFormatter를 추가한 뒤,\
  *ax.yaxis.set_major_formatter(StrMethodFormatter('{x:0.2f}'))*\
  위의 코드로 변경하면 된다 .
  ![](/hueman_images/grid_spine/formatter_tree.png)



```python
# major, minor tick 설정
from matplotlib.ticker import (MultipleLocator, AutoMinorLocator, StrMethodFormatter)
fig, ax = plt.subplots()
ax=plot_example(ax)

ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_visible(False)

# y축 tick 설정

ax.yaxis.set_major_locator(MultipleLocator(1)) # y축 major tick을 1단위로 설정
ax.yaxis.set_major_formatter(StrMethodFormatter('{x:0.2f}')) # y축 단위 소수점 2자리까지 보임
ax.yaxis.set_minor_locator(MultipleLocator(0.5)) # 0.5마다 minor tick 그림

plt.plot()
```




    []




    
![](/hueman_images/grid_spine/output_22_1.png)
    



```python
fig, ax = plt.subplots(ncols=3, figsize=(15,3))

for i in range(3):
  ax[i]= plot_example(ax[i], zorder=2) # bar를 grid 앞으로 위치시킴
  ax[i].spines['top'].set_visible(False)
  ax[i].spines['right'].set_visible(False)
  ax[i].spines['left'].set_position(('outward',10))

  ax[i].yaxis.set_major_locator(MultipleLocator(1))
  ax[i].yaxis.set_major_formatter(StrMethodFormatter('{x:0.2f}'))
  ax[i].yaxis.set_minor_locator(MultipleLocator(0.5))

# ax[0]: major, minor 둘다
ax[0].grid(axis='y',which='both')

# ax[1]: major 만
ax[1].grid(axis='y', which='major')

# ax[2]: major만 & option
ax[2].grid(axis='y', which='major', color='r', lw=0.5, alpha=0.5)

plt.show()

```


    
![](/hueman_images/grid_spine/output_23_0.png)
    



```python
fig, ax = plt.subplots()
ax=plot_example(ax, zorder=2)

ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_visible(False)

ax.yaxis.set_major_locator(MultipleLocator(1))
ax.yaxis.set_major_formatter(StrMethodFormatter('{x:0.2f}'))
ax.yaxis.set_minor_locator(MultipleLocator(0.5))

ax.grid(axis='y',which='major', color='lightgray')
ax.grid(axis='y', which='minor', ls=':') # 점선으로 minor 부분 그리기
```


    
![](/hueman_images/grid_spine/output_24_0.png)
    


## 출처
- [Jehyunlee: spines and grids](https://jehyunlee.github.io/2020/08/27/Python-DS-28-mpl_spines_grids/)
- [Formatter type error solution 참고](https://matplotlib.org/stable/api/ticker_api.html#matplotlib.ticker.TickHelper)
