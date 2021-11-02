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

## 과정
```bash
# 1
Travis CI - GitHub 연동

# 2 
Travis CI와 AWS S3 연동하기

# 3 
codeDeploy 를 활용하여 ec2에 배포하기 위해 ec2와 codeDeploy를 연동 받을 수 있도록 IAM의 역할을 추가하기(Ec2에 역할 추가하기)

#모든 과정 
로컬푸시 -> Travis CI(테스트 및 빌드 한 뒤, S3에 전달)  -> S3(Jar파일 & 스크립트 파일 압축)  -> Code Deploy(S3 파일을 ec2에 전달) -> ec2 
```



# 세부과정 

## 1. Travis - Github
- 1.travis 홈페이지 가입
```bash
1. https://travis-ci.com //깃과 연동
2. 프로젝트 연동
3 . CI하고싶은 프로젝트 선택
```

2. 프로젝트 내 설정
- .travis.yml 파일  루트파일에 생성
```yml
language: java
jdk:
- openjdk11
branches:
  only:
  - main
cache:
  directories:
  - "$HOME/.m2/repository"
  - "$HOME/.gradle"
script: "./gradlew clean build"

before_install:

 # yml 암호화파일 복호화
  - openssl aes-256-cbc -K $encrypted_dd868b5a6e58_key -iv $encrypted_dd868b5a6e58_iv -in application-dev.yml.enc -out application-dev.yml -d
  - chmod +x gradlew

# Travis Ci는 S3로 특정 파일만 업로드가 안된다. 디렉토리 단위로 업로드 가능
# deploy 이전에 설정 
before_deploy:

  - mkdir -p before-deploy # zip에 포함시킬 파일들을 담을 디렉토리 생성
  - cp script/*.sh before-deploy # 배포 스크립트 이동 
  - cp appspec.yml before-deploy # codedeploy파일 이동
  - cd before-deploy && zip -r before-deploy.zip * # zip 으로 압축
  - cd ../ && mkdir -p deploy
  - mv before-deploy/before-deploy.zip deploy/springboot-test.zip # deploy 폴더로 이동


deploy:
  #s3설정
  - provider: s3
    access_key_id: $AWS_ACCESS_KEY
    secret_access_key: $AWS_SECRET_KEY
    #s3 버킷 이름
    bucket: yushin09
    region: ap-northeast-2
    skip_cleanup: true
    acl: private # zip 파일 접근을 private으로
    local_dir: deploy # before_deploy에서 생성한 디렉토리로 이동한다. 해당 위치의 파일만 이동시킨다.
    wait-unil-deployed: true
    on:
      branch: main

#  codedeploy설정
#  ec2와 codedeploy가 연동되있을 경우  S3에있는 파일을 압축해제해서 Ec2에 넣어준다.
  - provider: codedeploy
    access_key_id: $AWS_ACCESS_KEY
    secret_access_key: $AWS_SECRET_KEY
    bucket: yushin09
    key: springboot-test.zip
    bundle_type: zip
    application: ec2-codedeploy # aws code_deploy 애플리케이션 이름
    deployment_group: code-deploy # code_deploy 의 배포그룹
    region: ap-northeast-2
    wait-until-deployed: true
    on:
      branch: main



notifications:
  email:
    recipients:
    - kkad45@naver.com

```

###  Travis 파일 암호화 오류

- Window에서 파일을 암호화 할 경우 오류가 생긴다 Mac이나 linux에서 암호화를 해야 한다


###  linux ruby설치

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

### 터미널 Travis 로그인

```bash
# 1.Ruby설치
위에 내용 참고
# 2. travis 설치
gem install travis
# 3. travis login
travis login --com --github-token [토큰]


# 관련된 오류
Please try re-syncing your user data at https://travis-ci.org/account/preferences and try logging in via travis-ci.org
>> " org로 연동되있기 때문이다. travis login --com --github-token을 활용하면
Travis.org가 아닌 Travis.com 으로 되기 때문에 오류해결가능"
```


### Travis  오류

