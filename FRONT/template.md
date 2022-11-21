Html Template 요소 - 페이지를 불러온 순간 즉시 그려지지는 않지만, 이후 JavaScript를 사용해 인스턴스를
생성할 수 있는 HTML코드를 담는 방법을 제공한다. 

```
<script type = "text/template" id = "000">
 ...
</script>
Mustache.render(000,data);
```

템플릿 엔진  
	- 지정된 템플릿 양식과 데이터가 합쳐져 HTML문서를 만들어주는 소프트웨어
	- 프로그램 로직 <-> 프레젠테이션 계층을 분리하기 위한 수단
	- 프레젠테이션 계층에서 로직을 쉽게 표현하고 개발의 유연성을 향상 시킴 & 유지보수 효율 향상 


JSP(JavaServer Pages) 
 - 서버에서 complie 후 html 형식을 결과값으로 클라이언트 쪽으로 보내면 브라우저에서 전달 받은 html
을 해석한다.
- HTML내에 자바 코드를 삽입하여 웹 서버에서 동적으로 웹 페이지를 생성하여 웹 브라우저에 돌려주는 서버 사이드 
스크립트 언어이다.



## HTML TEMPLATE  WITH SERVER
```
서버에서 받아온 리스트를 이용하기
.html() 개념 중요
```

### HTML 템플릿 코드
```
<div class="tempClass">
// 이곳에 템플릿 추가
</div>


<script  type="text/template" id="simple">
    {{#data}}
    {{phone}}
    {{name}}
    {{/data}}
</script>
```

### JS 코드

```
//서버에 데이터 요청 
$.ajax({
        url: "/temp/data", // 클라이언트가 HTTP 요청을 보낼 서버의 URL 주소
        method: "GET",                           // HTTP 요청 방식(GET, POST)
        dataType: "json", // 서버에서 보내줄 데이터의 타입
        success: (data) =>{
            const tempData = {
                data: data
            }
			//페이징 렌더링
            render(tempData);
        }
    })

function render(data){
    const  html = $('#simple').html()
    const tempResult = Mustache.render(html,data)
    $('.tempClass').html(tempResult);
}
```
