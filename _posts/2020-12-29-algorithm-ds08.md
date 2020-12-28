---
title: "알고리즘 - 자료구조(8)"
excerpt: "Tree 개념"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-29
---

## 트리(Tree)

![image](https://user-images.githubusercontent.com/46255148/103190615-14616480-4915-11eb-9e7e-dfb44bc018e6.png)

-   스택, 큐 등은 선형인 1차원의 자료구조인데 트리는 2차원의 자료구조이다. 일렬로 데이터들이 늘어서있지 않는다.
-   데이터의 검색엔진, 데이터베이스 시스템 등에서 자주 사용된다.
-   순환하는 길이 없는 그래프(graph)
-   트리란? node와 edge를 이용하여 데이터의 배치 형태를 추상화한 자료구조
-   용어
    -   부모 노드 & 자식 노드 & 서브 트리
    -   노드의 수준 (root가 level0)
    -   노드의 깊이 (최하단의 level + 1)
    -   노드의 차수 (자식 노드의 수)
-   종류
    -   이진 트리(Binary Tree) : 모든 노드의 차수가 2 이하인 트리
    -   포화 이진 트리(Full Binary Tree) : 모든 레벨에서 노드들이 모두 채워져 있는 이진 트리 (깊이가 k면 모든 노드의 개수가 2^k -1인 이진 트리)
        ![image](https://user-images.githubusercontent.com/46255148/103190666-4f639800-4915-11eb-828d-0ba21567b50a.png)
    -   완전 이진 트리(Complete Binary Tree) : 깊이가 k일 때 레벨k-2까지는 모든 노드가 2개의 자식을 가진 포화 이진 트리. 레벨k-1에선 왼쪽부터 노드가 순차적으로 채워져 있는 이진트리.
        ![image](https://user-images.githubusercontent.com/46255148/103190690-630efe80-4915-11eb-9870-7ef7aed2057b.png)
