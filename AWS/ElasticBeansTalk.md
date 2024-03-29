# 빈즈톡

* 사용이유
 - 애플리케이션을 실행하는 인프라에 대해 자세히 알지 못해도 AWS 클라우드에서 애플리케이션을 신속하게 배포하고 관리할 수 있다.
 
 - 선택 또는 제어에 대한 제한 없이 관리 복잡성을 줄일 수 있다.
    - 애플리케이션을 업로드하기만 하면 Elastic Beanstalk에서 용량 프로비저닝, 로드 밸런싱, 조정, 애플리케이션 상태 모니터링에 대한 세부 정보를 자동으로 처리한다.

 
 ```
 내가 느낀점은 ec2에 직접 배포하는것 보다 매우 편리하다.
 실제로 작업을 해보니 로드밸런싱을 구현하는게 생각보다 쉬웠다.
 ec2 3개를 따로따로 구성할 필요 없이 한번에 오토스케일링 처리를 할 수 잇는 장점도 존재한다.
 ```

* 사용방법
    - Elastic Beanstalk는 Go, Java, .NET, Node.js, PHP, Python 및 Ruby에서 개발된 애플리케이션을 지원합니다. 애플리케이션을 배포할 때, Elastic Beanstalk가 선택된 지원 가능 플랫폼 버전을 구축하고 Amazon EC2 등의 AWS 리소스를 하나 이상 프로비저닝하여 애플리케이션을 실행합니다.

* 주요 개념
    - 애플리케이션: 애플리케이션은 환경, 버전 및, 환경 구성을 포함한 Elastic Beanstalk 구성 요소의 논리적인 컬렉션이다. 개념적으로 폴더와 유사하다.

    - 환경: 애플리케ㅣ션 버전을 실행 중인 AWS리소스 모음이다. 각 환경은 한 번에 하나의 애플리케이션 버전만 실행하지만 여러 환경에서 동일한 애플리케이션 버전 또는 서로 다른 애플리케이션 버전을 동시에 실행할 수 있다.

    - 플랫폼: 운영 체제(os), 프로그래밍 언어 런타임, 웹 서버 애플리케이션 서버 및 Elastic beanstalk 구성 요소의 조합이다. 웹 어플리케이션을 설계하고 플랫폼에 맞게 타겟팅 한다. 