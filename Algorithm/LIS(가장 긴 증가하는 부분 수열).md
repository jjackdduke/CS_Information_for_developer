# LIS (Longest Increasing Sequence)

> 배열의 부분 수열 중, 각 **원소가 이전 원소보다 크다는 조건을 만족**하고, 그 **길이가 최대**인 부분 수열을 

#### - 예시) {6, 2, 5, 1, 7, 4, 8, 3}

증가하는 수열은 {2, 5}, {2, 7} 등 여러 수열이 존재하지만 그 길이가 최대인 수열은 {2, 5, 7, 8}

**따라서 LIS는 {2, 5, 7, 8}**



## 구현 방법 1. DP

```python
# A : 자연수의 집합 / N : 배열의 길이
A = [6, 2, 5, 1, 7, 4, 8, 3]
N = len(A)

# DP : LIS의 길이를 저장하는 리스트, 각 자리의 위치까지의 숫자를 확인하는 값이므로 항상 최솟값은 1(자기 자신을 포함하므로)
DP = [1 for _ in range(N)]

for i in range(1, N):
    for j in range(i):
        # 이미 완성된 DP[j]에 A[i] 값을 이어 붙일 수 있는 경우 DP[j] + 1의 값을 가질 수 있다.
        if A[j] < A[i]:
            DP[i] = max(DP[i], DP[j] + 1)

print(max(DP))


```

### 시간 복잡도 :

0 + 1 + 2 + 3 + ... + (n - 1) = n * (n - 1) / 2 이므로 O(n2)



## 구현 방법 2. Binary

```python
# 12015번
import bisect

arr = [6, 2, 5, 1, 7, 4, 8, 3]
n = len(arr)

# dp를 arr[0]으로 초기화한다. => dp : 매 상황에서의 가장 큰 원소를 저장하는 리스트
dp = [arr[0]]

for i in range(n):
    # 현재 위치 i가 이전 위치의 원소들보다 크면 dp에 추가한다.
    if arr[i] > dp[-1]:
        dp.append(arr[i])

    # 현재 위치가 이전 원소보다 작거나 같으면, bisect.bisect_left로 이전 위치의 원소 중 가장 큰 원소의 index 값을 구한다.
    else:
        idx = bisect.bisect_left(dp, arr[i])
        # 이후, 해당 위치의 숫자를 갱신 => 즉 매번, 해당 길이(i)에 해당하는 부분 수열의 최댓 값을 갱신
        dp[idx] = arr[i]

print(len(dp))
```

### 시간 복잡도 :

- 모든 리스트를 한번씩 탐색해야 하므로 n
- 해당 위치의 리스트를 확인하기 위해 이진 탐색을 사용 : log(n)
  - 따라서 시간 복잡도는 O(nlog(n))



## 구현 방법 3. LIS 리스트를 구하기

```python
import bisect   # 배열 이진 분할 알고리즘
arr = [6, 2, 5, 1, 7, 4, 8, 3]
n = len(arr)

dp = [1 for i in range(n)]

for i in range(n):
    for j in range(i):
        if arr[i] > arr[j]:
            dp[i] = max(dp[i], dp[j] + 1)

max_dp = max(dp)
print(max_dp)


max_idx = dp.index(max_dp)
lis = []

while max_idx >= 0:
    if dp[max_idx] == max_dp:
        lis.append(arr[max_idx])
        max_dp -= 1
    max_idx -= 1

lis.reverse()
print(*lis)
```

