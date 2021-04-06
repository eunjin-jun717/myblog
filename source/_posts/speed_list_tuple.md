# List vs Tuple

## 차이점
1) list는 *mutable* 즉, 생성된 후에 변경 **가능**하다.\
tuple은 *immutable* 즉, 생성된 후에 변경이 **불가능**하다. 

2) list는 dictionary의 key값으로 쓸 수 없지만 tuple은 가능하다.\
*why?* key값은 불변한 값만 올 수 있기 때문!\
만약, tuple에 list가 들어있다면?\
=> key 값으로 사용 **불가능**함! 
* 참고) 문자열 또한 **불변**한 값이므로 dictionary의 key값으로 사용 가능하다.



## *list* 대신 *tuple*을 사용하는 이유는?

## 메모리 크기 비교


```python
list_1=['A','B','C','D']
tuple_1='A','B','C','D'

import sys
sys.getsizeof(list_1)
sys.getsizeof(tuple_1)
```

    10000000 loops, best of 5: 52.8 ns per loop
    100000000 loops, best of 5: 16 ns per loop
    The slowest run took 21.06 times longer than the fastest. This could mean that an intermediate result is being cached.
    10000000 loops, best of 5: 47.8 ns per loop
    10000000 loops, best of 5: 42.3 ns per loop
    

- tuple이 메모리를 차이하는 것이 더 작은 것을 볼 수 있다. 

## 생성 속도


```python
%timeit list_2=['a','b','c','d']
%timeit tuple_2='a','b','c','d'
```

- tuple의 생성속도가 더 빠른 것을 확인할 수 있다.

## 인덱싱 속도


```python
list_3=['A','B','C','D']
%timeit list_3[0]

tuple_3='A','B','C','D'
%timeit tuple_3[0]
```

tuple이 list보다 indexing 으로 데이터에 접근하는 속도가 더 빠르다. 

## 결과
- 메모리 크기비교, 생성속도, 인덱싱 속도에서 모두 tuple의 결과가 더 빠른것을 확인 할 수 있었다. 

### 출처
- [https://codacoding.tistory.com/36](https://codacoding.tistory.com/36)
- [https://itholic.github.io/python-list-tuple/](https://itholic.github.io/python-list-tuple/)
