---
title: "알고리즘 -  이진 탐색"
excerpt: "순차 탐색, 이진 탐색"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2021-02-26
---

## 순차 탐색

-   정렬되어 있지 않은 배열에서 0번째 인덱스부터 순차적으로 끝까지 탐색하며 탐색하는 원소를 발견했을 때 탐색을 중단한다.
-   최악의 시간 복잡도는 O(N)

```python
def searchIndex(arr, target):
    for i, v in enumerate(arr):
        if v == target:
            return i
    return None

print(searchIndex([1, 3, 5, 7, 9, 11, 13, 15], 5))  # 2
print(searchIndex([1, 3, 5, 7, 9, 11, 13, 15], 6))  # None
print(searchIndex([1, 3, 5, 7, 9, 11, 13, 15], 12))  # None
```

## 이진 탐색

-   정렬되어 있는 배열에서 시작점, 끝점, 중간점을 이용하여 원소를 탐색하는 방법이다.
-   중간점의 값과 탐색하는 원소의 값을 반복적으로 비교하여 위치를 찾아낸다.
-   한 번 확인할 때마다 확인하는 원소의 개수가 절반씩 줄어들어서 시간복잡도는 O(logN)이다.

```python
# 재귀함수로 구현
def recursive_binary_search(arr, target, start, end):
    mid = (start + end) // 2
    if arr[mid] == target:
        return mid
    elif arr[start] <= target and target < arr[mid]:
        return recursive_binary_search(arr, target, start, mid-1)
    elif arr[mid] < target and target <= arr[end]:
        return recursive_binary_search(arr, target, mid+1, end)
    else:
        return None


print(recursive_binary_search([3, 5, 7, 9, 11, 13, 15, 17, 19, 21], 7, 0, 9))
print(recursive_binary_search([3, 5, 7, 9, 11, 13, 15, 17, 19, 21], 6, 0, 9))
print(recursive_binary_search([3, 5, 7, 9, 11, 13, 15, 17, 19, 21], 22, 0, 9))


# 반복문으로 구현
def iterative_binary_search(arr, target):
    start = 0
    end = len(arr) - 1

    while start <= end:
        mid = (start + end) // 2
        if arr[mid] == target:
            return mid
        elif arr[start] <= target and target < arr[mid]:
            end = mid - 1
        elif arr[mid] < target and target <= arr[end]:
            start = mid + 1
        else:
            return None


print(iterative_binary_search([3, 5, 7, 9, 11, 13, 15, 17, 19, 21], 7))
print(iterative_binary_search([3, 5, 7, 9, 11, 13, 15, 17, 19, 21], 6))
print(iterative_binary_search([3, 5, 7, 9, 11, 13, 15, 17, 19, 21], 22))
```

## bisect 라이브러리를 활용한 이진 탐색

-   이진 탐색과 같은 맥락으로 bisect 라이브러리는 정렬된 배열에서 특정한 원소를 찾아야할 때 효과적이다.
-   또한 정렬된 배열에서 특정 범위에 속하는 원소의 개수를 구할 때 효과적이다.
-   bisect_left(), bisect_right 두 함수가 메인이다. 시간복잡도는 O(logN)이다.
-   bisect_left(a, x) : 정렬된 배열 a에 x라는 원소를 삽입할 때 x가 삽입될 수 있는 가장 왼쪽 인덱스를 리턴 (정렬순서는 유지)
-   bisect_right(a, x) : 정렬된 배열 a에 x라는 원소를 삽입할 때 x가 삽입될 수 있는 가장 오른쪽 인덱스를 리턴 (정렬순서는 유지)

```python
from bisect import bisect_left, bisect_right

a = [3, 4, 5, 5, 6]
left = bisect_left(a, 5)
right = bisect_right(a, 5)

print(left, right) # 2 4

# 특정 범위에 속하는 원소의 개수를 구하기
def count_by_range(a, left_value, right_value):
    right = bisect_right(a, right_value)
    left = bisect_left(a, left_value)
    return right - left

print(count_by_range([1, 2, 3, 3, 4, 5, 7, 9], 2, 8)) # 2이상 8이하의 원소의 개수 -> 6개
print(count_by_range([1, 2, 3, 3, 4, 5, 7, 9], 22, 88)) # 22이상 88이하의 원소의 개수 -> 0개
```

