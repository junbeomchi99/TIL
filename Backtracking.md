# 백트래킹 알고리즘

참고 자료: https://blog.encrypted.gg/945

참고 자료: https://hongjw1938.tistory.com/88

## 정의:
 현재 상태에서 가능한 모든 후보군을 따라 들어가며 <b>탐색</b>하는 알고리즘 (시뮬레이션/선택지 게임 생각하면 이해하기 편함)

## 재정의 
= 재귀적으로 하나씩 풀어나가되 현재 재귀를 통해 확인 중인 상태(노드)가 제한 조건에 위배되는지 (유망하지 않은지) 판단하고 그러한 겨웅 해당 상태(노드) 를 제외하고 다음 단계로 나아가는 방식 

DFS 개념과 비슷하며 보통 recursion function (재귀함수)를 이용하여 진행함. 

For 문으로 비슷한 코드를 구현할수 있지만, 백트래킹 알고리즘이 코드상 훨씬 간결하고 "가지치기 (pruning)"이 가능하여서 전체 풀이 시간을 단축할수있다

- 가지치기(pruning) = 유망하지 않은 (더 이상 탐색할 필요가 없는) 상태 (노드)를 제외하는 행위
- 이를 통하여 모든 경우의 수를 체크 하지 않고 필요한 것만 체크하여 전체 풀이 시간을 단축 할수있음

## 예시 문제:
- https://www.acmicpc.net/submit/15649 

기존 세팅하기
```python 
n, m = map(int, input().split())

comb = []
visited = [False] * (n)
```
재귀적인 반복 (recursive call)을 통하여서 모든 선택지를 확인함.
```python
def search(x):
    if x == m:
        print(*comb)
        return
    for i in range(n):
        if visited[i] == False:
            visited[i] = True
            comb.append(i+1)
            search(x+1)
            visited[i] = False
            comb.pop()
search(0)
```