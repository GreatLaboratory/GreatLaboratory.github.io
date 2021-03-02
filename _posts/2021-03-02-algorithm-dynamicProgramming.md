---
title: "알고리즘 - dynamic programming"
excerpt: "dynamic programming"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2021-03-02
---

## 다이나믹 프로그래밍

-   시간 복잡도가 엄청나게 큰 상황에서 메모리 공간을 약간만 더 사용하면 시간 복잡도를 비약적으로 줄일 수 있을 때 사용하는 방법이 다이나믹 프로그램이이다. 해당 기법을 사용하기 위해선 2가지의 조건을 만족해야 한다.
-   조건1 : 큰 문제를 작은 문제로 나눌 수 있어야 한다.
-   조건2 : 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일해야 한다. (같은 문제를 한 번씩만 풀자!)
-   아래 코드는 단순히 피보나치 수열을 재귀함수로 구현한 것이다.

```python
def fibonacci_recursive(n):
    if n == 1 or n == 2:
        return 1
    return fibonacci_recursive(n-1) + fibonacci_recursive(n-2)

print(fibonacci_recursive(6)) # 8
```

-   위 코드의 재귀 스택 호출 과정을 트리구조로 확인하면 아래와 같이 f(3)이나 f(4)는 이미 앞서 연산이 이루어졌음에도 새롭게 연산을 한다. 이러한 알고리즘은 일반적으로 O(2^N)의 시간 복잡도를 가지게 되어 N이 30만 되더라도 약 10억 가량의 연산을 수행해야 한다.
    ![image](https://user-images.githubusercontent.com/46255148/108828981-ecd40300-760a-11eb-8c77-ef6cf3d970bb.png)

-   이러한 문제는 시간 복잡도가 엄청나게 큰 상황이며 큰 문제를 작은 문제로 나눌 수 있고(f(n) = f(n-1) + f(n-2)), 작은 문제에서 구한 정답이 그것을 포함하는 큰 문제에서도 동일하다. (f(4)를 구할 때 구한 f(3)은 f(5)를 구할 때 사용한 f(3)과 그 결과값이 같다.)
-   따라서 이는 다이나믹 프로그래밍으로 해결 가능하다.
-   아래는 탑다운 방식(메모이제이션) 기법을 사용한 코드와 재귀 스택 호출 과정을 트리구조로 보여준 것이다. 트리구조에서 알 수 있듯이 점선으로 된 노드들은 단순히 저장되어있는 값을 반환할 뿐 연산을 하지않는다. 따라서 다이나믹 프로그래밍 기법(탑다운)의 시간 복잡도는 O(N)이다.

```python
def fibonacci_recursive_dp(n):
    dp_table = [0] * (n+1)
    if n == 1 or n == 2:
        return 1
    if dp_table[n] == 0: # 한 번도 하지 않았던 연산이라면
        dp_table[n] = fibonacci_recursive_dp(n-1) + fibonacci_recursive_dp(n-2) # 연산을 한다.

    # dp_table[n]에 0이 아닌 수가 이미 들어와있다면 연산이 이미 수행되었으므로
    # fibonacci_recursive_dp(n-1) + fibonacci_recursive_dp(n-2)연산을 다시 하지 않는다.
    return dp_table[n]

print(fibonacci_recursive_dp(6)) # 8
```

![image](https://user-images.githubusercontent.com/46255148/108832538-0414ef80-760f-11eb-850e-a57ab22ec8d0.png)

-   하지만 이렇게 재귀 함수를 사용하면 컴퓨터 시스템에서 함수를 다시 호출했을 때 (아무리 단순 저장값 반환 작업이더라도) 메모리 상에 적재되는 일련의 과정을 따라야 하므로 오버헤드가 발생할 수 있다.
-   이를 해결하기 위해선 재귀의 방식이 아닌 반복문으로 알고리즘을 짜야하고, 일반적으로 반복문을 이용한 다이나믹 프로그래밍의 성능이 더 좋다.
-   아래는 반복문 방식의 다이나믹 프로그래밍(바텀업) 기법이다.

```python
def fibonacci_iterative_dp(n):
    dp_table = [0] * (n+1)
    dp_table[1], dp_table[2] = 1, 1

    # 세번째, 네번째 ... 낮은 인덱스부터 최종 구하려는 인덱스까지 순서대로 값이 정해져서 바텀업
    for i in range(3, n+1):
        dp_table[i] = dp_table[i-1] + dp_table[i-2]

    # dp_table[n]은 위 반복문의 가장 마지막 순회에서 구해지므로 시간 복잡도는 탑다운과 마찬가지로 O(N)이다.
    return dp_table[n]

print(fibonacci_iterative_dp(6)) # 8
```

## 예제1 - 1로 만들기

![image](https://user-images.githubusercontent.com/46255148/108976580-03419380-76cb-11eb-9b26-643e5efe2d34.png)
![image](https://user-images.githubusercontent.com/46255148/108976669-18b6bd80-76cb-11eb-8d07-fbb0a2e14783.png)

-   f(x)가 정수 x를 1로 만드는 연산횟수의 최솟값이라면, f(26)은 f(25), f(13) 중에 가장 작은 값에 1을 더한 값이다.
-   다시 말해, 구하려는 f(x)은 f(x-1), f(x//5), f(x//3), f(x//2) 중 가능한 값들 중 최솟값에서 1을 더한 값이 된다. (총 연산의 횟수를 최소로 해야하므로)
-   1을 더하는건 연산횟수를 증가시키는 것이다.
-   큰 문제(f(26))는 작은 문제(f(25), f(13))들로 나눌 수 있으며, f(26)을 구할 때 사용하는 f(25)와 f(50)을 구할 때 사용하는 f(25)는 항상 같은 값을 갖기에 다이나믹 프로그래밍이 가능하다.

```python
def solution(x):
    d = [0] * 30001

    # 매 반복마다 각 숫자의 1로 가는 연산 최솟값이 정해진다.
    for i in range(2, x+1):
        # ex) 만약 i가 30이라 f(30)구한다면
        d[i] = d[i-1] + 1  # 1을 빼는 연산은 항상 가능하므로 최초의 d[i]후보는 f(29)가 된다.
        # 30은 5로 나누어 떨어지므로 [f(29)+1, f(6)+1] 중 가장 작은 것을 택한다.
        if d[i] % 5 == 0:
            d[i] = min(d[i], d[i//5] + 1)
        # 30은 3으로 나누어 떨어지므로 [f(29)+1, f(6)+1, f(10)+1] 중 가장 작은 것을 택한다.
        if d[i] % 3 == 0:
            d[i] = min(d[i], d[i//3] + 1)
        # 30은 2로 나누어 떨어지므로 [f(29)+1, f(6)+1, f(10)+1, f(15)+1] 중 가장 작은 것을 택한다.
        if d[i] % 2 == 0:
            d[i] = min(d[i], d[i//2] + 1)

    return d[x]


print(solution(26))  # 3
```

## 예제2 - 개미 전사

![image](https://user-images.githubusercontent.com/46255148/108975510-e8baea80-76c9-11eb-8678-206e0587e3fd.png)
![image](https://user-images.githubusercontent.com/46255148/108975305-b1e4d480-76c9-11eb-81b6-7b4419e5c6fa.png)

-   식량창고가 4개일 땐 2번째 식량창고까지의 최대약탈값에서 4번째 식량창고의 식량을 더한 값과 3번째 식량창고까지의 최대약탈값을 비교하여
-   더 큰 값을 취한 것이 식량창고가 4개일 때 약탈할 수 있는 최댓값이 된다.
-   또한 6번째 식량창고까지의 최대약탈값을 구할 때 4번째 식량창고까지의 최대약탈값이 재활용되며 그 값이 당연히 똑같기에 다이나믹 프로그래밍을 적용한다.

```python
def solution(n, arr):
    # d배열의 각 인덱스의 값은 길이가 arr앞에서부터 인덱스만큼일 때 최댓값이다.
    # ex) d[3]은 arr배열의 앞에서부터 3개를 자른 배열에서 얻을 수 있는 식량의 최댓값이다.
    d = [0] * 101

    d[1] = arr[0]  # 따라서 arr의 길이가 앞에서부터 1일 때는 무조건 그 하나의 원소가 최댓값이고
    d[2] = max(arr[0], arr[1])  # arr의 길이가 앞에서부터 2일 때는 두 개의 원소 중 더 큰 값이 최댓값이다.

    # 다이나믹 프로그래밍 (바텀업)
    for i in range(3, n+1):
        # 만약 i가 3이라면 이전에 구했던 f(2)가 더 큰지, f(1)에서 새롭게 나타난 숫자인 arr[3]을 더한 값이 더 큰지를 비교한다.
        d[i] = max(d[i-1], d[i-2]+arr[i-1])
    return d[n]


print(solution(4, [1, 3, 1, 5]))  # 8
print(solution(9, [1, 3, 1, 5, 6, 8, 9, 11, 14]))  # 32
```
