---
layout: post
title: "require와 import의 차이"
subtitle:
tags: [javascript]
excerpt_separator: <!--more-->
---

코드 가장 위에 패키지를 읽어들이는 놈이 `require`와 `import`가 있다.

### require

자바스크립트 자체가 지원하는 패키지 읽는 방법

### import

ES6의 사양으로 새로운 패키지 읽는 방법

<br>

정리하고 보니 딱히 별 내용이 없네...CommonJS와 ES6의 차이임.

---

### _참고_

##### 불러오기

-   require

    ```javascript
    var foo = require("foo");
    var bar = requre("foo").bar;
    ```

-   import
    ```javascript
    import foo from "foo";
    import { bar } from "foo";
    ```

##### 내보내기

-   module.exports
    ```javascript
    module.exports = foo;
    exports.bar = bar;
    ```
-   export
    ```javascript
    export default foo;
    export { bar };
    ```
