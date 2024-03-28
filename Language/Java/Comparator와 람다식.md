## 발화: Array를 가진 Array - 내부 array의 요소를 사용하여 sorting하기
### 문제
Score Card라는 예제 코드를 작성하는 문제는 간단하다. 배열로 입력받은 이름과 점수에 대하여, score card를 이름, 점수에 따라 오름차순/내림차순으로 sorting하는 문제이다. 
### 해결
- 파이썬에는 배열을 가지는 자체를 sorting해주는 기능이 사용하기 쉽게 구현되어 있어 기능 검색만으로 배열을 쉽게 ordering할 수 있었다.
- 자바에서는 이러한 기능을 Java Collections Framework에서 제공한다.
- 실제 문제 해결은 다음과 같다: 비교 요소를 객체화 후 자료구조에 넣어 순차 접근하여 속성의 크기 비교.
	1. Score Card들을 클래스로 만들었다.
		- 이름, 점수를 필드로 가진다.
		- 기존의 sorting 메서드를 사용하여 객체를 필드 속성으로 비교하는 메서드를 추가한다.
	2. 객체로 생성 후, 객체들을 ArrayList에 넣었다.
	3. 결과물을 넣을 ArrayList를 새로 만든다. 기존 ArrayList를 돌며 하나씩 결과 ArrayList에 추가한다.
		- 결과물 ArrayList 첫 요소부터 접근하여 넣을 객체와 비교하여 왼쪽에 넣을지 결정하고 왼쪽에 넣을 요소를 찾지 못하면 결과 ArrayList 제일 끝에 추가한다.
	- 모든 ArrayList의 요소에 접근해야 하므로 O(n)의 시간 복잡도를 가진다. 요소가 많지 않으므로 이런 방식으로 푸는 것이 가능하다.
	- **더 많은 요소를 처리하기 위해서는 더 나은 성능을 보이는 sorting 알고리즘을 사용하는 것이 좋다.**
- 문제를 이렇게 해결한 것과는 별개로, 자바에서 배열을 속성에 따라 ordering하는 기능은 자주 활용할 수 있는 내용이라고 생각하여 Arrays의 sort 메서드와 Comparable인터페이스, sort가 인자로 받는 람다식에 대해서 좀 더 공부해보기로 한다.
```java
// scoreCard의 것이 더 앞선다면 양수를 반환
public int compareWithName(ScoreCard scoreCard) { 
	return name.compareTo(scoreCard.getName());
}

// scoreCard의 점수보다 낮다면 true 반환
public boolean compareWithScore(ScoreCard scoreCard) {
	return score < scoreCard.getScore();
}
```
## Comparator/Comparable
Comparable과 Comparator에 대한 내용은 Collections 인터페이스를 공부할 때 잠깐 살펴본 적이 있다. 
- Comparator/Comparable: 둘 다 인터페이스로, 이를 사용하고자 한다면 인터페이스 내의 메서드를구현해야 사용할 수 있다.
- `Interface Comparable<T>`: 이 인터페이스는 이를 구현하는 모든 객체에 대한 ordering을 제공한다.
- `Interface Comparator<T>`: 객체들의 collection에 대한 전체적인 ordering을 제공한다.
즉, 특정 객체의 compareTo 를 사용하기 위해서는 그 클래스가 Comparable을 구현하고 있어야 한다.
## 람다식
익명 함수. 인라인으로 쓰여지는 함수이며, 객체지향 언어인 자바에서 함수형 프로그래밍을 할 수 있도록 JDK 8부터 API를 제공한다.
## `Arrays`의 사용
Arrays는 Java Collections Framework의 기능 중 하나로, 배열을 다루는, sorting, searching 같은 기능들을 포함한다. 구현자가 이 클래스에서 요구하는 사양을 준수하는 한, 알고리즘의 구현은 구현자에게 달려있다: 그 예로, sort()메서드는 MergeSort로 구현되어 있다.
[Java docs: Arrays](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Arrays.html)
### 람다식을 인자로 받는 sort()
[Java docs: sort(T, Comparator)](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Arrays.html#sort(T%5B%5D,java.util.Comparator))

### 람다식을 받는 sort 메서드와 Comparator

#### 예제 1: 이중 배열 내부의 숫자 비교하기
```java
import java.util.Arrays;

public class ArraySorting {
    public static void main(String[] args) {
        int[][] arr = {{5, 2, 1}, {9, 3, 7}, {4, 6, 8}};
        
        // 2차원 배열 정렬
        Arrays.sort(arr, (a, b) -> Arrays.compare(a, b));
        
        // 정렬된 배열 출력
        for (int[] row : arr) {
            System.out.println(Arrays.toString(row));
        }
    }
}
```
#### 예제 2: 여러 속성을 가지는 배열을 비교하기
배열을 여러 차원에서 비교하기 위한 예제 코드. 람다식을 사용하였다.
```java
public static void main(String[] args) {
	String[][] arr = {{"lee", "99"}, {"kim", "75"}, {"park", "100"}};
	
	Arrays.sort(arr, (a, b) -> Arrays.compare(a, b));
	for(String[] row : arr) { // 배열을 출력
		System.out.println(Arrays.toString(row));
	}
	
	Arrays.sort(arr, Comparator.comparingInt(a -> Integer.parseInt(a[1])));
	for(String[] row : arr) { // 배열을 출력
		System.out.println(Arrays.toString(row));
	}
}
```
