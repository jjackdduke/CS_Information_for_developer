# 삽입 정렬 (Insertion Sort)



## Abstract

![img](https://cdn-images-1.medium.com/max/1600/1*IK3Q4NBRLthllMINV3OxpQ.gif)

자료 배열의 모든 요소를 앞에서 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘

2번째 원소부터 시작하여 그 앞(왼쪽)의 자료들과 비교하여 자신보다 작은 값을 만날 때까지 리스트 내 원소를 뒤로 옮기고, 작은 원소를 만나는 경우 해당 위치에 자료를 삽입하는 정렬



## 코드

### Python

```python
def insert_sort(x):
    # 1. 두번째 원소부터 마지막 원소까지 탐색을 시작
	for i in range(1, len(x)):
        # 2. 선정된 원소의 앞 원소부터 key(탐색으로 선정된 원소) 값과 비교를 시작
		j = i - 1
		key = x[i]
        # 3. 만일 앞 원소가 key값보다 크다면(단, j는 범위 내 원소)
		while x[j] > key and j >= 0:
            # 3.1. 앞 원소를 뒤로 옮기고, 비교하는 값을 한칸 앞으로 이동한다.
			x[j+1] = x[j]
			j = j - 1
        # 4. 만일 앞 원소가 key값보다 작거나 같다면, 해당 위치의 원소값으로 key값을 삽입
		x[j+1] = key
	return x
```



### JAVA

```python
void insertionSort(int[] arr)
{
   # 1. 두번째 원소부터 마지막 원소까지 탐색을 시작
   for(int index = 1 ; index < arr.length ; index++){
	  # 2. aux(선정된 원소의 앞 원소)부터 temp(탐색으로 선정된 원소) 값과 비교를 시작
      int temp = arr[index];
      int aux = index - 1;

      # 3. 만일 앞 원소가 temp값보다 크다면(단, aux는 범위 내 원소)
      while( (aux >= 0) && ( arr[aux] > temp ) ) {
		 # 3.1. 앞 원소를 뒤로 옮기고, 비교하는 값을 한칸 앞으로 이동한다.
         arr[aux + 1] = arr[aux];
         aux--;
      }
      # 4. 만일 앞 원소가 key값보다 작거나 같다면, 해당 위치의 원소값으로 key값을 삽입
      arr[aux + 1] = temp;
   }
}
```



## 시간 복잡도

**최선의 경우** 

 n개의 데이터가 전부 정렬이 되어 있을 때, 모든 원소들을 각 한번씩만 확인하기 때문에 **O(n)의 시간 복잡도**를 가진다.

**최악의 경우** 

 n개의 데이터가 전부 역순으로 정렬 되어 있을 때, n * (n - 1) / 2번 비교를 하게 되므로 시간 복잡도는 **O(n2)의 시간 복잡도**를 가지게 된다.

![{\displaystyle \sum _{i=1}^{n-1}{i}=1+2+3+4+\cdots +(n-1)={\frac {n(n-1)}{2}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6fd040d16ddcc273c6928e0e06485727f2c3c2cf)



## 공간 복잡도

주어진 배열 안에서 교환을 통해 정렬이 이루어지므로 **O(n)의 공간 복잡도**를 가진다.



## 장점

- 안정적인 정렬 방법
- 자료의 수가 적을 경우 알고리즘 자체가 매우 간단하므로 다른 복잡한 정렬 방법보다 유리할 수 있다.
- 알고리즘이 단순하다.
- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않는다. (제자리 정렬)
- 버블 정렬과 선택 정렬에 비해 빠른 정렬 방식이다.



## 단점

- 비교적 많은 자료들의 이동을 포함한다.
- 자료의 수가 많고 지료의 크기가 클 경우에 적합하지 않다.





## 출처

https://gmlwjd9405.github.io/2018/05/06/algorithm-insertion-sort.html

https://jinhyy.tistory.com/9

https://zeddios.tistory.com/20#recentComments