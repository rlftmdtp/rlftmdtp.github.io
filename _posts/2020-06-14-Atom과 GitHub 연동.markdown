---
layout: post
title:  "Atom과 GitHub 연동"
date:   2020-06-14 13:33:10 +0700
categories:  develope
---

## 1. [Atom](https://atom.io/)

**Atom을 쓰는 이유?**

Atom은 GitHub에서 개발한 오픈소스 기반의 텍스트 에디터 입니다. JavaScript등의 IDE로도 사용이 가능하지만 제가 사용하는 목적은 아래의 3가지를 통해 편리하게 블로그를 포스트 하기 위함입니다.

* Jekyll serve를 통해 로컬에서 정적페이지를 생성해서 확인
* 각 MD 파일을 프리뷰 하는 것 (올리기 전에 검토)
* 추가한 블로그 글을 바로 github에 업로드 하는 것


[이 링크를 눌려 Atom을 설치해 줍시다.](https://atom.io/)

## 2. Git-plus 설치

**git-plus를 설치하는 이유?**

git bash를 열고 매번 해당 프로젝트 폴더로 이동하여 입력하지 않고, ctrl + shift + h 단축키로 Atom 에디어텡서 바로 git 명령어를 사용하기 위함

![git-plus](https://rlftmdtp.github.io/static/img/_posts/gitplusInstall.PNG)

Atom 설치가 완료되었다면 실행 후 **단축키(Ctrl + ,)**를 통해 Setting를 띄우고 Install를 클릭 후 git-plus를 검색하여 설치합니다.


[git에 대한 기본적인 지식이 있어야합니다. git에 대해 학습 하실려면 이 링크를 참조해보세요](https://rogerdudler.github.io/git-guide/index.ko.html)

## 3. Git-Clone

**git-clonde를 설치하는 이유?**

Git-Hub 원격에 이미 저장소가 생성되어 있다면 $git clone을 수행해야하는데 앞서 설치한 git-plus패키지에서는 해당 명령어를 제공해 주지 않습니다.
따라서 git-clone패키지를 git-plus때와 마찬가지로 설치해 줍니다.

![screenshot](https://rlftmdtp.github.io/static/img/_posts/gitcloneInstall.PNG)

설치 후 **ctrl + shift + p** 단축키 입력후 git-clone을 입력하여 실행 후 자신의 원격 주소를 입력합니다.

![screenshot](https://rlftmdtp.github.io/static/img/_posts/gitcloneget.PNG)


[GitHub에 처음부터 원격저장소를 생성하고 Atom과 연동하는 것이  궁금하다면 좋은사람의 개발 노트 글을 참고 해보세요.](https://niceman.tistory.com/105)
