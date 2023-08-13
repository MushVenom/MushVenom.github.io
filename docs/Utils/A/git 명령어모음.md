---
title: git 명령어 모음
layout: default
parent: github
grand_parent: Utils
---


# git 기본🎯💡🔥📌✅

<br />

## git 버전 관리 하기

{: .highlight } 
> - branch 변경

```bash
$ git switch master
```

<br />
<br />

{: .highlight } 
> - 병합하기
>   - 현재 branch에 master branch병합

```bash
$ git merge master
```

<br />
<br />

{: .highlight } 
> - 충돌 메세지 이해하기

![Alt text](image-1.png)

```bash
# 이후에 commit 까지 해야 충돌이 해결된 것
# 커밋까지 해야 해결된 것
$ git add .
$ git commit -m "merge conflict resolve"

## 여기서 해결된 것은 다른 branch들에서 pull받으면 그냥 승복함
```


<br />
<br />

{: .highlight } 
> - 로컬 저장소에서 branch를 만들고 원격 저장소에 올리는 방법

```bash
# 로컬 저장소에 브랜치 생성
$ git checkout -b jjehyun
# 원격 저장소에 브랜치 푸쉬
$ git push --set-upstream origin jjehyun
```


<br />
<br />

{: .highlight } 
> - github > Projects > Board
> - 프로젝트 상황을 보여줌


![Alt text](image.png)


<br />
<br />

{: .highlight } 
> - github > issues 
> - 이슈를 생성 후 생성한 이슈에서 branch를 생성할 수 있다.
> - 오른쪽 Development > `Create a branch`를 클릭한다.

- ![Alt text](image-2.png)
- ![Alt text](image-3.png)

```bash
$ git fetch origin
$ git checkout feature-A
```

<br />
<br />

{: .highlight } 
> - pull-request 생성하기
> - 코드 리뷰 반영하기
> - github > Pull request > `Create pull request`

- ![Alt text](image-4.png)
- ![Alt text](image-5.png)

```bash
# 코드 리뷰 반영 하는 방법
# pr 보낸 상태에서 코드 리뷰받고 다시 pr를 수정하는 방법
# 그냥 pr 보낸 branch의 소스 코드를 원격 저장소에 다시 올리면 됨

$ git add .
$ git commit -m "코드 리뷰 반영"
$ git push origin jjehyun
```

<br />
<br />

{: .highlight } 
> - pr 충돌 해결하기
>   - 일단 로컬 저장소에서 merge작업을 진행해야함

- ![Alt text](image-6.png)
- ![Alt text](image-7.png)
- ![Alt text](image-8.png)

<br />

```bash
$ git checkout develop
# develop브랜치 내용을 가져옴
$ git pull origin develop
# 이전 branch로 돌아감
$ git checkout -
## 문제가 생기지 않게 로컬에서 merge작업을 진행
$ git merge develop 

## 어떤 코드가 맞는 지 팀원 상의 후에 선택하기!
# 충돌을 해결하고 원격 저장소에 올리기
$ git add .
$ git commit -m "resolve conflict"
$ git push
```

<br />
<br />

{: .highlight } 
> - 과거로 돌아가기 reset vs revert
> - `reset` : 과거로 돌아갈 해쉬 입력
> - `revert` : 취소할 커밋을 선택! 커밋을 반대로 실행하는 명령어
>   - revert 커밋메세지1 : `커밋메세지1의 변동 사항을 모두 취소하고 커밋을 하나 추가한다.`

<br />

```bash
# 과거로 돌아가기 원격저장소에서 작업 중일때 원격저장소에 올린거는 reset하면 안됨
git reset --hard 0bf487

# revert 커밋 중 취소할 하나의 커밋을 선택
git revert 0fe425

# revert 후에 자동 커밋 안되게 하기
git revert --no-commit 354ef2
```

<br />

{: .note } 
> - `revert` : 이해하기

- ![Alt text](image-9.png)
- ![Alt text](image-10.png)


<br />
<br />

{: .highlight } 
> - 브랜치 이동하기
> - 브랜치 관련 명령어

```bash
# branch 이동
git switch production

# branch 생성과 함께 이동하기
git switch -c production
```

<br />
<br />

