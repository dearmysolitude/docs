html 페이지에 대한 스타일 적용에는 중복 제거가 주요한 이슈이다. id는 페이지마다 고유하지만, class는 여러 block 객체들에 대한 정의이다.
## 외부 스타일 시트의 사용
•페이지에서 CSS3 스타일 시트 파일을 불러 사용
•동일한 스타일 시트를 웹 페이지마다 중복 작성 해소
•웹 사이트의 전체 웹 페이지 모양의 **일관성 확보**
### link 태그 혹은 @import
1. `link` 태그: 좀 더 빠름
```html
<head>
  <link href="mystyle.css" type="text/css" rel="stylesheet">
</head>
```
2. `@import`
```html
<style>
  @import url(mystyle.css);
  /* @import url(‘mystyle.css’); 로 해도 됨 */
  /* @import “mystyle.css”;로 해도 됨 */
</style>
```
## 셀렉터
```html
<!DOCTYPE html>

<html>
    <head>
        <title>셀렉터 만들기</title>
        <style>
            h3, li { /* 태그 이름 셀렉터 */
                color: brown;
            }

            div > div> strong { /* 자식 셀렉터 */
                background: yellow;
            }

            ul strong { /* 자손 셀렉터 */
                color: dodgerblue;
            }

            .warning { /* class 셀렉터 */
                color: red;
            }

            body.main { /* class 셀렉터 */
                background: aliceblue;
            }

            #list { /* id 셀렉터 */
                background: mistyrose;
            }

            #list span { /* 자손 셀렉터 */
                color: forestgreen;
            }

            h3:first-letter{ /* 가상 클래스 셀렉터 */
                color:red;
            }

            li:hover { /* 가상 클래스 셀렉터 */
                background: yellowgreen;
            }
        </style>
    </head>
    <body class="main">
        <h3>Web Programming</h3>
        <hr>
        <div>
            <div>2학기<strong>학습 내용</strong>입니다.</div>
            <ul id="list">
                <li><span>HTML5</span></li>
                <li><strong>CSS</strong></li>
                <li>JAVASCRIPT</li>
            </ul>
            <div class="warning">60점 이하는 F</div>
        </div>
    </body>
</html>
```