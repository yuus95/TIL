# 타임리프 내용정리

## 타임리프 레이아웃
- 레이아웃을 적용하면 중복되는 코드를 간소화 할 수 있다.

- 레이아웃에 적용되는 fragment( th:fragment속성을 이용하여 fragment등록)
```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<footer class="page-footer footer font-small cyan darken-3" th:fragment="footer">
    <div class="footer-copyright text-center py-3">
        2022 Shopping Mall WebSite
    </div>
</footer>
</body>
</html>
```


- 레이아웃 html
``` java
// 레이아웃
<html
        lagn="ko"
        xmlns:th="http://www.thymeleaf.org"
        xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
>
// 등록한 fragmentd위치 :: 등록한 fragment 이름
<head th:replace="fragments/header :: header"/>
<nav th:replace="fragments/navBar :: navBar"></nav>
<div layout:fragment="content"></div> // 레이아웃안에 동적으로 움직이는 내용을 content로 정의
<footer th:replace="fragments/footer :: footer"></footer>
</html>

```

- 실제 적용 페이지(html layout:decorate 이 속성에 레이아웃 파일 위치를 지정)

```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{/layouts/layout}">

<th:block layout:fragment="content"> // 동적으로 처리해야할 내용
    <div class="content">
        <div class="container center_div w-50 mt-4">
            <form  th:action="@{/login}" role="form" method="post">
            // 내용생략
            </form>
        </div>
    </div>
</th:block>
</html>

```