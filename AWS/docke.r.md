# 도커 
```
회사에서 도커를 요즘 많이 사용하기에,
개념을 정리하기위해 작성한다.
```

## 도커와 관련된 개념

- 커널:  컴퓨터 과학에서 커널(kernel)은 컴퓨터 운영 체제의 핵심이 되는 컴퓨터 프로그램 

    - 역할
        - 커널은 컴퓨터 하드웨어와 프로세스의 보안을 책임진다.
       
        <br> 

        - 자원관리 : 한정된 시스템 자원을 효율적으로 관리하여 프로그램의 실행을 원활하게 한다. 특히 프로세스에 처리기를 할당하는 것을 스케쥴링이라고 한다.
       
         <br>
       
        - 추상화 : 같은 종류의 부품에 대해 다양한 하드웨어를 설계할 수 있기 때문에 하드웨어에 직접 접근하는 것은 문제를 매우 복잡하게 만들 수 있다. **일반적으로 커널은 운영 체제의 복잡한 내부를 감추고 깔끔하고 일관성 있는 인터페이스를 하드웨어에 제공하기 위해 몇 가지 하드웨어 추상화(같은 종류의 장비에 대한 공통 명령어의 집합)들로 구현된다.** 이 하드웨어 추상화는 프로그래머가 여러 장비에서 작동하는 프로그램을 개발하는 것을 돕는다. 하드웨어 추상화 계층(HAL)은 제조사의 장비 규격에 대한 특정한 명령어를 제공하는 소프트웨어 드라이버에 의지한다.
![img](/이미지/kernel.png)

[출처](https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%84%90_(%EC%BB%B4%ED%93%A8%ED%8C%85))


## 도커 

- 도커는 응용 프로그램과 그 의존성을 가상 컨테이너에 담을 수 있는 도구

- 도커는 리눅스의 응용 프로그램들을 프로세스 격리 기술들을 사용해 컨테이너로 실행하고 관리하는 오픈 소스 프로젝트이다

- 도커는 리눅스에서 운영 체제 수준 가상화의 추상화 및 자동화 계층을 추가적으로 제공한다


- docker image : 이미지는 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것으로 상태값을 가지지 않고 변하지 않습니다

- docker 컨테이너 : 컨테이너(Container)는 개별 Software의 실행에 필요한 실행환경을 독립적으로 운용할 수 있도록 기반환경 또는 다른 실행환경과의 간섭을 막고 실행의 독립성을 확보해주는 운영체계 수준의 격리 기술을 말합니다.
 ## Docker 명령어

 
 ```bash

 #  도커 이미지 받기
docker pull {옵션} {imageName}:{tag- 버전을뜻함}
 -> ex) docekr pull mysql:5.7

# 로컬에 있는 도커 이미지확인
docker images

# docekr  Image부터 컨테이너 생성 및 실행 
docker run {옵션} {이미지네임}:{태그 - optinal}
-> ex) docekr run -d(데몬으로실행) -p (호스트포트와 컨테이너 포트를 매칭) mysql:5.7

# docker image 삭제 
docker rmi {imagename}

# 컨테이너 삭제
docker rm {containerName}

# docker 시작,재시작, 멈춤
docekr start imageName
docekr restart imageName
docker stop imageName

# 도커 실행중인 프로세스 보기
docker ps

 ```


## Dockerfile

```
docekrImage를 도커파일을 통해 직접 생성할 수 있다.
기존의 이미지를 다운받아서 커스텀 하기위해 사용된다.
```

- ### 생성방법

``` bash
# 베이스이미지
FROM node:6.2.2
# 명령 실행 - 이미지를 작성하기 위해 실행하는 명령 
RUN mkdir -p /app

# 작업 디렉토리 설정 
WORKDIR /app

# 호스트 OS 의 파일들을 컨테이너 안으로 이동
# 현재작업 디렉토리구성 Dockerfile express-test{node앱}
# add 와 copy가 있따. add가 더 넓은 활용도를 갖는다.
ADD express-test

# 환경변수추가
ENV NODE_ENV developer

# 포트 노출 (3000,80 노출)
EXPOSEE 3000 80
# 컨테이너 생성 후 cmd 실행
CMD ["npm","start"]
```

- 이미지 빌드
```
docker build {option} name
ex) docker build -t{태그추가} nodejs:1.0
```

- 이미지 실행
```
docker run {option} {name}:{tag}
ex) docker run -d{백그라운드실행} -p{포트연결} 3000:3000 nodejs:1.0
```

## Docker compose
```
하나의 격리된 환경에서 같이 실행할수 있는 도커환경?
조금 더 공부하고 정리하기

Define your app’s environment with a Dockerfile so it can be reproduced anywhere.

Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment.

Run docker compose up and the Docker compose command starts and runs your entire app. You can alternatively run docker-compose up using the docker-compose binary.
```


