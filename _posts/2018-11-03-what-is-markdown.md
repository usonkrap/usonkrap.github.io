---
layout: post
title: "마크다운(MarkDown)에 대해 알아보자"
tags: [markdown]
excerpt_separator: <!--more-->
---

## 1. 마크다운(MarkDown)이란?
`Markdown`은 2004년 존그루버에 의해 만들어졌으며 특수기호와 문자를 이용한...
<br>  
요약하자면

- 웹상에서 좀 더 쉽고 빠르게 글쓰기를 도와주는 문법이라고 할 수 있다. 
- 파일 확장자는 `.md`
- HTML과 함께 사용도 가능하다.
- 다 필요없고 그냥 <u>사용법이나 익혀서 바로 쓰면 된다.</u>

<br><br>  
## 2. 마크다운 사용법(Cheatsheet)

<br><br>
<i class="fa fa-header fa-2x"></i>

### 헤더(Headers)
---------------------------------------------
```markdown
# 제목1 : h1
## 제목2 : h2
### 제목3 : h3
#### 제목4 : h4
##### 제목5 : h5
###### 제목6 : h6

h1과 h2는 다음과 같이 밑줄로 대체 가능

제목1 : h1
=========

제목2 : h2
---------
```
---------------------------------------------
# 제목1 : h1
## 제목2 : h2
### 제목3 : h3
#### 제목4 : h4
##### 제목5 : h5
###### 제목6 : h6

h1과 h2는 다음과 같이 밑줄로 대체 가능

제목1 : h1
=========

제목2 : h2
---------

<br><br>
<i class="fa fa-font fa-2x"></i>

### 강조(Emphasis)
---------------------------------------------
```markdown
이텔릭체(italics)는 *별표(asterisks)* 또는 _언더바(underscore)_.  
굵게(bold)는 **별표두개(asterisks)** 또는 __언더바두개(underscore)__.  
**별표(asterisks)와 _언더바(underscore)_**를 같이 사용도 가능.  
취소선(strikethrough)은 ~~물결표시(tilde)~~.
```
---------------------------------------------
이텔릭체(italics)는 *별표(asterisks)* 또는 _언더바(underscore)_.  
굵게(bold)는 **별표두개(asterisks)** 또는 __언더바두개(underscore)__.  
**별표(asterisks)와 _언더바(underscore)_**를 같이 사용도 가능.  
취소선(strikethrough)은 ~~물결표시(tilde)~~.

<br><br>
<i class="fa fa-list fa-2x"></i> 

### 목록(List)
---------------------------------------------
```markdown
1. 순서있는 목록의 첫 번째 항목
2. 두 번째 항목
    * 순서없는 목록의 서브항목
0. 사실 숫자는 별 의미없다.
    1. 또 다른 순서 있는 서브목록
    1. 또또 다른 순서 있는 서브목록
5. 또 다른 항목
- 순서없는 항목
    - 서브항목

* 순서없는 목록은 별표(asterisks)사용
- 아니면 마이너스(minus sign)를 사용하던지
+ 성격 특이하면 더하기(plus sign)를 사용해도 뭐 상관없음
```
---------------------------------------------
1. 순서있는 목록의 첫 번째 항목
2. 두 번째 항목
    * 순서없는 목록의 서브항목
0. 사실 숫자는 별 의미없다.
    1. 또 다른 순서 있는 서브목록
    1. 또또 다른 순서 있는 서브목록
5. 또 다른 항목
- 순서없는 항목
    - 서브항목

* 순서없는 목록은 별표(asterisks)사용
- 아니면 마이너스(minus sign)를 사용하던지
+ 성격 특이하면 더하기(plus sign)를 사용해도 뭐 상관없음

<br><br>
<i class="fa fa-link fa-2x"></i> 