## 예제

-   문제 : 높이가 19, 14, 10, 17인 떡이 있고 손님이 총 6만큼의 떡을 요청한다. 이러한 상황에서 떡의 높이 배열과 요청하는 떡의 길이가 주어진다면 이를 수행할 수 있는 절단기 높이의 최댓값을 구하시오.
-   조건 : 떡의 개수는 1 <= n <= 1,000,000이고 손님이 요청한 떡길이는 1 <= m <= 2,000,000,000이다. 각 떡의 길이는 1과 10억 사이다.
-   예시 : 만약 절단기의 높이가 15라면 잘리고 남은 떡 배열은 각각 4, 0, 0, 2이 되고 합이 6이 되어 문제의 조건을 충족시킨다. 답은 15가 된다.
-   잘못된 풀이 : 절단기의 높이는 0부터 19까지 가능하다. 떡 배열을 내림차순으로 정렬시킨 후, 절단기의 높이를 19부터 내림차순으로 탐색하여 19일 때 얻게 되는 떡의 수와 m을 비교하며 m보다 크거나 같아지면 반복문을 종료한 후 해당 절단기 값을 리턴 **(순차 탐색으로 시간초과)**
-   올바른 풀이 : 절단기의 높이는 0부터 19까지 가능하다. 따라서 중간값인 9일 때 얻게 되는 떡의 수가 m보다 작은지 큰지에 따라 중간값을 재할당하고를 반복한다. m보다 클 때엔 전역변수에 값을 저장해놨다가 반복이 중단되면 마지막으로 저장해둔 전역변수가 절단기의 최댓값이 된다. **(이진 탐색)**
-   절단기가 가질 수 있는 높이는 0에서 10억이므로 이진탐색(O(logN))으로 절단기가 가질 수 있는 높이의 경우의 수는 약 31가지로 단축된다. (2의 31제곱이 약 10억)
-   각 절단기의 경우의 수마다 모든 떡을 절단기의 길이와 비교하는 과정이 필요하다. 따라서 최악의 경우 약 31 x 100만번의 연산이 진행되므로 이는 시간 초과에 걸리지 않는 범위가 된다.

```python
import sys

# 순차 탐색 (시간 초과)
n, m = map(int, input().split())
dduckList = list(map(int, sys.stdin.readline().rstrip().split()))

dduckList.sort(reverse=True)

longest = dduckList[0]
for i in range(longest, 0, -1):
    tmp = 0
    for dduck in dduckList:
        if dduck > i:
            tmp += (dduck-i)
            if tmp >= m:
                break
        else:
            break
    if tmp >= m:
        print(i)
        break

# 이진 탐색
n, m = map(int, input().split())
dduckList = list(map(int, sys.stdin.readline().rstrip().split()))

start = 0 # 절단기가 가질 수 있는 최솟값
end = max(dduckList) # 절단기가 가질 수 있는 최댓값

answer = 0
while start <= end:
    mid = (start + end) // 2 # 중간값(절단기) 설정
    total = 0
    for dduck in dduckList:
        if dduck > mid: # 중간값(절단기)보다 큰 떡만 찾아내어
            total += (dduck - mid) # 합에 더한다.
    if total < m:
        # 자르고 난 뒤 합친 떡의 양이 부족할 경우 중간값을 왼쪽으로 하여 더 많이 자르기 (왼쪽 부분 탐색)
        end = mid - 1
    else:
        # 자르고 난 뒤 합친 떡의 양이 충분할 경우 중간값을 오른쪽으로 하여 더 조금 자르기 (오른쪽 부분 탐색)
        # 요청한 떡의 길이보다 큰 값을 찾자마자 반복을 종료하는 것이 아니라 계속 진행하여 절단기가 가질 수 있는 최댓값까지 반복한다.
        start = mid + 1
        answer = mid

print(answer)
```
