# 타임리프 내용정리

## 타임리프 레이아웃
- 레이아웃을 적용하면 중복되는 코드를 간소화 할 수 있다.

``` java
// 레이아웃
<html
        lagn="ko"
        xmlns:th="http://www.thymeleaf.org"
        xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
>

<head th:replace="fragments/header :: header"/>
<nav th:replace="fragments/navBar :: navBar"></nav>
<div layout:fragment="content"></div>
<footer th:replace="fragments/footer :: footer"></footer>
</html>

```