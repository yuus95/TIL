# git


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