# 도커 
```
회사에서 도커를 요즘 많이 사용하기에,
개념을 정리하기위해 작성한다.
```

## 도커란?
- 리눅스 컨테이너를 기반으로 하여 특정한 서비스를 패키징하고 배포하는데 유용한 오픈소스 프로그램이다. 


## 가상 머신과 도커 컨테이너 
- 가상머신은 하이퍼바이저를 이용해 Guest OS를 만들어낸다. 예를 들어 운영체제를 메인으로 쓰고 싶다면 이는 Host OS가 되는 것이고 이 위에 Ubuntu를 가상 머신위에 구동시키는것이다. Guest OS를 구동시키려면  Host OS에서 자원을 일부 사용해야 한다. 
따라서 Host OS가 느려진다.

- 가상머신에 비해 꼭 필요한 것만 담겨서 구동된다는 차이가 있다. 즉 컨테이너에 필요한 커널은 호스트의 커널과 공유해서 사용하고, 컨테이너안에는 애플리케이션을 구동하는데 필요한 라이브러리 및 실행파일만 존재하기 때문에 컨테이너를 이미지로 만들경우 용량이 대폭 줄어든다.

## 도커와 관련된 개념

- 커널:  컴퓨터 과학에서 커널(kernel)은 컴퓨터 운영 체제의 핵심이 되는 컴퓨터 프로그램 

    - 역할
        - 커널은 컴퓨터 하드웨어와 프로세스의 보안을 책임진다.
        
        <br> 

        - 자원관리 : 한정된 시스템 자원을 효율적으로 관리하여 프로그램의 실행을 원활하게 한다. 특히 프로세스에 처리기를 할당하는 것을 스케쥴링이라고 한다.
       
         <br>
       
        - 추상화 : 같은 종류의 부품에 대해 다양한 하드웨어를 설계할 수 있기 때문에 하드웨어에 직접 접근하는 것은 문제를 매우 복잡하게 만들 수 있다. **일반적으로 커널은 운영 체제의 복잡한 내부를 감추고 깔끔하고 일관성 있는 인터페이스를 하드웨어에 제공하기 위해 몇 가지 하드웨어 추상화(같은 종류의 장비에 대한 공통 명령어의 집합)들로 구현된다.** 이 하드웨어 추상화는 프로그래머가 여러 장비에서 작동하는 프로그램을 개발하는 것을 돕는다. 하드웨어 추상화 계층(HAL)은 제조사의 장비 규격에 대한 특정한 명령어를 제공하는 소프트웨어 드라이버에 의지한다.

    ```
    메모리 관리: 메모리가 어디에서 무엇을 저장하는 데 얼마나 사용되는지를 추적합니다.
    프로세스 관리: 어느 프로세스가 중앙 처리 장치(CPU)를 언제 얼마나 오랫동안 사용할지를 결정합니다.
    장치 드라이버: 하드웨어와 프로세스 사이에서 중재자/인터프리터의 역할을 수행합니다.
    시스템 호출 및 보안: 프로세스의 서비스 요청을 수신합니다.
    ```
![img](/이미지/kernel.png)

[출처](https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%84%90_(%EC%BB%B4%ED%93%A8%ED%8C%85))


## 도커 

- 도커는 응용 프로그램과 그 의존성을 가상 컨테이너에 담을 수 있는 도구

- 도커는 리눅스의 응용 프로그램들을 프로세스 격리 기술들을 사용해 컨테이너로 실행하고 관리하는 오픈 소스 프로젝트이다

- 도커는 리눅스에서 운영 체제 수준 가상화의 추상화 및 자동화 계층을 추가적으로 제공한다

- docker image : 이미지는 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것으로 상태값을 가지지 않고 변하지 않습니다

- docker 컨테이너 : 컨테이너(Container)는 개별 Software의 실행에 필요한 실행환경을 독립적으로 운용할 수 있도록 기반환경 또는 다른 실행환경과의 간섭을 막고 실행의 독립성을 확보해주는 운영체계 수준의 격리 기술을 말합니다.
 
## 도커이미지
 - 도커 이미지는 컨테이너를 만들기 위한 지침서가 포함되어있는 읽기전용 템플릿이다.
 - 하나의 이미지는 다른 이미지를 기반으로 만들 수 있다. (커스텀)
 - 하나의 우분투 이미지에 다운받아 아파치 서버와 어플리케이션을 동봉한 이미지로 만들 수 있다. 이 때 어플리케이션과 서버를 실행시키기위한 필요한 환경구성들을 포함시켜서 만들 수 있다.

## 도커 컨테이너
- 이미지를 실행하는 하나의 인스턴스이다.
- 컨테이너는 호스트와 다른 컨테이너와 비교적 잘 격리되어있다.


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

# 도커 종료된 컨테이너까지 보기
docker ps -a

# 컨테이너 bash실행하기 
docker exec -it mysql-basic bash
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
# CMD 명령은 도커 이미지를 run 시킬 때 인자값을 주게 될 경우 입력된 입자값이 입력되고 실행이 안된다.
CMD ["npm","start"]

```
- docker명령어 CMD와 ENTRYPOINT
    - 차이점 : 컨테이너 시작시 실행 명령에 대한 Default 지정 여부
    - ENTRYPONT: 해당 컨테이너가 수행될 때 반드시 지정한 명령을 수행하도록 지정
    - CMD: 컨테이너 실행할 때 인자값을 주게 되면 Dokckerfile에 지정된 CMD값을 대신 하여 지정한 인자값으로 변경하여 실행하게 된다.
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
-->DockerFil로 정의된 환경은 어떤 곳이든

Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment.
->  서비스들을 docker-compose.yml에 구성해라 그들을 함께 하나의 격리된환경에서 작동하기 위해서

Run docker compose up and the Docker compose command starts and runs your entire app. You can alternatively run docker-compose up using the docker-compose binary.
-> 도커 컴포즈를 실행하면  도커 컴포즈 명령어가 실행되면서 우리의 모든앱이 시작된다.
```
- 도커 컴포즈란: 여러 개의 컨테이너로부터 이루어진 서비스를 구축, 실행하는 순서를 자도응로 하여, 관리를 간단히 하는 기능이다.

- Docker compose에서는 compose의 파일을 준비하여 커맨드를 1회 실행하는 것으로, 그파일로 부터 설정을 읽어들여 모든 컨테이너 서비스를 실행시키는 것이 가능하다.


- ### 도커 컴포즈 설치

    - (mac)docker설치시 자동으로 설치 된다.
    -  linux 
    ```
    sudo curl -L <https://github.com/docker/compose/releases/download/{설치버전}/docker-compose-`uname>

    sudo chmod +x /usr/local/bin/docker-compose
    ```

- ### 도커 컴포즈 명령어

```yml
# 서비스명
node-js:
#  도커 허브에서 이미지에서 가져올 수도있고 직접 빌드할 수 있따.
# 빌드할 때는 Dockerfile 위치를 표시해주면된다.
 build: "."
 # 컨테이너네임
 container_name: "node_test"
 # 작업 장소 -> Dokcerfile에 설정했으면 설정안해도된다.
 working_dir: "/app"
 # 호스트 포트와 맵핑
 ports:
  - "3000:3000"
```