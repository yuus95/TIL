# vim

- 모드
    - Normal 모드일 때에는 아직 마음껏 타이핑을 하지 않도록 합니다. 커서가 여기저기 점프하고 글자가 삭제될 수 있다.
    -   Insert모드가 되면 평범한 워드 프로세서처럼 사용할 수 있다.

-  전환
    - NORMAL모드일 때 i를 누르면 INSERT모드로 바뀝니다.
    - INSERT모드일 때 ESC를 누르면 NORMAL모드로 돌아갑니다.

- vim을 종료하는 방법

    ```
    :q -> VIM을 종료합니다. 편집중이던 파일이 있으면 저장하지 않았음을 알려주며 종료되지 않습니다.
    :q! -> VIM을 강제로 종료합니다.
    :wq -> 편집중인 파일을 저장하고 VIM을 종료합니다.
    :x -> 편집중인 파일을 저장하고 VIM을 종료합니다. 단 파일 내용이 변경되지 않았을 경우 저장없이 종료합니다.
    :ZZ -> 편집중인 파일을 저장하고 VIM을 종료합니다.
    ```

- 저장하는방법

    - ## VIM은 NORMAL모드에서만 저장할 수 있습니다.

    - ESC키를 입력하여 INSERT모드에서 NORMAL모드로 전환합니다.
    - :w를 입력하면 편집중이던 파일이 저장됩니다.
    - :w filename 을 입력하면 filename으로 saveas할 수 있습니다.(다른이름으로 저장)




## 패키지 관리

- 패키지관리란 (리눅스환경에서 소프트웨어를 설치 삭제 업데이트하는 작업을 의미)

- sudo : 관리자 모드를 의미

- apt-get /etc/apt/source.list 인덱스 위치 패키지의 정보를 얻어옴

- sudo apt-get update 인덱스 정보 업데이트
- sudo apt-get upgrade 모두 새버전으로 업그레이드 

- sudo apt-get install 패키지 이름

- sudo apt-get remove 패키지 이름


## 디렉터리 이동

- pwd : 현재 위치확인

- ~ : 리눅스에서 홈 디렉토리로 이동하는 명령어


## 편집기 삭제 기능
```
x: 커서가 있는 문자 삭제
X: 커서가 있는 문자 앞의 문자 삭제
dw: 커서가 있는 단어 삭제
db : 커서 앞에 있는 단어 삭제
dW: 공백으로 구본된 뒷 단어 삭제
dd: 커서가 있는 라인 삭제
D: 커서가 있는 라인 나머지 삭제
d):문장의 나머지 삭제
d}: 문단의 나머지 삭제

```


## CP 파일 복사
-  폴더로 파일 복사
```bash
cp 파일1 디렉토리 //1 
cp 파일1 , 파일2 ,... 디렉토리 //2

>> 디렉토리로 파일들을 복사한다.
```

- 파일을 복사

```bash
cp 파일1 파일2
>> 파일2는 파일1을 복사한파일이다.
```

## mkdir 
```bash
-p 옵션을 사용할 경우에는 존재하지 않는 중간의 디렉토리를 자동을 생성해준다.
```

## 포트확인

```bash
netstat -tnlp
```