`netstat -antup | grep 22`  → 이건 무슨 뜻이지?
## 파이프(`|`), 리디렉션(`<`, `>`, `>>`)
파이프(`|`)를 기준으로, 앞의 출력 값(stream, 문자열 집합)이 뒤의 입력으로 넘겨짐
## `grep`
Globally find Regular-Expression and Print. 지정된 표현식이 전체에 있는지 찾아서 프린트한다.
### 주요 옵션
- **-a:** 모든 라인을 출력합니다. 기본적으로는 일치하는 라인만 출력합니다.
- **-c:** 일치하는 라인의 개수만 출력합니다.
- **-E:** 확장된 정규 표현식을 사용합니다.
- **-i:** 대소문자를 구분하지 않습니다.
- **-n:** 일치하는 라인의 행 번호를 함께 출력합니다.
- **-v:** 패턴과 **일치하지 않는** 라인만 출력합니다.
- **-w:** 패턴이 단어로 구분되어야 합니다.
## Extra: 표준 입출력 및 표준 에러
리눅스에서는 모든 것을 파일로 표현한다.
표준 출력 = 모니터 / 표준 입력 = 키보드 / 표준 에러 = 
즉, 문자열의 집합인 파일을 표준 방법들을 통해 입출력/에러 처리하는 것이 기본 설정이다.
## Extra: 정규 표현식

## 실습 따라가기
`cat aa; cat > aa; cat aa; cat >> aa; cat aa`
- 파일 없으면 첫 줄에서 실행 막힌다.
- 두번째 명령어: 아무런 옵션이 없으므로 표준 입력으로부터 받는다. 
- ctrl + d를 입력하면 세미콜론의 다음 명령어로 넘어간다.
### `cmp`, `diff`
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217070&authkey=%21AGCMfk9JSR5Ekgk&width=251&height=193)
a 파일의 2 번째, 3번째 행이 b파일 2, 3 번째와 변경되었다는 것을 표시해주고 다른 부분을 출력한다.
#### `diff`의 `-Dflag`옵션
Dflag 옵션을 넣어주면 공통 부분은 동일하게 출력해 주지만 다른 부분은 

`#ifndef  flag`
첫 파일에서 다른 라인
`# else`
둘째 파일에서 다른 라인
`# endif` 

으로 플래그를 넣어 표시해 준다(라인 by 라인). 띄어쓰기가 다른 것도 판별할 수 있다.
### `find`
- hosts라는 이름을 지닌 파일 찾기: `sudo find / -name hosts -ls`
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217071&authkey=%21AMM6bznVqmWNhc4&width=805&height=59)
- 현재 디렉터리에서 이름에 .c가 들어가거나 .txt가 들어가는 경우 출력한다: `find . \( -name '*.c' -o -name '*.txt' \) -print`
	- `\(`: 앞에 \, escape 문자를 넣어 (를 특수 문자로 인식하지 않게 해 준다. 
	- `-o`: OR의 역할을 한다.
#### Escape 문자
파일 이름에 공백 포함된 경우,
```
find . -name 'file with space'
```
위 명령어는 `file with space`라는 이름의 파일을 찾지 못한다.
**공백은 특수 문자**이기 때문에 `find` 명령어는 공백을 파일 이름 구분자로 해석하는 것이다. **따라서 다음과 같이 이스케이프 문자 `\`를 사용해야 한다.**
```
find . -name 'file\ with\ space'
```
위 명령어는 `file with space`라는 이름의 파일을 찾는다. 이와 같이,
- `(`는 `\(`
- `)`는 `\)`
- `*`는 `\*`
- `?`는 `\?`
- `[`는 `\[`
- `]`는 `\]
라고 작성해야 유도한 대로 특수 문자를 일반 문자로 인식한다.
#### `mtime`옵션
변경된 시간을 기준으로 찾는다. 동일하게 `ctime`은 생성 시간, `atime`은 접근 시간을 기준으로 한다.
관련 포스트를 읽어보자: [리눅스 find](https://m.blog.naver.com/ooa1769/220521238103)
### `tar`: 파일 묶기
u 수정된 경우엔 붙고 안되면 붙지 않는다.
#### 옵션
- c: 압축(create)
- v: 상세 출력(Verbose)
- f: 아카이브 파일 이름 지정(File)
- t: 테이블 출력(Table)
- x: 압축 해제(eXtract)
- r: 기존 아카이브에 추가(Append)
- u: 업데이트(Update)
	기존 아카이브에 파일이 있으면 해당 파일을 열고, 없으면 새로 생성한다.
- z: 압축 알고리즘 지정(gzip)
#### 예시
- `tar -tf tarfile | grep 'a2*'` + `tar -xvf ../tarfile (a2 like)`는 다음과 같이 쓸 수 있다.
 ![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217072&authkey=%21AP3Q2nTrRIVtdek&width=708&height=15)
ㄴ 뒤에 `*`가  빠졌다.
- `tar -cvf tarfile .`: 현재 디렉터리의 내용을 압축/상세 출력/아카이브 파일 저장, 압축해서 tarfile이라는 이름으로 저장한다.
- `tar -tvf tarfile`:  아카이브 파일의 내용을 테이블 형식으로 출력
- `tar -rvf tarfile reverse.c`: 기존 아카이브인 tarfile에 reverese.c를 추가
- `tar -uvf tarfile reverse.c`:  u → 기존 아카이브에 파일이 있으면 해당 파일을 열고, 없으면 새로 생성한다.
`mkdir ./tmp`
- `tar -xvf ../tarfile`: 해당 경로에 있는 tarfile이라는 아카이브를 압축 해제한다. 해제된 파일들은 현재 파일에 생성.
- `tar -xvf ../tarfile ./reverse.c`: reverse.c 파일만 압축 해제하여 명령어가 실행되는 디렉터리에 저장
- `tar -cvf tarfile /dev/rmt0`: 해당 경로의 파일들을 압축하여 tarfile로 만든다.
- `tar -xvf /dev/rmt0`: 해당 경로에 파일을 압축 해제한다.