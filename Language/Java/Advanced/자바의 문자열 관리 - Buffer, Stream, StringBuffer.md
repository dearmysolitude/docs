## Byte
Byte 자료형은 메모리 바이트와 동일한 크기를 담고 있어 Byte, Byte[] 는 C에서 char, char[]와 같은 의미를 가지고 있다. 문자열이나 비트 연산을 수행하여 데이터를 처리하고자 할 때 byte로 데이터를 저장하거나 조작한다.

> [!Note]
> String 👉 Byte
> ```java
> String a = "한글";
> Byte[] b = a.getBytes();
> ```
> Byte 👉 String
> ```java
> String c = new String(b, 0, 5); 
> // String으로 바꿀 바이트 배열의 시작 인덱스와 마지막 인덱스를 넣어준다.
> ```

## Buffer와 Stream
- File, socket, 웹 관련 클래스들은 Buffer, Stream 클래스로 데이터를 주고받는다.
- Buffer/Stream은 일정 크기에 메모리 공간을 가지고 데이터를 처리하는 클래스이다.
### Buffer
- 데이터를 일시적으로 저장하는 메모리 공간이다. 입출력 작업 시 성능 향상을 위해 사용된다.
- 버퍼에 데이터를 쌓아 둔 후 한 번에 입출력 작업을 수행하여 시스템 호출 횟수를 줄인다.
- 자바에서는 ByteBuffer, CharBuffer, DoubleBuffer 등의 클래스로 제공한다.
- 버퍼는 고정 크기의 배열로 구현되어 있다.
- `put()`메서드로 데이터를 저장하고 `get()`메서드로 데이터를 읽는다.
### Stream
- 데이터의 소스와 목적지 사이를 연결하는 채널이다.
- 입출력 작업 시 데이터를 전송하는 데 사용된다.
- 자바에서는 InputStream, OutputStream, Reader, Writer 등의 클래스로 제공한다.
- 스트림은 바이트 단위/문자 단위로 데이터를 처리한다.
- `read()`메서드로 데이터를 읽고, `write()` 메서드로 데이터를 쓴다.
### Buffer와 Stream
둘 다 입출력(I/O) 작업과 관련된 개념이다.
- 버퍼는 데이터를 일시적으로 저장하는 공간이지만, 스트림은 데이터를 전송하는 채널이다.
- 버퍼는 고정 크기이지만 스트림은 고정된 크기를 가지지 않는다.
일반적으로 이 둘은 함께 사용하여 입출력 작업의 효율성을 높인다. 데이터를 버퍼에 쌓아두었다가 한 번에 스트림을 통해 전송하여 성능을 향상시킬 수 있다.
## StringBuffer
String 클래스의 메모리 처리 비합리성을 개선하기 위해 StringBuffer가 추가되었다. String의 변경이 자주 일어나는 경우 StringBuffer 형을 사용한다.
String 데이터형은 immutable(불변)이기 때문에([[14. Java의 String 클래스|Java가 String을 불변으로 취급하는 이유]]) 메모리 상에서 변경을 하려면 새로운 메모리 공간에 저장하여 그 데이터를 가르키는 형태로 변수 초기화가 이루어진다. 이런 특징 때문에 변경이 수 천, 수 만번 발생하는 경우에 대한 string에 대한 처리는 프로그램에 부담을 준다.
### StringBuffer의 특징
- String 처럼 문자형 배열(char[])을 내부적으로 가지고 있다.
- String 클래스와 달리 내용을 변경할 수 있다.
- 인스턴스를 생성할 때 버퍼(배열)의 크기를 충분히 지정해주는 것이 좋다: 작으면 성능 저하 발생하기 쉽다.
- String 클래스와 달리 `equals()`를 오버라이딩하지 않는다.
```java
public static void main(String[] args) {
	StringBuffer sb = new StringBuffer("abc");
	StringBuffer sb2 = new StringBuffer("abc");
	
	System.out.println(sb == sb2); // flase
	System.out.println(sb.equals(sb2)); // false

	String s = sb.toString();
	String s2 = sb2.toString();
	System.out.println(sb.equals(s2)); // true
}
```
### 주요 메서드
- `StringBuffer(String str)`: 생성자
- `append()`: 여러 메서드가 오버로딩 되어있음, 문자열 추가
- `delete(int start, int end)`: 해당 index의 문자열 제거
- `toString()`: String 형태로 변경

## 참고
-------------------
자바에서 문자열과 관련된 인코딩 처리는 다음과 같이 동작합니다.

1. 입력 문자열(String or StringBuffer)의 인코딩
    - 자바에서 문자열 리터럴이나 String 객체는 UTF-16 인코딩을 사용합니다.
    - StringBuffer로 생성된 문자열 역시 내부적으로는 UTF-16 인코딩을 사용합니다.
2. toString() 메서드를 통해 StringBuffer -> String 변환시
    - toString() 메서드는 StringBuffer 내부의 문자 배열을 새로운 String 객체로 복사합니다.
    - 이때 String 객체 역시 UTF-16 인코딩을 유지합니다.
3. 문자열 출력시
    - 콘솔이나 파일 등으로 출력할 때는 기본 플랫폼의 기본 인코딩(보통 UTF-8)을 사용하여 변환됩니다.
    - 이때 필요에 따라 인코딩 변환이 수행됩니다. (UTF-16 -> 플랫폼 기본 인코딩)

따라서 toString() 메서드를 통해 StringBuffer에서 String으로 변환할 때, 인코딩 정보 자체는 변경되지 않고 유지됩니다. 다만 최종적으로 출력할 때 플랫폼의 기본 인코딩으로 변환되는 과정이 있습니다.
만약 특정 인코딩을 명시적으로 지정하고 싶다면 String 생성자나 getBytes() 메서드 등을 사용하여 인코딩을 지정할 수 있습니다.

```java
byte[] bytes = "Hello".getBytes("UTF-8"); // UTF-8 인코딩된 바이트 배열 String str = new String(bytes, "UTF-8"); // UTF-8 인코딩된 문자열 객체`
```
-------------------