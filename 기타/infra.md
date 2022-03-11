# 인프라 용어 정리


- provisioning:
    - 제공하는 것
    - 어떤 종류의 서비스든 사용자의 요구에 맞게 시스템 자체를 제공 하는 것을 프로비저닝이라고 하며, 제공해줄 수 있는 것은 인프라 자원이나 서비스, 또는 장비가 될 수 있다.

    - 종류 :([출처](https://jins-dev.tistory.com/entry/%ED%94%84%EB%A1%9C%EB%B9%84%EC%A0%80%EB%8B%9DProvisioning-%EC%9D%B4%EB%9E%80))
    ```bash
    # 종류
    Server Resource Provisioning : CPU, Memory, IO 등과 같은 실제 서버의 자원을 할당해주고 운영할 수 있게 제공해주는 것을 말한다.

    OS Provisioning : OS를 서버에 설치하고 구성작업을 해서 사용할 수 있도록 제공하는 것을 말한다.

    Software Provisioning : WAS, DBMS 등의 소프트웨어를 설치하고 세팅하여 실행할 수 있도록 제공하는 것을 말한다.

    Account Provisioning : 접근 권한을 가진 계정을 제공해주는 것을 말한다. 클라우드 인프라 쪽에서는 해당 업무를 담당하던 관리자가 변경된 경우 권한의 인계를 Account Provisioning 을 통해 하는 경우가 많다.

    Storage Provisioning : 데이터를 저장하고 관리할 수 있는 Storage 를 제공할 수 있다. 특히 클라우드에서는 제공하는 Storage 의 종류와 용도에 따라 다양한 방식의 제공이 이루어진다.
    ```