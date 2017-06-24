---
layout: post
title: 두 배열의 교집합 데이터 구하기
---

두 배열 A, B간의 교집합인 요소를 구하려고 한다. 이 때, 각 배열속에는 중복된 요소가 존재할 수 있고 두 배열간의 교집합을 구하려할때 이 중복된 요소는 한번만 출력하도록 한다.


## First Trial(Brute Force)
교집합을 구하는 방법중 가장 쉽게 생각할 수 있는 방법은 Brute Force 방법이다. 

A의 모든 요소에 대해 B의 모든 요소를 비교해가면서 교집합을 구한다.

```python
def get_intersection_bt_arrays_first_trial(arr01, arr02):
    printed_elements = {}
    intersection = []

    for elem01 in arr01:
        for elem02 in arr02:
            if elem02 == elem01 and elem02 not in printed_elements:
                intersection.append(elem02)
                printed_elements[elem02] = True

    return intersection
```
배열 A의 길이가 m이고 배열 B의 길이가 n이라고 했을 때 
<br/>해당 로직의 time complexity는 **O(mn)**

이 방법의 장점은 구현하기 쉽다는 것.?
이 방법의 단점은 이중 루프를 쓰기에 각 배열의 길이가 늘어났을 때 time complexity가 기하급수적으로 늘어난다

## Second Trial(Single Loop)
First Trial의 문제점은 이중루프를 쓴다는 것이다. 이중루프를 없애고 A의 중복되지 않은 모든 요소를 딕셔너리(sth. like hash table)에 넣고, B를 linear search하면서 딕셔너리에 값이 있는지 확인(O(1))하도록 하면 O(mn)보다 시간을 줄일 수 있다.

```python
def get_intersection_bt_arrays_second_trial(arr01, arr02):
    overlapped_elements = {}
    printed_elements = {}
    intersection = []

    for elem in arr01:
        overlapped_elements[elem] = True

    for elem in arr02:
        if elem in overlapped_elements and elem not in printed_elements:
            intersection.append(elem)
            printed_elements[elem] = True
    return intersection
```

**시간복잡도: O(m+n)**

### Third Method(Sorting & Search)
그 다음으로 두 배열을 정렬한 후에 교집합을 찾는 방법이다.
각 배열의 길이, 정렬된 상태를 모른다고 가정했을 때 빠른 성능을 낼 수 있는 배열로 퀵소트를 사용하자.

교집합을 구하기전에 두 배열을 먼저 정렬하고,
그다음에 각 배열의 첫 인덱스부터 시작해서 

* 같으면 교집합에 넣고
* 다르면 비교한 요소 중 작은 요소가 속한 배열의 인덱스를 증가시킨다.

이와 같은 비교를 두 배열 중 하나라도 마지막 인덱스를 넘어버리면 탐색을 중지한다.

```python
def get_intersection_bt_arrays_third_trial(arr01, arr02):
    arr01.sort()
    arr02.sort()

    intersection = set()

    i = j = 0
    len01 = len(arr01)
    len02 = len(arr02)

    while i < len01 and j < len02:
        if arr01[i] == arr02[j]:
            intersection.add(arr01[i])
            i += 1
            j += 1
        elif arr01[i] < arr02[j]:
            i += 1
        else:
            j += 1

    return list(intersection)
```

위와 같은 방법을 썼을 때 
* 두 배열의 정렬 시간 mlogm, nlogn
* 탐색을 끝내는 조건인 작은 배열의 길이 min(m,n)
의 시간이 필요하게 된다.

**시간복잡도: O(mlogm + nlogn + min(m,n)**


## 전체 코드 및 테스트
```python

def get_intersection_bt_arrays_first_trial(arr01, arr02):
    """
    O(mn)
    """
    printed_elements = {}
    intersection = [] 

    for elem01 in arr01:
        for elem02 in arr02:
            if elem02 == elem01 and elem02 not in printed_elements:
                intersection.append(elem02)
                printed_elements[elem02] = True

    return intersection 

def get_intersection_bt_arrays_second_trial(arr01, arr02):
    """
    O(m+n)
    """
    overlapped_elements = {}
    printed_elements = {}
    intersection = [] 

    for elem in arr01:
        overlapped_elements[elem] = True

    for elem in arr02:
        if elem in overlapped_elements and elem not in printed_elements:
            intersection.append(elem)
            printed_elements[elem] = True
    return intersection

def get_intersection_bt_arrays_third_trial(arr01, arr02):
    """
    O(mlogm + nlogn + min(m,n))
    """
    arr01.sort()
    arr02.sort()
    
    intersection = set()

    i = j = 0
    len01 = len(arr01)
    len02 = len(arr02)

    while i < len01 and j < len02:
        if arr01[i] == arr02[j]:
            intersection.add(arr01[i])
            i += 1
            j += 1
        elif arr01[i] < arr02[j]:
            i += 1
        else:
            j += 1
    
    return list(intersection)



def main():
    arr01 = [0, 1, 1, 3, 3, 4, 5, 6, 7, 8, 1]
    arr02 = [0, 1, 1, 7, 8, 1, 9, 2, 2, 5, 3]

    answer = [0,1,7,8,5,3]

    first_trial = get_intersection_bt_arrays_first_trial(arr01, arr02)
    second_trial = get_intersection_bt_arrays_second_trial(arr01, arr02)
    third_trial = get_intersection_bt_arrays_third_trial(arr01, arr02)

    print('First Trial: %r' % (set(answer) == set(first_trial)))
    print('Second Trial: %r' % (set(answer) == set(second_trial)))
    print('Third Trial: %r' % (set(answer) == set(third_trial)))
    """
    First Trial: True
    Second Trial: True
    Third Trial: True
    """
if __name__ == '__main__':
    main()
```

## 시간복잡도 테스트
```
from math import log

def print_time_complexity(m, n):
    print("m: %r |n: %r" % (m, n))
    print("O(mn): %r" % (m*n))
    print("O(m+n): %r" % (m+n))
    print("O(mlogm+nlogn+min(m,n)): %r" % (m*log(m,2) + n*log(n,2) + min(m,n)))
    print("-----------------------------------")



if __name__ == '__main__':
   print_time_complexity(1,1) 
   print_time_complexity(10,10) 
   print_time_complexity(1000,1000) 
   print_time_complexity(1000,100000000) 
   print_time_complexity(100000000,100000000) 

"""
m: 1 |n: 1
O(mn): 1
O(m+n): 2
O(mlogm+nlogn+min(m,n)): 1.0
-----------------------------------
m: 10 |n: 10
O(mn): 100
O(m+n): 20
O(mlogm+nlogn+min(m,n)): 76.43856189774725
-----------------------------------
m: 1000 |n: 1000
O(mn): 1000000
O(m+n): 2000
O(mlogm+nlogn+min(m,n)): 20931.568569324176
-----------------------------------
m: 1000 |n: 100000000
O(mn): 100000000000
O(m+n): 100001000
O(mlogm+nlogn+min(m,n)): 2657553441.694175
-----------------------------------
m: 100000000 |n: 100000000
O(mn): 10000000000000000
O(m+n): 200000000
O(mlogm+nlogn+min(m,n)): 5415084951.81978
-----------------------------------
"""
```
