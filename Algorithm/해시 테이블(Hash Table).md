# 해시 테이블(Hash Table)

### 1. 해시 구조

Hash Table : 키(Key)에 데이터(Value)를 저장하는 데이터 구조

- Key를 통해 바로 데이터를 받아올 수 있으므로, 속도가 획기적으로 빨라짐
- 파이썬 딕셔너리(Dictionary) 타입이 해쉬 테이블의 예
- 보통 배열로 미리 Hash Table 사이즈만큼 생성 후에 사용(공간 복잡도와 시간 복잡도를 맞바꾸는 기법)



### 2. 알아둘 용어

- 해시(Hash) : 임의 값을 고정 길이로 변환하는 것
- 해시 테이블(Hash Table) : 키값의 연산에 의해 직접 접근이 가능한 데이터 구조
- 해싱 함수(Hashing Function) : Key에 대한 산술 연산을 이용해 데이터 위치(해시 값 혹은 해시 주소)를 찾을 수 있는 함수
- 해시 값(Hash Value) 또는 해시 주소(Hash Address) : Key를 해싱 함수로 연산하여 해시 값을 알아내고, 이를 기반으로 해시 테이블에서 해당 Key에 대한 데이터 위치를 일관성 있게 찾을 수 있음
- 슬롯(Slot) : 한 개의 데이터를 저장할 수 있는 공간

![img](https://www.fun-coding.org/00_Images/hash.png)



### 3. 해시 테이블의 장단점 & 주요 용도

- 장점
  - 데이터 저장 / 읽기 속도가 빠르다. (검색 속도가 빠르다)
  - 해시는 케애 대한 데이터가 있는지(중복) 확인이 쉬움
- 단점
  - 일반적으로 저장공간이 좀 더 많이 필요하다
  - 여러 키에 해당하는 주소가 동일할 경우 충돌을 해결하기 위한 별도 자료 구조가 필요함
- 주요 용도
  - 검색이 많이 필요한 경우
  - 저장, 삭제, 일기가 빈번한 경우
  - 캐시 구현 시 (중복 확인이 쉽기 떄문)



#### 4. hash_table 구현

1. 해시 함수 : key % 8
2. 해시 키 생성 : hash(data)

```python
hash_table = [0 for i in range(8)]

# 데이터에 따른 key 값을 반환하는 함수
def hash_func(data):
    return hash(data) % 8

# 키 값을 해시 값으로 두고 해당 위치에 데이터를 저장
def set_data(data,value):
    key = hash_func(data)
    hash_table[key] = value

# 데이터를 통해 얻은 key 값에 저장되어 있는 데이터를 반환
def get_data(data):
    key = hash_func(data)
    return hash_table[key]
```

> 상기 함수의 경우, 서로 다른 데이터에서도 동일한 key 값을 반환할 수 있다. 이를 **해시 충돌(Hash Collision)**이라고 한다.
>
> 해시 충돌을 해결하기 위한 기법 : Chaining 기법 & Linear Probing 기법

### 5. Chaining 기법

```python
hash_table = [0 for i in range(8)]

# 데이터에 따른 key 값을 반환하는 함수
def hash_func(key):
    return key % 8

# 키 값을 해시 값으로 두고 해당 위치에 데이터를 저장
def set_data(data,value):
    # hash_idx : 해시 값 / hash_key : 데이터를 통해 얻은 key값
    hash_idx = hash(data)
    hash_key = hash_func(hash_idx)
    
    # hash_key 값에 다른 값이 존재하는 경우
    if hash_table[hash_key] != 0:
        # 해당 hash_key 값의 hash_table의 모든 원소를 탐색
        for i in range(len(hash_table[hash_key])):
            # 만일 hash_key값 내 원소 [hash_idx, value] 중 hash_idx가 있을 때, 해당 value를 갱신
            if hash_table[hash_key][i][0] == hash_idx:
                hash_table[hash_key][i][1] = value
                return True
        # 값이 없을 경우, 해당 [hash_idx, value]를 저장
        hash_table[hash_key].append([hash_idx,value])
        return True
   	# hash_key에 해당하는 원소가 없을 때, 해당 [hash_idx, value]를 저장
    else:
        hash_table[hash_key] = [[hash_idx,value]]
        return True

# 데이터를 통해 얻은 key 값에 저장되어 있는 데이터를 반환
def get_data(data):
    hash_idx = hash(data)
    hash_key = hash_func(hash_idx)
    if hash_table[hash_key]:
        for value in hash_table[hash_key]:
            if value[0] == hash_idx:
                print(value[1])
                return value[1]
        print("None")
        return None
    else:
        print("None")
        return None
```









### 참고

https://study-grow.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-hash-%EA%B5%AC%ED%98%84-python