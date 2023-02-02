## JS

- querySelector
    - Document.querySelector()는 제공한 선택자 또는 선택자 뭉치와 일치하는 문서 내 첫 번째 Element를 반환합니다. 일치하는 요소가 없으면 null을 반환합니다.
    - 가장 먼저 발견한 선택자를 가져온다.
        ```html
        <div class = test>1</div>
        <div class = test>2</div>
        <div class = test>3</div>
        <!-- 1의 value를 가진 div를 가장 먼저 발견 -->
        ```

#### querySelector 사용예시
```javascript
// toggle 을 이용하여 class 추가하기
const testClass = document.querySelector(".test-class")
const testH1 = document.querySelectorAll(".test-class h1")

console.log(testH1)
testClass.addEventListener('click',function(){
    if(testClass.classList.contains("test-font-class")){
        testClass.classList.remove("test-font-class")
    }else{
        testClass.classList.add("test-font-class")
        // 기존의 클레스에 test-font-class를추가
    }
    console.log(testClass.classList)
})
``` . 


- padStart(String.padStart)
    - 글자의 형식을 맞춰준다(prefix를 추가해주는기능)
    ```
    "1".padStart(2,"0") --> 01로 변환된다/
    ```



##### INPUT  RADIO

`input 태그에서 type 속성이 RADIO일 경우 name 속성이 같은 이름들의 input들 중에서 하나를 선택할 수 있다. `

- 개념 
```
여러 개의 항목 중에서 하나만 선택하도록 하는 동그란 버튼을 라디오 버튼이라고 한다.
라디오 버튼은 input 태그로 만든다.
```


- 테스트 코드 
```
<div class = "teest">
    <div id = "01">
        <input type = "RADIO" name = "test">
        <label> 1번 선택 </label>
    </div>
    <div id = "02">
        <input type = "RADIO" name ="test"/>
        <label> 2번 선택 </label>
    </div>
    <div id = "03">
        <input type = "RADIO" name ="test" />
        <label> 3번 선택 </label>
    </diiv>
</div>
```
s