```bash
#오류 1
application-dev.yml.enc: No such file or directory

>> enc 파일을 Resource폴더에 넣지말고
bulild.gradle과 같은 프로젝트 맨처음 디렉토리에 넣는다.


# 오류2
The overall deployment failed because too many individual instances failed deployment, too few healthy instances are available for deployment, or some instances in your deployment group are experiencing problems.
>> 오타 오류

>> 해결방법
/var/log/aws/codedeploy-agent # 이동
vi codedeploy-agent.log #입력
vim 창에서 G 입력 # 맨끝에 오류 확인

#오류 3
2021-10-28 14:54:15 ERROR [codedeploy-agent(3399)]: InstanceAgent::Plugins::CodeDeployPlugin::CommandPoller: Error during perform: InstanceAgent::Plugins::CodeDeployPlugin::ApplicationSpecification::AppSpecValidationException - The deployment failed because the application specification file specifies only a source file ({"destination"=>"/home/ec2-user/app/zip/", "overwrite"=>true}). Add the name of the destination file to the files section of the AppSpec file, and then try again. - /opt/codedeploy-agent/lib/instance_agent/plugins/codedeploy/application_specification/file_info.rb:13:in `initialize'
x
>> "appspec 오타로 인한 오류 "


# 오류 4 
Travis 암호화파일은 root 폴더 그대로 복호화 시킨다.
배포스크립트를 활용하여 복호화한 yml파일을  resources로 옮겨줘야한다.
```


## 2. Amazon S3 생성 및 연동 
```bash
"아마존 웹 서비스에서 제공하는 온라인 스토리지 웹 서비스이다. s3가 의미하는 것은 Simple Storage Service의 각 단어의 맨 앞 글자 s3개를 의미"
```

- 사용하는 이유
```bash
Travis 서버에서 생성된 jar파일을 S3에 전달해야 한다. 이유는 실제 배포는 AWS의 CodeDeploy를 통해 이루어진다. 하지만 이런 CodeDeploy는 파일을 저장할 수 있는 기능을 가지고 있지 않다. 그렇기 때문에 배포에 필요한 파일을 별도 보관할 수 있는 공간이 필요한데 그것이 바로 S3이다.
```

- AWS Key 발급
```bash
"일반적으로 AWS 서비스에 외부 서비스가 접근할 수 없습니다."
그러므로 "접근 가능한 권한을 가진 Key를 생성해서 사용해야 합니다. " AWS에서는 이러한 인증과 관련된 기능을 제공하는 서비스로 IAM이 있습니다.

IAM
>> AWS에서 제공하는 서비스의 접근 방식과 권한을 관리합니다.
IAM을 통해 Travis CI가 AWS S3와 CodeDeploy에 접근할 수 있도록 해야한다.

IAM설정 방법 
>> https://yuus95.tistory.com/8 


S3 생성 방법
>>  https://yuus95.tistory.com/9
```


## 2 Code Deploy 설정

### Code Deploy란?
```bash
CodeDeploy는 Amazon EC2 인스턴스, 온프레미스 인스턴스, 서버리스 Lambda 함수 또는 Amazon ECS 서비스로 애플리케이션 배포를 자동화하는 배포 서비스입니다.

"자세한내용"
https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/welcome.html 
```
### 초기 설정
```bash
#1 ec2 - IAM역할 추가하기(ec2 가 CodeDeploy을 연동받을 수 있게 하기 위해 역할 추가를 해줘야 한다)
https://yuus95.tistory.com/10

#2 CodeDeploy IAM역할 추가하기(ec2에 접근하기 위해서 추가해줘야한다)

```


### 1. Ec2 -> CodeAgent 설치
```bah  
aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2

chmod +x ./install

sudo ./install auto

sudo service codedeploy-agent status
```

### 2. 프로젝트 루트 폴더에 appspec.yml  작성
-  CodeDeploy할 때 필요한 설정파일 등록

```yml
version: 0.0
os: linux
files: # 파일을 받을 때 무엇을 받을 지, 어디다 저장할 지 저장
  - source: /
    destination: /home/git/simpleMarket
    overwrite: yes

# deploy 이후 권한설정을 할 수 있다.
permissions:
  - object: /
    pattern: "**"
    owner: ec2-user
    group: ec2-user
    mode: 777

hooks:
  AfterInstall:
    # EC2경로가 아니라 배포할 소스 코드에 shell script가 있어야 함에 유의!!
    - location: deploy.sh
      runas: ec2-user
      timeout: 60


```

## 다중 스크립트

```bash
stop.sh: 
>> "기존 엔진엑스에 연결되어 있지 않지만, 실행중이던 스프링 부트 종료"

start.sh:
>> "배포할 신규 버전 스프링 부트 프로젝트를 stop.sh로 종료한 'profile로 실행'"

health.sh: 
>> "start.sh: 엔진엑스가 바라보는 스프링 부트를 최신 버전으로 변경"

switch.sh: 
>> "엔진엑스가 바라보는 스프링 부트를 최신 버전으로 변경"

profile.sh:
>> "앞선 4개 스크립트 파일에서 공용으로 사용할 profile 과 포트 체크 로직"

```