{: .highlight } 
> - branch 병합하기 
> - `rebase` : main branch에 변경 사항을 하나하나 이어 붙인 것 (`rebase후 merge를 해야함`)
>   - `merge할때는 사용XXXX, pull 받을 때 주로 사용함`
>   - 히스토리를 깜끔하게 만드는 것이 중요하다면 `Rebase`
>   - `이미 팀원들간 공유된 커밋들에 대해서는 사용XX`
> - `merge` : 두개의 브랜치를 합치는 것
>   - 브랜치의 사용 내역을 남겨둘 필요가 있다면 `merge`

<br />


{: .important-title }
> - `merge` : 브랜치 두개를 합친 것

![Alt text](image-11.png)


{: .important-title }
> - `rebase` : main에 병합할걸 이어 붙인것

![Alt text](image-12.png)

```bash
# merge 사용법
git merge main

# rebase 사용
git rebase main

##병합된 branch는 삭제 한다.
git branch -d add-coach
```

<br />
<br />
<br />

{: .highlight } 
> - `merge 충돌 해결` (모든 파일충돌을 해결 하고 커밋하는 방식이기 때문에 하나의 커밋으로 충돌 해결이 가능하다.)

```bash
# merge 시 너무 많은 충돌이 발생했을때 머지 취소하고 머지 실행 바로 직전으로 돌아가기
git merge --abort

# 충돌 해결 후 실행할 명령어
git add .
git commit -m "resolve conflict"
```

<br />
<br />
<br />

{: .highlight } 
> - `rebase 충돌 해결` (merge할 커밋들을 main branch에 하나씩 이어 붙이는 방식이기 때문에 충돌 나는 모든 커밋을 해결해야한다.)


```bash
# rebase 명령어 실행
git rebase main

# rebase 취소
git rebase --abort

## 충돌 해결 후  (다 rebase 될 때까지 반복 **)
git add .
git rebase --continue

# 마지막 merge
git merge main
```

<br />
<br />
<br />

{: .highlight } 
> - `pull`할 것이 있을 때 push를 할 때 사용할 수 있는 명령어 
>   - `git pull --no-rebase` : merge 방식
>   - `git pull --rebase` : rebase 방식 
>   - `pull받아올 때는 주로 rebase를 사용함`
<br />

> `git pull --no-rebase` (로컬과 원격의 분기를 하나로 merge하는 것)

![Alt text](image-13.png)

> `git pull --rebase` (내 커밋을 원격의 main branch뒤에 이어서 붙여줌)

![Alt text](image-14.png)


<br />
<br />
<br />

{: .highlight } 
> - 원격 branch 다루기

```bash
# 원격 저장소에 jjehyun branch 생성
git swich jjehyun
git push -u origin jjehyun


# 원격 저장소, 로컬 저장소 모든 git branch 목록 확인하기
git branch -a

# git fetch를 통해 원격 저장소와 로컬 저장소 동기화 하기
git fetch
```


<br />
<br />
<br />

{: .highlight } 
> - 새로 생긴 원격의 다른 브랜치(jjehyun) pull받기

```bash
# git fetch를 통해 원격 저장소와 로컬 저장소 동기화 하기
git fetch

# 아래 명령어로 로컬에 같은 이름의 브랜치를 생성하고 연결해서 swich
# 이후에도 로컬의 jjehyun branch와 원격의 jjehyun 브랜치를 연결하겠다.
git switch -t origin/jjehyun

## 원격의 브랜치 삭제하기
git push origin --delete jjehyun
```


<br />
<br />
<br />

---

# 깃의 3가지 공간

![Alt text](image-15.png)

{: .highlight } 
> - Working directory : 로컬 상태
> - Staging area : 푸쉬하기 이전 상태
> - repository : 깃 원격 레파지토리
>   - `git rm` : 파일명 
>       - 파일 삭제한 내용이 git Staging area에 자동 등록 됨
>   - `git mv` : 파일명
>       - 파일 명이 바뀐 것을 git Staging area에 자동 등록 됨

<br />
<br />
<br />

--- 

## 깃 스테이지에 올린 후 특정 파일만 제외하는 명령어

![Alt text](image-16.png)

{: .highlight } 
> - `git restore --staged 파일명`
> - `git restore 파일명`
>   - git add . 후 특정 파일만 스테이지에서 빼고 싶을 때 사용하는 명령어

<br />
<br />
<br />

--- 