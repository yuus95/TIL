# CICD

## CICD란
- CI(Continuous Integration - 지속적인 통합) : VCS 시스템에 PUSH가 되면 자동으로 테스트와 빌드가 수행되어 안정적인 배포 파일을 만드는 과정을 CI라고 한다.
- CD(Continuous Deployment - 지속적인 배포): 빌드 결과를 자동으로 운영 서버에 무중단 배포까지 진행되는 과정을 CD라고 한다.


## CICD가 나온 이유

```bash
 현대의 웹 서비스 개발에서는 하나의 프로젝트를 여러 개발자가 함께 개발을 진행합니다. 그러다 보니 각자가 개발한 코드가 합쳐야 할 때마다 큰일이였습니다. 그래서 매주 병합일을 정하여 이날은 각자가 개발한 코드를 합치는 일만 진행했습니다. 

이런 작업을 수작업으로 진행하여 생산성이 좋지 않기에 CICD가 탄생되었습니다.

한두 대의 서버에 개발자가 수동으로 배포할 수 있지만, 수십 대 수백 대의 서버에 배포를 해야 하거나 긴박하게 당장 배포를 해야 하는 상황이 오면 더는 수동으로 배포할 수가 없다.
```

- 마틴 파울러의 4가지 규칙
``` bash
1. 모든 솟 코드가 살아 있고(현재 실행되고) 누구든 현재의 소스에 접근할 수 있는 단일 지점을 유지할 것.

2. 빌드 프로세스를 자동화해서 누구든 소스로부터 시스템을 빌드하는 단일 명령어를 사용할 수 있게 할 것.

3. 테스팅을 자동화해서 단일 명령어로 언제든지 시스템에 대한 건전한 테스트 수트를 실행할 수 있게 할 것

4. 누구나 현재 실행 파일을 얻으면 지금까지 가장 완전한 실행 파일을 얻었다는 확신을 하게 할 것.
```

- 제일 중요한 것은 테스팅 자동화 이다. 지속적으로 통합하기 위해서는 무엇보다 이 프로젝트가 완전한 상태임을 보장하기 위해 테스트 코드가 구현되어 있어야만 합니다.


## 구축방법

- 1.travis 홈페이지 가입
```bash
1. https://travis-ci.com //깃과 연동
2. 프로젝트 연동
3 . CI하고싶은 프로젝트 선택
```

2. 프로젝트 내 설정

```java
language: java
jdk: 
  - openjdk11
  
branches:
  only:
    - main
#Travis CI 서버의 Home
cache:
  directories:  
    - '$HOME/.m2/repository'
    - '$HOME/.gradle'
script: "./gradlew clean build"

# CI 실행 완료시 메일로 알람
notifications: 
  email:
    recipients: 
      - "kkad45@naver.com"


```

## 터미널 Travis 로그인

```bash
# 1.Ruby설치
# 2. travis 설치
gem install travis
# 3. travis login
travis login --com --github-token [토큰]


# 관련된 오류
Please try re-syncing your user data at https://travis-ci.org/account/preferences and try logging in via travis-ci.org
```


##  Travis 파일 암호화 오류

- Window에서 파일을 암호화 할 경우 오류가 생긴다 Mac이나 linux에서 암호화를 해야 한다


### 1. linux ruby설치

```bash
# 1. 기본버전 2.0 루비 설치하기
sudo yum install ruby

# 2. 루비 빌드 도구에 필요한 종속성 설치하기
sudo yum install git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel

# 3. rbenv 및 ruby-build설치하기
# with curl
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash


# ruby 버전 설치
rbenv install 2.7.4
rbenv global 2.7.4

# ruby 버전 확인하기
ruby -v
```


## 오류

```bash
application-dev.yml.enc: No such file or directory

>> enc 파일을 Resource폴더에 넣지말고
bulild.gradle과 같은 프로젝트 맨처음 디렉토리에 넣는다.

```