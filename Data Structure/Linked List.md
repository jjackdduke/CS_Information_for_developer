# Linked List

> 연속적인 메모리 위치에 저장되지 않는 선형 데이터 구조



## 0. 메모리의 구조 : Array List VS Linked List

![img](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/1335/2903.png)

### Array List

일렬로 데이터를 저장하는 방법. 만약 더 많은 공간이 필요하다면 더 많은 사람을 수용할 수 있는 공간을 생성하여 전체를 옮겨야 한다.

### Linked List

![img](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/1335/2928.png)

새로운 공간이 필요하더라도 비어있는 메모리를 할당하여 지정하여 연결

하지만 index를 알고 있는 상황에서 해당 데이터를 찾는 것은 Array List보다 느리다



# 1. Linked List

![img](https://camo.githubusercontent.com/970d0aa479a166937dd2851a01488aa6fef22044213907f1c9a45a42af51c496/68747470733a2f2f7777772e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f67712f323031332f30332f4c696e6b65646c6973742e706e67)

- 연속적인 메모리 위치에 저장되지 않는 선형 데이터 구조
- 각 노드는 **데이터 필드(Data field)**와 **다음 노드에 대한 참조(Link field)**를 포함하는 노드로 구성



## 2. Linked List를 사용하는 이유

1. 배열의 크기가 고정되어 있어 미리 요소의 수에 대한 할당을 받아야 함
2. 새로운 요소를 삽입하는 것은 비용이 많이 듦(공간을 만들고, 기존 요소 전부 이동)



## 3. Linked List의 장/단점

### 장점

1. 동적 크기 구현
2. 삽입 / 삭제 용이

### 단점

1. 임의로 액세스를 허용할 수 없음. 즉, 첫 번째 노드부터 순차적으로 요소에 액세스 해야 함(이진 검색 수행 불가능)
2. 포인터의 여분의 메모리 공간이 목록의 각 요소에 필요



## 4. Linked List 구현(파이썬)

```python
class Node:
    # data와 link(다음 노드의 주소)를 가진 Node 객체 생성
    def __init__(self,data):
        self.data=data
        self.link=None

class LinkedList:
    # Node로 구성된 Linked List 생성
    # 최초 생성 시, head 데이터를 가진 head 노드 생성
    def __init__(self):
        new_node=Node('head')
     	# Linked List의 head와 tail을 head 노드로 설정
        self.head=new_node
        self.tail=new_node
		# 현재/이전 노드를 None으로 설정
        self.current=None
        self.before=None

    # 새로운 Node를 더하는 함수 
    def append(self,data):
        new_node=Node(data)
        self.tail.link=new_node
        self.tail=new_node

    # idx의 인덱스를 가진 Node로 이동
    def move(self,idx):
        self.current=self.head.link
        self.before=self.head
        for _ in range(idx):
            if self.current==None:
                return False
            self.before=self.current
            self.current=self.current.link
        return True
	
    # idx의 인덱스에 Node를 저장
    def insert(self,idx,data):
        new_node=Node(data)
        self.move(idx)
        self.before.link=new_node
        new_node.link=self.current

    # idx의 인덱스의 노드를 제거
    def delete(self,idx):
        self.move(idx)
        self.before.link=self.current.link
	
    # idx의 인덱스의 Node의 데이터를 갱신
    def change(self,idx,data):
        self.move(idx)
        self.current.data=data

    # idx의 인덱스이 Node의 데이터를 반환
    def result(self,idx):
        if self.move(idx):
            return self.current.data
        else:
            return -1




```





