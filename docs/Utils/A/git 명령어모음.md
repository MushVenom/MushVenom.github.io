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