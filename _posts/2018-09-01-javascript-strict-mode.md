---
layout: post
title: "JavaScript의 'use strict'란? (strict mode)"
tags: [javascript]
excerpt_separator: <!--more-->
---

자바스크립트 소스코드를 보다보면 가장 처음에 `use strict;`를 선언한 것을 종종 볼 수 있다.
<!--more-->  
strict mode를 사용한다라는 뜻인데 strict mode가 무엇이고 왜 쓰는지를 알아보자.

<br>

--------------------------------------------------

## 1. Strict Mode

별거 없다. 단어 뜻 그대로 코드를 `엄격한 모드`에서 사용한다라는 뜻이다.  
즉, 다른 언어에 비해 자유도(??)가 높은 자바스크립트에서 코드가 개판이 되는걸 미리 좀 관리해보자란 의미라고 볼 수 있다.  
간단히 말해 개발자가 실수하기 쉬운 `잘못된 문법`들을 미리 알려준다라고 생각하자.  
~~strict라고 그닥 엄격하진 않다~~  

**strict mode**는 `ECMAScript5`부터 적용되었다. 지원하는 브라우저는 다음과 같다.

브라우저 | 버전
------|:---:
Chrome | 13 이상
Internet Explorer | **10 이상**
Edge | 12 이상
Firefox | 4 이상
Safari | 5 이상
Opera | 12.1 이상

참고로 `ECMAScript6`는 내부적으로 항상 **strict mode**로 동작하므로 써 줄 필요가 없지만, 아직은 시기상조.  
기술의 발전은 빠르지만, 아직도 윈도우XP에 IE8 이하 버전을 사용하는 사람들이 많다는걸 생각하자.

<br>

--------------------------------------------------

## 2. 선언 방법 (Declaring Strict Mode)
**strict mode**의 선언 방법에는 두 가지가 있다.

- `전역(global) 선언` : 모든 소스코드가 대상

```javascript
"use strict";
x = 1;   // 변수 x가 선언되지 않았기 때문에 에러
myFunc();

function myFunc() {
    y = 2;  // 변수 y도 선언되지 않았기 때문에 에러
}
```

- `로컬(local) 선언` : 오직 함수 내에서만 대상

```javascript
x = 1;   // strict mode가 아니므로 에러 아님
myFunc();

function myFunc() {
    "use strict";
    y = 2;  // 함수내 strict mode이므로 에러
}
```

<br>

--------------------------------------------------

## 3. 제한되는 경우(Not Allowed in Strict Mode)

백문이 불여일견. 코드로 직접 보는게 이해가 빠르다.

##### 1. 암시적 변수 금지
```javascript
'use strict';
var foo = 111;  // var 선언 필수 
bar = 222;       // Uncaught ReferenceError: foo is not defined
```
<br>

##### 2. 읽기전용 프로퍼티(property)에 값 설정 금지
```javascript
'use strict';
var obj1 = {};
Object.defineProperty(obj1, 'x', { value: 42, writable: false });
obj1.x = 100;   // Uncaught TypeError: Cannot assign to read only property 'x' of object '#<Object>'
```
<br>

##### 3. getter만 있는 프로퍼티에 대입 금지
```javascript
'use strict';
var obj2 = { get x() { return 17; } };
obj2.x = 5;   // Uncaught TypeError: Cannot set property x of #<Object> which has only a getter
```
<br>

##### 4. 확장불가 객체(object)에 신규 프로퍼티 할당 금지
```javascript
'use strict';
var fixed = {};
Object.preventExtensions(fixed);
fixed.newProp = "ohai";   // uncaught TypeError: Can't add property newProp, object is not extensible
```
<br>

##### 5. 삭제 불가능 프로퍼티 삭제 금지
```javascript
"use strict";
delete Object.prototype;   // Uncaught TypeError: Cannot delete property 'prototype' of function Object() { [native code] }
```
<br>

##### 6. 함수의 매개변수 중복 금지
```javascript
'use strict';
function x(p1, p1) {};   // Uncaught SyntaxError: Duplicate parameter name not allowed in this context
```
<br>

##### 7. 변수의 삭제 금지
```javascript
'use strict';
var obj1 = 123;
delete obj1;   // Uncaught SyntaxError: Delete of an unqualified identifier in strict mode.
```
<br>

##### 8. 8진수 표시 금지
```javascript
'use strict';
var x = 077;    // Uncaught SyntaxError: Octal literals are not allowed in strict mode.
```
<br>

##### 9. eval 또는 arguments를 변수로 사용 금지
```javascript
'use strict';
var eval = 123;   // Uncaught SyntaxError: Unexpected eval or arguments in strict mode
```
```javascript
'use strict';
var arguments = 123;   // Uncaught SyntaxError: Unexpected eval or arguments in strict mode
```
<br>

##### 10. with 키워드 사용 금지
```javascript
'use strict';
with (Math) {   // Uncaught SyntaxError: Strict mode code may not include a with statement
    x = cos(2)
};
```
<br>

##### 11. 몇몇 식별자는 예약어로 설정되어 있으므로 사용 금지
- implements
- interface
- let
- package
- private
- protected
- public
- static
- yield

```javascript
'use strict';
var interface = 100;    // Uncaught SyntaxError: Unexpected strict mode reserved word
};
```
<br>


--------------------------------------------------

> 잠재적 오류를 알려주므로 `use strict;` 쓰고 시작하자.