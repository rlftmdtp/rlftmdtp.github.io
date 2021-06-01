---
layout: post
title:  "Atom과 GitHub 연동"
date:   2021-06-01 13:33:10 +0700
categories:  git/github
---

## 1. Updates were rejected because the remote contains work that ~

**첫번째 오류**

[인종차별 이슈로 인한 Github의 기술용어 변경](https://atom.io/)에 따른 새로운 main branch에 push 하려고 하자 아래와 같은 오류가 발생했다.

원인은 GitHub repository 생성 할 때 readme.md를 생성했기 떄문이다.(github는 항상 원격저장소와 동일하게 맞춰준 다음 push가 가능하다)
따라서 git pull을 하여 해결

![git-plus](static\img\posts\01main 브런치 생성과 이동 후 push 했을때 에러(원격저장소에 ReadMe가 있기 떄문).PNG)

## 2. Updates were rejected because the tip of your current branch is behind ~  

**두번째 오류**

이 오류는 데이터 유실 등 문제가 있을 수 있기 때문에 git에서 처리 되지 않도록 에러를 띄우는 것 원인을 찾아서 해결하거나 "+"를 사용하여 강제로 push 가능

git push -u origin +master

![git-plus](static\img\posts\02에러해결을 위해서 pull 한후 다시 push.PNG)
