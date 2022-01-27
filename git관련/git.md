# git

## git fetch 
```bash
Fetch를 실행하면 원격 저장소의 최신 이력을 확인할 수 있습니다.
이 때 가져온 최신 커밋 이력은 이름 없는 브런치로 로컬에 가져오게 됩니다. 이 브런치는 FETCH_HEAD라는 이름으로 CHECKOUT할 수 있습니다.
>> 최신 내용을 확인하기 위해 (병합하기 전)
```



## git& ec2 연동하기

```
// ec2 
1. cd ~/.ssh
2. ssh-keygen -t rsa -b 4096 -C "kkad45@naver.com"
3. cat ~/.ssh/id_rsa.pub 입력 뒤 내용복사


// 깃페이지
4. git-> setting -> SSH and GPG keys  등록

```

## git cached삭제 

- 사용하는 이유 : ignore 하고싶은 파일이 아직 git에 존재할 때
```
git rm -r -cached .
git add .
git commit -m "cached delete"
```


## git 충돌 해결하기

- Pull Request했을 때 충돌
```
PR창에서 Resolve conflicts클릭
충돌된 부분 수정하여 Commit merge 누르기
```

## git 협업 방법

```
1.협업할 프로젝트 선택
2. 자신의 계정으로 fork
3. git remote add origin 내 깃 포크 주소
4. git remote add upstream 협업한프로젝트 깃 주소
```


## Upstream , rebase
```
Upstream & Downstream은 상대적인 개념  origin과 local을 기준으로 생각하면 origin이 upstream,local이
downstream 

- local에서 origin으로 push한다
- origin에서 local로 pull한다.

git fetch [branch]  : branch 내용을 가져옴 merge 하기전
git pull : git fetch + git merge
 
git에서 브렌치에서 다른 브렌치로 합치는 방법 두가지 merge , rebase

rebase 한 브렌치의 log는 선형이다.
Rebase는 보통 리모트 브랜치에 커밋을 깔끔하게 적용하고 싶을 때 사용한다.

```


## git merge

- 여러 개의 브랜치를 하나로 모을 수 있다.
- Master브런치에서 3개의 브런치(a b c)를 만들었을 경우.
    - a브런치에서 만든 커밋 메세지를 마스터의 브렌치와 합치고 싶을 경우 Merge를 사용하면 병합할 수 있다.
    - a브랜치를 merge한 마스터와 b를 병합할 경우와 a브랜치와 master브랜치와 병합할 때랑 방법이 다르다 
        - 전자는 다른뿌리에 있는 커밋히스토리를 병합하는것이라 병합하는 과정자체를 커밋 메세지에 등록하게 된다
        - 후자는 커밋메세지가 따로 저장되지않는다 (fast-forward)

```
git branch [개발 브런치]
// 개발완료후

git add .
git commit -m "메세지"

git checkout main
git merge 개발 브런치
git push origin main// 포크한 내 깃허브 프로젝트로 올라간다
풀리퀘스트 요청하기

```

## git branch 

- Branch란
    - 개발하던 코드를 통째로 복사하여 원래의 코드와는 독립적으로 개발할 수 있는 환경을 제공해주는게 브랜치이다.

- 브랜치 이동하기
```
git checkout 브랜치명
```

- 브랜치 만들기
```
git branch 브랜치명 
// 또는
git checkout -b 브랜치명
```

```
git branch -v // 각 브런치마다 마지막 커밋메세지를 확인할 수 있다. 
```

