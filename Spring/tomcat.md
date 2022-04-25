# 톰캣과 웹서버

- 톰켓
	- Web Server + Web Container
	- 웹서버(아파치)
        - 웹서버란:
            - 소프트트웨어입장: 웹 브라우저와 같은 클라이언트로부터 Http요청을 받아들이고, HTML문서와 같은 페이지 웹페이지를 반환하는 컴퓨터 프로그램
            - 하드웨어입장: 위에 언급한 기능을 제공하는 컴퓨터 프로그램을 실행시키는 컴퓨터 
    - 웹컨테이너 :
        - 웹 컨테이너(web container, 또는 서블릿 컨테이너)는 웹 서버의 컴포넌트 중 하나로 자바 서블릿과 상호작용한다. 웹 컨테이너는 서블릿의 생명주기를 관리하고, URL과 특정 서블릿을 맵핑하며 URL 요청이 올바른 접근 권한을 갖도록 보장한다

        - 웹 컨테이너는 서블릿, 자바서버 페이지(JSP) 파일, 그리고 서버-사이드 코드가 포함된 다른 타입의 파일들에 대한 요청을 다룬다.

        - 웹 컨테이너는 서블릿 객체를 생성하고, 서블릿을 로드와 언로드하며, 요청과 응답 객체를 생성하고 관리하고, 다른 서블릿 관리 작업을 수행한다.

    - 서블릿
        - 자바를 사용하여 웹페이지를 동적으로 만들어 생성하는 서버측 프로그램
        - 자바서블릿은 웹 서버이 성능을 향상하기 위해 사용되는 클래스의 일종이다.

        - 서블릿은 요청-응답 프로그래밍 모델을 통해 접근한 애플리케이션을 호스트하는 서버의 기능을 확장하는 데 사용되는 자바 프로그래밍 언어 클래스입니다