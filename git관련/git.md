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