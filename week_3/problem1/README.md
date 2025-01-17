# 문제1. 요격 시스템

프로그래머스 레벨 2

https://school.programmers.co.kr/learn/courses/30/lessons/181188

## 문제설명

미사일은 [시작x좌표, 끝x좌표] 형태로 나타냄

요격미사일은 해당 x좌표에 있는 모든 미사일을 요격함(단, 정확히 시작좌표와 끝좌표면 요격 불가)

필요한 요격 미사일의 최솟값

<img src="https://github.com/user-attachments/assets/be8bc775-2a8c-423e-a318-786a9b65f5c2" alt="image" style="width: 50%; height: 50%;">

### 입력

targets : 미사일 정보. [[시작,끝], [시작,끝], ...]

### 출력

answer : 필요한 요격 미사일 수의 최솟값

### 제한사항

<img src="https://github.com/user-attachments/assets/5cca31b7-c569-4cd4-959a-d1110a46a3b5" alt="image" style="width: 50%; height: 50%;">

## 풀이

끝 좌표 순서로 sort하고 cover되지 않은 미사일의 끝좌표보다 약간 앞(epsilon, e)에 요격 미사일을 배치(Greedy Algorithm)

### Correctness of Greedy Algorithm

#### Greedy Choice Property

sorted_targets T에서 T[0]의 끝점의 좌표를 T[0].f, 시작점의 좌표를 T[0].s라고 하고 greedy choice에 의해 선택된 첫 번째 좌표는 g0 = T[0].f-e, 이 좌표를 포함하지 않는 optimal한 요격 미사일 좌표의 set을 M 이라고 하자.

이 때 M은 T[0]를 cover하므로 M[0]<T[0].f이고 M[0] < g0 < T[0].f 이다. (without loss of generality)

M에 의해 cover되는 미사일의 set을 MT(=T), M[0]에 의해 cover되는 미사일의 set을 MT0, g0에 의해 cover되는 미사일의 set을 GT0라고 할 때,

M0[0].s <= M0[1].s <= ... <. M0[-1].s < M[0] < g0 < M0[0].f = T[0].f 이므로 MT0 ⊂ GT0 이다.

M' = M-{M[0]} ∪ {g0} 일 때 M'에 의해 cover되는 미사일의 set을 M'T라고 할 때, 

|T| = |MT| = |MT - MT0 ∪ MT0| <= |MT - MT0 ∪ GT0| = |M'T| <= |T| 이므로 |M'T| = |T| 이다.

또한 |M| = |M'| 이므로 M' 역시 optimal solution 이고, greedy choice에 의해 선택된 g0를 포함하는 optimal solution은 항상 존재한다.

따라서 Greedy Choice Property가 성립

#### Optimal Substructure

greedy choice로 선택된 첫 번째 좌표를 g0라고 하고, g0에 의해 cover되는 미사일의 set를 GT0, T-GT0를 cover하는 optimal solution을 O', G[0]를 포함하는 optimal solution을 O라고 할 때, 

|{g0} ∪ O'| > |O| 라고 하자.(즉, G가 optimal solution이 아니라고 하자.)  --(1)

O'와 O-{g0}는 모두 T-GT0를 cover하고, 정의에 의해 O'는 T-GT0의 optimal solution이므로 |O'| <= |O-{g0}|라서 O에서 O-{g0}를 O'로 대체할 수 있고,

|O| >= |{g0} ∪ O'|이다.  --(2)

(1)과 (2)에서 |{g0} ∪ O'| <= |O| < |{g0} ∪ O'| 이고 이는 모순이다.

따라서 |{g0} ∪ O'| <= |O| 이고 O는 optimal solution이므로 |{g0} ∪ O'| >= |O| 이므로 |{g0} ∪ O'| = |O| 이다.

즉, optimal substructure가 성립한다.

#### 따라서 greedy algorithm을 사용할 수 있다.

### 코드
```
def solution(targets):
    answer = 0
    T=sorted(targets, key=lambda x: x[1])
    ## 주석처리한 부분은 미사일 전체를 출력하고 싶을 때
    # G=[T[0][1]]
    g=T[0][1]
    answer=1
    for t in T[1:]:
        # if t[0] >= G[-1]:
        #     G.append(t[1])
        if t[0] >= g:
            g=t[1]
            answer+=1
    # answer=len(G)
    return answer
```
#### 분석

sorting에서 O(n lgn)  (radix sort의 경우 O(n), memory complexity 증가)

for문 안은 O(1)이므로 for문 전체는 O(n)

따라서 전체 O(n lgn) (radix sort를 하면 O(n))

## 후기

greedy algorithm의 가장 기본 예제 중 하나인 activity selection problem과 매우 유사한 형태라서 금방 한듯

코드 구현도 바로 해서 5분정도 걸림
