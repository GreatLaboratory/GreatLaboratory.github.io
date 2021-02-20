---
title: "알고리즘 - 정렬"
excerpt: "버블정렬, 선택정렬, 삽입정렬, 퀵정렬, 계수정렬"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2021-02-20
---

## 버블정렬

-   인접한 두 원소를 비교하여 더 작은 숫자가 앞으로 가게끔 두 원소의 위치를 바꾸는(또는 바꾸지 않는) 방식의 정렬이다.
-   각 정렬 단계마다 뒤쪽의 인덱스 원소부터 정렬된다.
-   시간복잡도는 평균, 최악 모두 O(N^2)이다.

```python
def bubble_sort(arr):
    for i in range(len(arr), 0, -1):
        for j in range(i-1):
            if arr[j] > arr[j+1]: # 현재 탐색 원소가 인접한 원소보다 클 경우
                arr[j], arr[j+1] = arr[j+1], arr[j] # 서로의 자리를 바꾼다.
    return arr


print(bubble_sort([7, 5, 3, 8, 0, 9, 1, 2, 4, 6]))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 선택정렬

-   현재 순차 탐색하는 원소와 나머지 꼬리 배열에서 가장 작은 원소를 비교하여 현재 원소가 더 크면 두 원소의 위치를 바꾸는 방식의 정렬이다.
-   각 정렬 단계마다 앞쪽의 인덱스 원소부터 정렬된다.
-   시간복잡도는 O(N^2)이다.

```python
def selection_sort(arr):
    for i in range(len(arr)):
        minIndex = i # 처음엔 가장 작은 원소의 인덱스를 탐색 원소의 인덱스로 설정
        for j in range(i+1, len(arr)): # 꼬리 배열 순회
            if arr[minIndex] > arr[j]: # 만약 가장 작은 원소의 인덱스라고 여겨지는 minIndex의 원소보다 더 작은게 등장하면
                minIndex = j # 그 원소의 인덱스를 minIndex로 대체한다.
        arr[i], arr[minIndex] = arr[minIndex], arr[i] # 최종적으로 가장 작은 원소의 인덱스가 확정되면 해당 인덱스의 원소와 현재 탐색 원소의 위치를 바꾼다.
    return arr


print(selection_sort([7, 5, 3, 8, 0, 9, 1, 2, 4, 6]))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 삽입정렬

-   특정한 위치에 알맞은 원소를 삽입하며 정렬하는 알고리즘이다.
-   원소들이 거의 정렬되어 있을 때 굉장히 효율적이다.
-   시간복잡도는 O(N^2)인데 만약 원소들이 거의 정렬되어 있는 최선의 경우에선 O(N)의 시간복잡도를 가질 수 있다.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        for j in range(i, 0, -1):
            if arr[j] < arr[j-1]: # 왼쪽의 원소(j-1)가 현재 원소(j)보다 크면
                arr[j], arr[j-1] = arr[j-1], arr[j] # 서로의 위치를 바꾼다. (현재 원소(j)를 더 왼쪽으로 위치시킨다.)
            else: # 만약 왼쪽의 원소가 현재 원소보다 작을 경우
                break # 위치를 바꾸는걸 멈추고 다음루프(i를 다루는 루프)로 진행한다.
    return arr


print(insertion_sort([7, 5, 3, 8, 0, 9, 1, 2, 4, 6]))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 퀵정렬

-   동작원리
    1. 리스트에서 첫 번째 원소를 피벗으로 정한다.
    2. 피벗을 기준으로 피벗보다 작은 원소들은 피벗의 왼쪽으로, 피벗보다 큰 원소들은 피벗의 오른쪽으로 분리한다. (make partition)
    3. 각 왼쪽, 오른쪽 분리된 배열을 상대로 위의 1단계부터 반복한다.
    4. 리스트의 길이가 1 이하가 될 때 재귀를 탈출한다.
    5. 탈출 후 각각의 파티션들을 각 자리에서 합친다.
