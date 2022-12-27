# 퀵 정렬 (Quick Sort)



## Abstract

![img](https://cdn-images-1.medium.com/max/1600/1*wwCw5TzLd79k2WQ6YVsQVw.gif)

퀵 정렬은 '찰스 앤터니 리처드 호어(Charles Antony Richard Hoare)'가 개발한 정렬 알고리즘으로 **real-world 데이터에서 빠르다고 알려져 있어 가장 많이 쓰는 알고리즘**이다.

퀵 정렬은 pivot을 선정하여 pivot의 value를 기준으로 좌측으로 작은 값을 우측으로 큰값을 재배치 하고 계속하여 분할하여 정렬하는 알고리즘

퀵 정렬은 분안정 정렬 / 분할 정복 알고리즘에 속하며, 다른 원소와의 바교만으로정렬을 수행하는 비교 정렬에 속한다.



## Detail

### 퀵 정렬의 단계

- **분할(Divide)** : 입력 배열을 피벗을 기준으로 비균등하게 2개의 부분 배열(피벗을 중심으로 왼쪽 : 피벗보다 작은 요소들, 오른쪽 : 피벗보다 큰 요소들)로 분할한다.
- **정복(Conquer)** : 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않으면 순환 호출을 이용하여 다시 분할 정복 방법을 적용한다.
- **결합(Combine)** : 정렬된 부분 배열들을하나의 배열에 합병한다.
- 순환 호출이 한번 진행될 때마다 최소한 하나의 원소(피벗)는 최종적으로 위치가 정해지므로, 이 알고리즘은 반드시 끝난다는 것을 보장할 수 있다.



![img](https://gmlwjd9405.github.io/images/algorithm-quick-sort/quick-sort.png)





## 코드

### Python

```python
def quicksort(x):
    # 1. 분할된 데이터의 크기가 1 이하인 경우 더 이상 정렬을 진행하지 않는다.
    if len(x) <= 1:
        return x

    # 2. 데이터의 중앙에 위치한 데이터를 pivot으로 설정
    pivot = x[len(x) // 2]
    # 3. less : pivot보다 작은 값의 데이터의 집합 / more : pivot보다 큰 값의 데이터의 집합 /
    # equal : pivot과 동일한 값의 데이터의 집합
    less = []
    more = []
    equal = []
    # 4. 집합 내 각 원소들을 탐색하며 크기에 따라 less / more / equal에 분류하여 넣는다.
    for a in x:
        if a < pivot:
            less.append(a)
        elif a > pivot:
            more.append(a)
        else:
            equal.append(a)
	# 모든 정렬이 끝나면 less / more을 분할 정복을 실시하고 최종적으로 나온 값을 less > equal > more	  순서로 정렬하여 반환
    return quicksort(less) + equal + quicksort(more)
```

### Python (Cache 없이)

```python
def partition(arr, start, end):
    # 1. 데이터의 첫번째 원소를 pivot으로 설정
    pivot = arr[start]
    # 2. pivot을 기준으로 다음 원소를 left / 마지막 원소를 right로 설정
    left = start + 1
    right = end
    done = False
    while not done:
        # 3. left가 pivot과 right보다 작거나 같으면 left는 다음 원소(우측)를 탐색
        while left <= right and arr[left] <= pivot:
            left += 1
        # 4. right가 pivot과 left보다 크거나 같으면 right는 다음 원소(좌측)를 탐색
        while left <= right and pivot <= arr[right]:
            right -= 1
        # 5. right와 left가 이동하여 서로 만나게 되는 지점(right < left)의 경우 반복문을 종료
        if right < left:
            done = True
        # 6. 3, 4과정을 전부 거친 후에도 right와 left가 만나지 않는다면 두 지점의 숫자를 바꾸고 해당 			지점(left / right)들부터 다시 반복문을 실시.
        else:
            arr[left], arr[right] = arr[right], arr[left]
    # 7. right와 pivot값을 교환하면 좌측에는 작은 값, 우측에는 큰 값들이 배치되어 있다.  
    arr[start], arr[right] = arr[right], arr[start]
    # 8. right의 index를 반환한다 > 해당 index를 기준으로 분할 정복 실시
    return right


def quick_sort(arr, start, end):
    if start < end:
        # partition으로 정렬하여 반환된 값을 기준으로 분할 정복 실시
        pivot = partition(arr, start, end)
        quick_sort(arr, start, pivot - 1)
        quick_sort(arr, pivot + 1, end)
    return arr
```



### JAVA

```python
public void quickSort(int[] arr, int left, int right) {
    // base condition
    if (left >= right) {
        return;
    }
    int pivot = arr[right];
    
    int sortedIndex = left;
    for (int i = left; i < right; i++) {
        if (arr[i] <= pivot) {
            swap(arr, i, sortedIndex);
            sortedIndex++;
        }
    }
    swap(arr, sortedIndex, right);
    quickSort(arr, left, sortedIndex - 1);
    quickSort(arr, sortedIndex + 1, right);
}

private void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```



## 시간 복잡도

**최선의 경우**

![img](https://gmlwjd9405.github.io/images/algorithm-quick-sort/sort-time-complexity-etc1.png)

- 순환 호출의 깊이 : k = log2(n)
- 각 순환호출 단계의 비교 연산
  - 각 순환 호출에서는 전체 리스트의 대부분의 레코드를 비교해야 하므로 평균 n번 정도의 비교가 이루어진다.
- 최종 시간 복잡도  = 순환 호출의 깊이 * 각 순환 호출 단계의 비교 연산 = nlog2(n) > O(nlogn)

**최악의 경우** 

![img](https://gmlwjd9405.github.io/images/algorithm-quick-sort/sort-time-complexity-etc2.png)

- 리스트가 계속 불균형하게 나누어지는 경우(특히, 이미 정렬된 리스트에 대하여 퀵 정렬을 실행하는 경우)
  - 순환 호출의 깊이 : n
  - 각 순환 호출 단계의 비교 연산 : n
- 최종 순환 횟수 = 순환 호출의 깊이 * 각 순환 호출 단계의 비교 연산 = n^2 > O(n2)



## 공간 복잡도

정렬을 위해 평균 적으로 O(log n)만큼의 memory를 필요로 한다. 이는 재귀적 호출로 발생하는 것이며 최악의 경우 O(n)의 공간 복잡도를 보인다.



## 장점

- 불필요한 데이터의 이동을 줄이고 먼 거리의 데이터를 교환할 뿐만 아니라, 한번 결정된 피벗들이 추후 연산에서 제외되는 특성 때문에, 시간 복잡도가 O(nlog n)를 가지는 다른 정렬 알고리즘과 비교했을 때도 가장 빠르다.
- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리공간을 필요로 하지 않는다.

## 단점

- **불안정 정렬(Unstable Sort)**이다.
  - 안정 정렬(Stable Sort) : 중복된 값을 입력 순서와 동일하게 정렬
  - 불안정 정렬(Unstable Sort) : 중복된 값을 입력 순서와 상관없이 무작위로 뒤섞인 상태에서의 정렬

- 정렬된 배열에 대해서는 Quick Sort의 불균형 분할에 의해 오히려 수행시간이 더 많이 걸린다.





## 출처

https://jinhyy.tistory.com/9

https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC

https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html

https://velog.io/@good159897/%EC%95%88%EC%A0%95-%EC%A0%95%EB%A0%AC-VS-%EB%B6%88%EC%95%88%EC%A0%95-%EC%A0%95%EB%A0%AC-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%9D%B8%ED%84%B0%EB%B7%B0