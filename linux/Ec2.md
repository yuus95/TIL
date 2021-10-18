
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