-   시간복잡도는 평균적으로 O(NlogN)이지만 최악의 경우 O(N^2)이다.

```python
def quick_sort1(arr):
    if len(arr) <= 1: # 리스트의 길이가 1 이하일 땐 리스트 자체를 리턴 후 재귀 탈출
        return arr

    pivot = arr[0] # 첫 번째 원소를 피벗으로
    tail = arr[1:] # 피벗을 제외한 나머지 배열에서
    left_side = [v for v in tail if v <= pivot] # 피벗보다 작은건 왼쪽 배열로
    right_side = [v for v in tail if v > pivot] # 피벗보다 작은건 왼쪽 배열로 만든다.

    return quick_sort1(left_side) + [pivot] + quick_sort1(right_side) # 재귀적으로 호출 후 각 자리에서 합친 후 리턴


print(quick_sort1([7, 5, 3, 8, 0, 9, 1, 2, 4, 6]))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



def quick_sort2(arr, start, end):
    if start >= end:
        return
    pivot = start
    left = start + 1
    right = end
    while left <= right:
        while left <= end and arr[left] <= arr[pivot]:
            left += 1
        while right > start and arr[right] >= arr[pivot]:
            right -= 1
        if left > right:
            arr[right], arr[pivot] = arr[pivot], arr[right]
        else:
            arr[right], arr[left] = arr[left], arr[right]
    quick_sort2(arr, start, right-1)
    quick_sort2(arr, right+1, end)

arr = [7, 5, 3, 8, 0, 9, 1, 2, 4, 6]
quick_sort2(arr, 0, len(arr)-1)

print(arr)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 계수정렬

-   원소들의 크기 범위가 제한되어 정수 형태로만 표현할 수 있을 때 사용 가능하다.
-   일반적으로 가장 작은 데이터와 가장 큰 데이터의 차이가 1,000,000을 넘지 않을 때 효과적이다.
-   모든 범위를 담을 수 있는 크기의 리스트를 선언해야 하므로 공간복잡도에서 취약하다.
-   하지만 시간복잡도는 O(N+K)로 굉장히 빠르다. (N=원소의 개수, K=가장 큰 원소의 값)

```python
def count_sort(arr):
    a = [0]*(max(arr) + 1) # 모든 범위를 담을 수 있는 크기의 리스트를 선언
    for v in arr:
        a[v] += 1 # 값에 해당하는 인덱스마다 해당 값이 몇번 존재하는지를 기록
    result = []
    for i, v in enumerate(a):
        for j in range(v): # 해당 인덱스에서 값만큼 반복
            result.append(i) # 해당 인덱스가 result의 값이 되어 순서가 정렬됨
    return result

print(count_sort([7, 5, 3, 8, 0, 9, 1, 2, 4, 6]))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 예제

-   [카드 정렬하기](https://www.acmicpc.net/problem/1715)
-   리스트에서 가장 작은 숫자 두 개를 꺼낸 후 두 숫자를 더한 값을 다시 리스트에 넣는 상황에서
-   매 반복마다 리스트가 정렬상태를 유지해야하는 상황이라면 우선순위 큐를 이용
-   우선순위 큐에선 enqueue한 후에 바로 정렬이 완료되고 dequeue에선 마지막 인덱스의 원소를 빼내므로 가능하다.
-   python의 heapq 라이브러리를 사용하면 편리하다.

```python
import heapq

n = int(input())

arr = []

for _ in range(n):
    # enqueue할 때마다 정렬상태가 유지(최신화)된다.
    heapq.heappush(arr, int(input().rstrip()))

result = 0
if n == 1:
    print(0)
elif n == 2:
    print(arr[0] + arr[1])
else:
    while True:
        a = heapq.heappop(arr)
        b = heapq.heappop(arr)
        double = a+b
        result += double
        if len(arr) == 1:
            result += arr[0] + double
            break
        heapq.heappush(arr, double)

    print(result)

```
