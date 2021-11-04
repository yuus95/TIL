# React 입문

## JS 기초
[참고문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

### 1. 변수

```javascript
**let**을 사용하면 블록 유효 범위 변수를 선언 할 수 있습니다. 선언 된 변수는 변수가 포함 된 함수 블록에서 사용할 수 있습니다.

let a;
let name = 'simon';

// myLetVariable는 여기에서 보이지 *않습니다*

for (let myLetVariable = 0; myLetVariable < 5; myLetVariable++) {
  // myLetVariable는 여기에서 유효합니다
}

// myLetVariable는 여기에서 보이지 *않습니다*

---

**const**는 값이 변경되지 않는 변수를 선언 할 수 있게 합니다. 변수는 변수가 선언 된 함수 블록에서 사용할 수 있습니다.
const Pi = 3.14; // 변수 Pi 설정
Pi = 1; // 상수로 설정된 변수는 변경 할 수 없기 때문에 애러 발생.

---

**var**은 가장 일반적인 변수 선언 키워드입니다. let, const 키워드가 가지는 제한을 var은 갖지 않습니다. 이는 자바스크립트에서 변수를 선언하는 전통적인 유일한 방법이었기 때문입니다. var 키워드로 선언 된 변수는 변수가 선언 된 함수 블록에서 사용 할 수 있습니다.

var a;
var name = 'Simon';

// myVarVariable는 여기에서 사용 할 수 *있습니다*

for (var myVarVariable = 0; myVarVariable < 5; myVarVariable++) {
  // myVarVariable는 함수 전체에서 사용 할 수 있습니다.
}

// myVarVariable는 여기에서 사용 할 수 *있습니다*


자바스크립트와 자바 같은 다른 언어 사이의 중요한 차이점은 자바스크립트는 블록에 범위가 없다는 것입니다. 함수에만 범위가 있습니다. 
```


### 객체
```javascript
JavaScript 객체는 간단히 이름-값 쌍(name-value pairs)의 모임입니다.

// 생성하는방법
var obj = new Object();
var obj = {};

// 전체적인 구조 
var obj = {
    name: "Carrot",
    "for": "Max",
    details: {
        color: "orange",
        size: 12
    }
}

obj.details.color 이렇게 접근도 가능함
obj["details"]["size"]; 이렇게도 가능


//객체 프로토타입, 인스턴스 생성

function Person(name, age) {
  this.name = name;
  this.age = age;
}

// 객체를 정의한다
var you = new Person('You', 24);
// "You"라는 이름의 24세인 새로운 사람을 생성중이다.



```

### 내장함수

```javascript
내장 함수 (Inner functions)
다른 함수의 내부에서 JavaScript 함수를 선언할 수 있습니다. 우리는 makePerson() 함수 초기 버전에서 이것을 한번 본적이 있습니다. JavaScript에서 중첩 함수(nested functions)의 중요한 세부사항은 부모 함수 범위의 변수에 접근할 수 있다는 사실입니다:

function parentFunc() {
    var a = 1;

    function nestedFunc() {
        var b = 4; // parentFunc은 사용할 수 없는 변수
        return a + b;
    }
    return nestedFunc();  // 5
}

```

### 클로저

```javascript
클로저(closure)
클로저(closure)란, 외부 함수에 접근할 수 있는 내부 함수 혹은 이러한 원리를 일컫는 용어인데 스코프에 따라서 내부함수의 범위에서는 외부 함수 범위에 있는 변수에 접근이 가능하지만 그 반대는 실현이 불가능하다는 개념이다.


function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
//myFunc변수에 displayName을 리턴함
//유효범위의 어휘적 환경을 유지
myFunc();
//리턴된 displayName 함수를 실행(name 변수에 접근)

```


## 시작하기

```bash
# Node.js 설치
# Yarn 설치

npx create-react-app my-app

```



