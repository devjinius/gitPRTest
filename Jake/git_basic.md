# Git

## 개념

### DVCS

Git은 DVCS(분산 버전 관리 시스템)이다. Git같은 DVCS에서의 클라이언트 저장소를 히스토리와 더불어 전부 복제한다. 서버에 문제가 생기면 이 복제물로 다시 작업을 시작할 수 있다. 클라이언트 중에서 아무거나 골라도 서버를 복원할 수 있다. Clone은 모든 데이터를 가진 진정한 백업이다.

![분산 버전 관리 시스템(DVCS)](https://git-scm.com/book/en/v2/images/distributed.png)

### Git이 데이터를 다루는 방법

Git은 데이터를 파일 시스템 스냅샷의 연속으로 취급하고 크기가 아주 작다. Git은 커밋하거나 프로젝트의 상태를 저장할 때마다 파일이 존재하는 그 순간을 중요하게 여긴다. 파일이 달라지지 않았으면 Git은 성능을 위해서 파일을 새로 저장하지 않는다. 단지 이전 상태의 파일에 대한 링크만 저장한다. Git은 데이터를 **스냅샷의 스트림**처럼 취급한다.

![시간순으로 프로젝트의 스냅샷을 저장.](https://git-scm.com/book/en/v2/images/snapshots.png)



### Git의 무결성

Git은 데이터를 저장하기 전에 항상 체크섬을 구하고 그 체크섬으로 데이터를 관리한다. 그래서 체크섬을 이해하는 Git 없이는 어떠한 파일이나 디렉토리도 변경할 수 없다. 체크섬은 Git에서 사용하는 가장 기본적인(Atomic) 데이터 단위이자 Git의 기본 철학이다. Git 없이는 체크섬을 다룰 수 없어서 파일의 상태도 알 수 없고 심지어 데이터를 잃어버릴 수도 없다.

### Git은 데이터를 추가하기만 한다

Git으로 무얼 하든 Git 데이터베이스에 데이터가 *추가* 된다. 되돌리거나 데이터를 삭제할 방법이 없다. 다른 VCS처럼 Git도 커밋하지 않으면 변경사항을 잃어버릴 수 있다. 하지만, 일단 스냅샷을 커밋하고 나면 데이터를 잃어버리기 어렵다. 

[되돌리기](https://git-scm.com/book/ko/v2/ch00/_undoing)을 보면 Git에서 데이터를 어떻게 저장하고 손실을 어떻게 복구하는지 알 수 있다.

### 세 가지 상태

Git은 파일을 *Committed*, *Modified*, *Staged* 이렇게 세 가지 상태로 관리한다.

- `Committed`란 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미한다.
- `Modified`는 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 말한다.
- `Staged`란 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태를 의미한다.

![Working tree, staging area, and Git directory.](https://git-scm.com/book/en/v2/images/areas.png)

Git으로 하는 일은 기본적으로 아래와 같다.

1. 워킹 트리에서 파일을 수정한다.
2. Staging Area에 파일을 Stage 해서 커밋할 스냅샷을 만든다. 모든 파일을 추가할 수도 있고 선택하여 추가할 수도 있다.
3. Staging Area에 있는 파일들을 커밋해서 Git 디렉토리에 영구적인 스냅샷으로 저장한다.

Git 디렉토리에 있는 파일들은 Committed 상태이다. 파일을 수정하고 Staging Area에 추가했다면 Staged이다. 그리고 Checkout 하고 나서 수정했지만, 아직 Staging Area에 추가하지 않았으면 Modified이다. [Git의 기초](https://git-scm.com/book/ko/v2/ch00/ch02-git-basics-chapter)에서 이 상태에 대해 좀 더 자세히 배운다. 특히 Staging Area를 이용하는 방법부터 아예 생략하는 방법까지도 설명한다.

### 워킹 트리, Staging Area, Git 디렉토리(.git)

`Git 디렉토리(local repo)`는 Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳을 말한다. 이 Git 디렉토리가 Git의 핵심이다. 다른 컴퓨터에 있는 저장소를 *Clone* 할 때 Git 디렉토리가 만들어진다.

`워킹 트리`는 프로젝트의 특정 버전을 Checkout 한 것이다. Git 디렉토리는 지금 작업하는 디스크에 있고 그 디렉토리 안에 압축된 데이터베이스에서 파일을 가져와서 워킹 트리를 만든다.(쉽게 말해 .git 뺀 나머지)

`Staging Area`는 Git 디렉토리에 있다. 단순한 파일이고 곧 커밋할 파일에 대한 정보를 저장한다. Git에서는 기술용어로는 “Index” 라고 하지만, “Staging Area” 라는 용어를 써도 상관 없다.

### Git Configuration

Git 설정은 `git config`라는 명령어로 할 수 있다. 이때 사용하는 설정 파일은 세 가지가 있다.

- `/etc/gitconfig`: 시스템의 모든 사용자와 모든 저장소에 적용되는 설정이다. `git config --system` 옵션으로 이 파일을 읽고 쓸 수 있다.(root 권한 필요)
- `~/.gitconfig`, `~/.config/git/config`: 특정 사용자(즉 현재 사용자)에게만 적용되는 설정이다.`git config --global` 옵션으로 이 파일을 읽고 쓸 수 있다. 특정 사용자의 *모든* 저장소 설정에 적용된다.
- `.git/config` : 이 파일은 Git 디렉토리에 있고 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용된다. `--local` 옵션을 사용하면 이 파일을 사용하도록 지정할 수 있다. 하지만 기본적으로 이 옵션이 적용되어 있다. (당연히, 이 옵션을 적용하려면 Git 저장소인 디렉토리로 이동 한 후 적용할 수 있다.)

각 설정의 우선순위는 역순이다. 

예시

```console
# 사용자 정보 
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com

# 기본 편집기 수정
$ git config --global core.editor emacs
```

### .git 디렉토리 구조

`git init` 명령어를 사용하면 .git 디렉토리와 내부 파일이 생성된다. `.git` 디렉토리 구조는 다음과 같다.

```console
.git$ ls -F1
config
description
HEAD
hooks/
info/
objects/
refs/
```

- ` config`: 해당 프로젝트에만 적용되는 설정 옵션이 들어 있다.
- `info`: `.gitignore`파일처럼 무시할 파일의 패턴을 적는 곳이다. 그러나 `.gitignore`파일과는 다르게 Git으로 관리되지 않는다.
- ` hooks`: 클라이언트 훅이나 서버 훅이 위치한다. 
-  `HEAD`: 현재 Checkout 한 브랜치를 가리킨다.
- `objects`: 모든 컨텐츠를 저장하는 데이터베이스 디렉토리.
- `refs`: 커밋 객체의 포인터(branch, tag, remote 등)를 저장한다.
- `index`: Staging Area의 정보를 저장한다.

### 파일의 라이프사이클

![The lifecycle of the status of your files.](https://git-scm.com/book/en/v2/images/lifecycle.png)



## Git 기초

### 저장소 만들기

두가지 방법이 존재한다.

#### 로컬 디렉토리를 Git 저장소로 만들기

```console
$ cd ~/workspace/my_repo
$ git init
```

#### 기존 저장소를 clone하기

```console
$ git clone https://github.com/username/repo
```

Git은 http, git, ssh 등 다양한 프로토콜을 지원한다.

## 

## Git convention

첫번째 원격 저장소 - origin(git remote -v)]

첫번째 커밋 - master

### Git 수정

git commit --amend

내용이 바뀔 때 object가 생김

중복된 내용은 파일이 여러개여도 하나만 저장됨

Edab