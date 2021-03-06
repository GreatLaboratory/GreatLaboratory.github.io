---
title: "알고리즘 - greedy"
excerpt: "당장 좋은 것만 선택하는 greedy"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-11-28
---

## Greedy 개념

-   말 그대로 어떠한 문제가 있을 때 단순 무식하게, 탐욕적으로 문제를 푸는 알고리즘이다.
-   현재 상황에서 지금 당장 좋은 것만 고르는 방법을 의미하는데 이 때 현재의 선택이 나중에 미칠 영향에 대해서는 고려하지 않는다.
-   그리디 알고리즘은 창의력, 즉 문제를 풀기 위한 최소한의 아디이어를 떠올릴 수 있는 능력을 요구한다.
-   가장 다이렉트로 문제의 해결에 접근하게끔 한다.
-   이 해결에 도달하려면 가장 탐욕적인 기준을 정립해야 한다. '가장 큰 화폐단위부터', '정렬 후 가장 큰 숫자만을', '나눗셈을 최대한 많이 수행' 등의 탐욕적인 기준을 세워야 한다.
-   정당성 분석 또한 중요하다. 단순히 가장 좋아 보이는 것을 반복적으로 선택해도 최적의 해를 구할 수 있는지를 꼭 검토해야 한다.

## 예제 - 거스름돈

-   **문제** : 카운터엔 거스름돈으로 사용할 500, 100. 50. 10원짜리 동전이 무한히 존재한다. 거슬러 줘야 할 돈이 N원일 때 거슬러 줘야 하는 동전의 최소 개수를 구해라. 단, N은 항상 10의 배수이다.
-   **탐욕적인 기준** : 가장 큰 화폐 단위부터 돈을 거슬러 주기
-   **시간 복잡도** : 화폐의 종류가 N개라 했을 때 O(N)
-   **정당성** : 가지고 있는 동전 중에서 큰 단위가 항상 작은 단위의 배수이므로 작은 단위의 동전들을 종합해 다른 해가 나올 수 없기 때문에 정당하다. (만약 배수 관계가 없다면 가장 큰 단위부터 조합을 짜서 최소의 개수를 찾는 for문 돌리면 될 듯)

```python
def solution(N):
    answer = 0
    coinList = [500, 100, 50, 10]
    while True:
        for coin in coinList:
            answer += N//coin
            N %= coin
        if N == 0:
            break
    return answer

print(solution(1260))  # 6
```

## 예제 - 큰 수의 법칙

-   **문제** : 큰 수의 법칙이란 다양한 수로 이루어진 배열이 있을 때 주어진 수들을 M번 더하여 가장 큰 수를 만드는 법칙이다. 단 배열의 특정한 인덱스에 해당하는 수가 연속해서 K번을 초과하여 더해질 수 없다. 또한 서로 다른 인덱스에 해당하는 수가 같은 경우에도 서로 다른 것으로 간주한다. 배열과 숫자가 더해지는 횟수 M과 K가 주어질 때 결과를 출력하시오.
-   **탐욕적인 기준** : 배열에서 다루는 숫자는 가장 큰 수와 두번째로 큰 수면 된다. 배열의 길이만큼 반목문을 돌리지 않아도 된다. 반복되는 수열이 존재함을 캐치하여 하나의 연산으로 처리한다.
-   **시간 복잡도** : 배열의 길이와 상관없이 하나의 연산으로 끝나므로 O(1)
-   **정당성** : 반복되는 수열이 존재하고 예외적으로 나오는 랜덤 숫자가 존재하지 않기 때문에 정당하다.

```python
def solution(L):
    numList = L[1]
    M = L[0][0]
    K = L[0][1]

    numList.sort()
    iterableGroup = (M // (K + 1)) * (K * numList[-1] + numList[-2])
    restGroup = (M % (K + 1)) * numList[-1]

    return iterableGroup + restGroup

print(solution([[8, 3], [2, 4, 5, 4, 6]]))  # 46 = 6+6+6+5+6+6+6+5
print(solution([[7, 2], [3, 4, 3, 4, 3]]))  # 28 = 4+4+4+4+4+4+4
```
