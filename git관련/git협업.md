```
[Github]
1. A 깃허브 Repository 생성
2. B(본인) 깃허브에서 A 깃허브 Repository를 Fork하기




[Local]
git remote add origin "본인깃허브에 포크한 주소"

git remote add upstream "A깃허브에잇는 진짜 레포지터리주소"

git checkout -b kys-dev // 신규 개발 브랜치 생성

//기능, 도메인 단위로 add , commit 하기!

// ex) 멤버도메인를 다 만들었을 경우
git add .
git commimt - m  " 멤버도메인 구현"



[Merge]
git checkout main
git merge kys-dev --no-ff // no fastforward

[Push]
git push origin master

[PR]
// 적절한 메시지를 작성하여 풀리퀘스트 수행 및 upstream repo에서 코드 병합

[동기화]
git fetch upstream
git merge upstream/master

[개발 브랜치 재구성]
git branch -D kys-dev
git checkout -b kys-dev

```