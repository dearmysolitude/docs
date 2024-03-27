## Array를 가진 Array를 비교하여 sorting하기

파이썬에서는 별도의 조작 없이 list를 비교해주던 것과 달리, 자바에서는 Arrays의 sort() 메서드에 두 번째 인자를 람다식으로 전달해줌으로써 배열의 sorting을 할 수 있다.

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

배열을 여러 차원에서 비교하기 위한 예제 코드: 람다식을 적극적으로 사용하였다.
```java
public static void main(String[] args) {
	String[][] arr = {{"lee", "99"}, {"kim", "75"}, {"park", "100"}};
	
	Arrays.sort(arr, (a, b) -> Arrays.compare(a, b));
	for(String[] row : arr) {
		System.out.println(Arrays.toString(row));
	}
	
	Arrays.sort(arr, Comparator.comparingInt(a -> Integer.parseInt(a[1])));
	for(String[] row : arr) {
		System.out.println(Arrays.toString(row));
	}
}
```

좀 더 잘 활용할 수 있도록 자바의 Comparator와 람다식을 공부해보자.
