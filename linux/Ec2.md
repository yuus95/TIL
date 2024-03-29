## IAM
```bash
AWS Identity and Access Management(IAM)는 AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스입니다. IAM을 사용하여 리소스를 사용하도록 인증(로그인) 및 권한 부여(권한 있음)된 대상을 제어합니다.

```
- IAM 기능
    - AWS 계정에 대한 공유 엑세스
    - 세분화된 권한
    - Amazone EC2에서 실행되는 애플리케이션을 위한 보안 AWS리소스 액세스
    - 멀티팩터 인증
    - 자격 증명 연동
    - 보장을 위한 자격 증명 정보
    - PCI DSS 준수 



- IAM의 사용자와 역할의 차이
```bash
#역할
>> "AWS 서비스에만 할당할 수 있는 권한: EC2,CodeDeploy등이 이에 해당한다"

#사용자
>> "AWS 사용자 외에 사용할 수 있는 권한, 로컬 PC나 IDC서버 등이 이에 해당한다:
```

## 자바 11설치

```bash
sudo curl -L https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.rpm -o java11.rpm

sudo yum localinstall java11.rpm

java --version
```


## 타임존 변경
```bash
sudo rm /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```


## EC2 Git 설치
```bash
sudo yum install git

git --version

```


## EC2 배포

```java
mkdir git

git clone [프로젝트주소]

chmod [권한] gradlew

./gradlew build

cd build/libs

java -jar jar파일명
```

## Winscp FTP서버 오류 났을 경우

- Linux일 경우
```java
// -R은 하위 디렉토리까지 소유권 변경
chown -R ec2-user:ec2-user [폴더명]
```


## Mac에서 Ec2들어가기
- 1회성

```bash
ssh -i {pem키 위치 /Users/Yushin/~.pem} {ec2-user@ec2아이피 ec2-user@192.168.0.1}
```

- 편리하게 config파일에 설정하여 쉽게 접속하기
```bash
cp /Users/Yushin/~.pem ~/.ssh/
vim ~/.ssh/config

Host {호스트네임 ec2-yushin}
    HostName {ec2ip 192.168.0.1}}
    User ec2-user
    IdentityFile: ~/.ssh/{pem키이름}
```