# Array

> **배열(array)이란?**
>
> 동일한 자료형으로 선언된 데이터 공간을 메모리 상에 연속적으로 나열한 것

- 데이터를 저장하기 위해 여러 개의 변수를 선언할 수 있지만, 하나의 배열 이름으로 여러 개의 변수들을 다루어 코드를 간단하게 처리할 수 있다.
- 자바에서는 배열 공간은 자유 메모리 영역인 Heap 영역에 할당하도록 지정되어 있으며, 배열 변수는 할당된 배열 공간의 주소를 이용해 인덱스(순번, Index)를 참조하는 방식으로 값들을 처리하도록 정해두었다. 
  - heap 영역은 프로그래머가 직접 공간을 할당, 해제하는 메모리 공간
    - 선입선출(FIFO, First-In First-Out) 방식으로, 가장 먼저 들어온 데이터가 가장 먼저 인출
      (힙 영역이 메모리의 낮은 주소에서 높은 주소의 방향으로 할당되기 때문)
    - 런 타임에 의해 크기가 결정됨



## 배열의 선언과 생성

> 배열의 선언 : 배열 공간의 주소 저장용 참조(Reference) 

- 타입[] 변수 이름;

  - int[] arr;

- 타입 변수 이름[]

  - int arr;

  

> 배열의 생성 : Heap 영역에 값을 저장하는 변수들을 연속 나열 할당하고, 발생한 배열 공간의 시작 주소를 선언된 배열 레퍼런스에 대입한다.

- 배열 참조 변수 = new 데이터타입 [연속 할당될 변수 갯수];

- 배열 생성할 때, 배열의 길이(첨자)를 지정해 주면 해당 데이터 타입의 값을 저장할 수 있는 저장 공간을 길이 갯수 만큼 연속 할당한다. 
  - 데이터 타입[] 변수 = new 데이터타입 [첨자];
    - String[] strArr = new String[3];



## 배열의 초기화

> 배열 초기화 시에는 배열의 초기값을 기록하는 것

- 배열 초기화 시에는 배열의 길이를 따로 지정하지 않고 초기화에 사용되는 값의 개수가 자동 배열의 길이로 처리된다.
- 자바에서는 배열 공간의 초기화를 따로 지정하지 않아도 자동 초기화 처리가 되도록 각 자료형 별 기본값이 준비되어 있다.
  - 데이터타입[] 배열참조변수 = {값1, 값2, 값3, ...};
  - 데이터타입[] 배열참조변수 = new 데이터타입[] {값1, 값2, 값3, ...};



## 배열의 인덱스

> 생성된 배열 공간의 각 저장공간을 요소(Element)라고 부르며, '배열이름[인덱스]' 형식으로 요소에 접근할 수 있다.

- 배열의 길이(Length) : 배열의 저장 공간의 개수
- 배열의 인덱스 : 0부터 시작해서 (배열의 길이 - 1)까지 지정된다.

```java
public class TestArrayLength {
    public static void main(String[] args) {
        String strArr[] = {"Apple", "Banana", "Orange"};
        
        System.out.println("배열의 길이는 " + strArr.length + "입니다.");
    for (int i = 0; i < strArr.length; i++)
        System.out.println(strArr[i]);
    }
}
```



## 배열의 복사

> **얕은 복사(Shallow Copy)** : 배열의 복사는 참조하는 배열 공간의 주소를 다른 참조변수에 복사
>
> **깊은 복사(Deep Copy)** : 배열 공간의 값들을 다른 배열 공간에 하나씩 복사하는 깊은 복사

- 얕은 복사

  - 배열의 주소를 복사한 경우에는 두 개의 배열 레퍼런스가 같은 대상을 공유

  ```java
  01 public class TestdeepCopy {
  02     public static void main(String[] args) {
  03         String originArr[] = {"Apple", "Banana", "Orange"};
  04         String destArr[];
  05
  06         destArr = new String [originArr.length];
  07
  08         for(int i = 0; i < originArr.length; i++)
  09             destArr[i] = originArr[i];
  10
  11         for(int i = 0; i < originArrlength; i++)
  12             System.out.println(i + " : " + originArr[i] + ", " + destArr[i];
  13     }
  14 }
  ```

  

- 깊은 복사

  - for문을 이용하여 별도의 배열 공간에 하나씩 복사하는 방법

  ```java
  01 public class TestdeepCopy {
  02     public static void main(String[] args) {
  03         String originArr[] = {"Apple", "Banana", "Orange"};
  04         String destArr[];
  05
  06         destArr = new String [originArr.length];
  07
  08         for(int i = 0; i < originArr.length; i++)
  09             destArr[i] = originArr[i];
  10
  11         for(int i = 0; i < originArrlength; i++)
  12             System.out.println(i + " : " + originArr[i] + ", " + destArr[i];
  13     }
  14 }
  ```

  

  - arrcopy() 함수를 이용하는 복사 : 
    - System.arraycopy(src, srcPos, dest, destPos, length)
    - 매개변수(src : 원본 배열, srcPos : 원본 배열 시작 인덱스, dest : 복사할 배열, destPos : 복사할 배열 시작 인덱스, length : 복사할 길이)
    - for문을 이용한 복사보다 arraycopy()함수를 이용한 방법이 더 효율적

  ```java
  01 public class TestDeepCopy {
  02     public static void main(String[] args) {
  03         String originArr[] = {"Apple", "Banana", "Orange");
  04         String destArr[] = new String [originArr.length];
  05
  06     System.arrayCopy(originArr, 0, destArr, 0, originArr.length);
  07
  08     for(int i = 0; i < destArr.length; i++)
  09         System.out.println(i + " : " + originArr[i] + ", " + destArr[i];
  10    }
  11 }
  ```

  





## 참조

https://velog.io/@normal114/Chapter-7.-%EA%B8%B0%EB%B3%B8%EC%9E%90%EB%A3%8C%ED%98%95-%EB%B0%B0%EC%97%B4