### 링크(Link)
---------------------------------------------
```markdown
[링크](https://www.google.com)

[링크 with 링크설명](https://www.google.com "구글 홈페이지")

[상대적링크](../../../posts)

[참조링크][1] 또는 [참조링크][blog link]

URL 이나 꺽쇠기호(<,>)로 둘러싼 URL은 자동으로 링크로 변환.  
https://uosonkrap.github.io 또는 <https://uosonkrap.github.io>

[1]: https://uosonkrap.github.io
[blog link]: https://uosonkrap.github.io
```
---------------------------------------------
[링크](https://www.google.com)

[링크 with 링크설명](https://www.google.com "구글 홈페이지")

[상대적링크](../../../posts)

[참조링크][1] 또는 [참조링크][blog link]

URL 이나 꺽쇠기호(<,>)로 둘러싼 URL은 자동으로 링크로 변환.  
<https://uosonkrap.github.io> 또는 <https://uosonkrap.github.io>

[1]: https://uosonkrap.github.io
[blog link]: https://uosonkrap.github.io

<br><br>
<i class="fa fa-picture-o fa-2x"></i> 

### 이미지(Images)
---------------------------------------------
```markdown
Inline-style: 
![대체 텍스트](https://usonkrap.github.io/img/takagi.jpg) "이곳에 이미지 설명을 작성해 주세요.")

Reference-style:  
![alternative text][image]

[image]: https://usonkrap.github.io/img/this_is_not_pipe.jpg "this is not pipe"
```
---------------------------------------------
Inline-style:  
![대체 텍스트](https://usonkrap.github.io/img/takagi.jpg "이곳에 이미지 설명을 작성해 주세요."){: width="100%" height="100%"}

Reference-style:  
![alternative text][image]{: width="100%" height="100%"}

[image]: https://usonkrap.github.io/img/this_is_not_pipe.jpg "this is not pipe"

<br><br>
<i class="fa fa-code fa-2x"></i> 

### 코드(Code Block and Syntax Highlighting)
---------------------------------------------
참고로 `코드블럭(Code Block)`은 마크다운의 기능이지만  
`코드강조(Syntax Highlighting)`는 별도의 설치(ex.[highlight.js](https://highlightjs.org/static/demo/))가 필요함.
- 인라인(inline) 강조는 `(Grave)
- 블럭(block) 강조는 ```

```markdown
    이것은 `인라인 강조`.

    ```html
    <div>
      <br />
    </div>
    ```

    ```css
    h1,h2,h3 {
      color: red;
    }
    ```

    ```javascript
    function func() {
      var str = 'javascript';
      return str;
    }
    ```

    ```python
    s = "Python syntax highlighting"
    print s
    ```

    ```
    No language indicated, so no syntax highlighting in Markdown.
    But let's throw in a <b>tag</b>.
    ```
```
---------------------------------------------
이것은 `인라인 강조`.

```html
<div>
  <br />
</div>
```

```css
h1,h2,h3 {
  color: red;
}
```

```javascript
function func() {
  var str = 'javascript';
  return str;
}
```

```python
s = "Python syntax highlighting"
print s
```

```
No language indicated, so no syntax highlighting in Markdown.
But let's throw in a <b>tag</b>.
```

<br><br>
<i class="fa fa-table fa-2x"></i> 

### 표(Tables)
---------------------------------------------
- 헤더 셀을 구분 하기 위해서는 3개 이상의 `-`(dash)가 필요하다.
- `:`(colon)으로 셀 내용을 정렬 할 수 있다.
- 가장 바깥쪽 `|`(pipe)는 생략 가능하다.

```markdown
테이블 | 만들기 | 쉽다
--- | :---: | ---:
정렬도 | 잘 | 되고
_마크다운_ | 사용도 | 가능하네
가나다라마바사 | 아에이오우 |
```
---------------------------------------------

테이블 | 만들기 | 쉽다
--- | :---: | ---:
정렬도 | 잘 | 되고
_마크다운_ | 사용도 | 가능하네
가나다라마바사 | 아에이오우 |

<br><br>
<i class="fa fa-quote-left"></i> 
<i class="fa fa-quote-right"></i> 

### 인용문(Blockquotes)
---------------------------------------------
```markdown
> 인용문에는 마크다운도 적용이 가능하며 다음줄에서
> 계속해서 이어서 쓸 수도 있다.
>> 인용문 안에서도 쓸 수 있다.
>>> **_마크다운_** 문법도 사용 가능.

break

> 중단하는 자는 승리하지 못한다.
> <span> 1969년 1월 1일 대통령 박정희 <span>
```
---------------------------------------------
> 인용문에는 마크다운도 적용이 가능하며 다음줄에서
> 계속해서 이어서 쓸 수도 있다.
>> 인용문 안에서도 쓸 수 있다.
>>> **_마크다운_** 문법도 사용 가능.

break

> 중단하는 자는 승리하지 못한다.
> <span> 1969년 1월 1일 대통령 박정희 <span>

<br><br>
<i class="fa fa-arrows-h fa-2x"></i> 

### 수평선(Horizontal Rule)
---------------------------------------------
```markdown
각 기호를 세 번 이상 입력하면 됨

---
Hyphens

***
Asterisks

___
Underscores
```
---------------------------------------------
각 기호를 세 번 이상 입력하면 됨

---
Hyphens

***
Asterisks

___
Underscores

<br><br>
<i class="fa fa-level-down fa-2x"></i> 

### 줄바꿈(Line Breaks)
---------------------------------------------
```markdown
문단 구분은 엔터(enter)

줄바꿈은 문장 마지막에  //스페이스 두 번
스페이스(space) 두 번

물론 <br>태그도 사용 가능
```
---------------------------------------------
문단 구분은 엔터(enter)

줄바꿈은 문장 마지막에  
스페이스(space) 두 번

물론 <br>태그도 사용 가능

