---
layout: post
title:  "Maven에 대해서 제대로 아시나요?(Maven 설치하기)"
date:   2022-05-11 13:33:10 +0700
categories:  develope
---

필자의 경우 맨처음 Maven을 .jar와 같은 파일을 직접 다운받아서 내 로컬에 옮기는 것이 아닌 Maven사이트에 등록된 라이브러리를 내 프로젝트의 pom.xml에서 dependency를 추가해서 편하게 가져다 쓰는 용도로만 사용했다.

그랬던 내가 빌드 배포를 위해 Maven을 제대로 사용하고 공부했던 내용을 정리하고자 한다.

## 1. Maven이란?

Apache Ant, Maven, Gradle 과 같은 것들을 **빌드도구**라고 한다.
Build 는 작성된 Source Code을 실제 기기 ( 컴퓨터, 핸드폰 ) 등에서 실행 될 수 있는 소프트웨어로 변한화기 위한 과정을 하는 것을 말하며, Build Tool 은 이러한 과정을 해주는 것이다.

<details>
<summary>(빌드도구에 대한 나의 생각)</summary>
흔히들 빌드도구라고 칭한다...

하지만 빌드도구가 무엇인지에 대해서 위와 같이 서술되고 가슴에 와닿게 설명해주는 글은 잘없다. 

그렇다면 빌드도구가 무엇인가? 위 글과 같이 소프트웨어로 변환하기 위한 과정이지만
가장 로우레벨로 생각하면 cmd창에서 javac 이런식으로 입력해서 소스코드를 바이너리코드로 바꾸는 것이다.

지금 우리들은 흔히 Eclipse, Intelij와 같은 IDE를 편리하게 사용함으로 와닿지 않는 것인데 이것들을 하나씩 사람이 직접 할 수 없으니깐 한꺼번에 관리 할 수 있도록 나온게 빌드도구이며
Ant apache, Maven, Gradle은 단순 빌드뿐만 아니라 문서화, 의존관계관리, 릴리즈. 배포까지 모든 단계를 관리해주도록 발전이 된 것이다.
<div markdown="1">
</div>
</details>

그래서 Maven이라는 Build Tool이 지원해주는 기능들은 아래와 구성되어있다.

* 1. Build : 소스코드 컴파일
* 2. Package : 배포 가능한 파일 생성 : war,jar,exe 등
* 3. Test : 단위 테스트 수행, 빌드 결과 정상적인지 점검
* 4. Report : Build, Package, Test 결과 정리 및 문서화(Report 생성)
* 5. Release : Build 후 생성된 결과물(Artifact)를 **특정 저장소**에 배포

**이 특정 저장소라는 것이 로컬이든 원격저장소이든 내가원하는 곳에 할 수 있는데 Maven의 중앙저장소에 모여있는 라이브러리들을
우리들은 편하게 pom.xml dependency 설정을 통해 간편하게 사용하는 것이다.**


## 2. Maven 설치
(Maven은 Java 도구이므로 이전에 당연히 Java가 설치 되어 있어야 한다.)

[Maven 설치링크](https://maven.apache.org/download.cgi)에 가서 설치를 한다.

그 후 Java 설치 떄와 마찬가지로 모든 경로에서 명령어를 사용할 수 있도록 PATH를 설정한다.

제어판 > 시스템 > 고급 시스템 설정  > 고급 > 환경변수 > 시스템 변수 Path 항목에 Maven bin 위치 추가
잘 설정 되었다면 cmd 창에서 mvn -version을 치면 아래와 같이 나온다

이미지추가
![git-plus](https://rlftmdtp.github.io/static/img/posts/gitplusInstall.PNG)


## 3. 프로젝트 생성하기

```java
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

위와같이 명령어를 실행하면 실행 디렉터리에 my-app이라는 폴더가 생성된다.

이 프로젝트를 통해 Java를 열심히 코딩을 한다.

이후 Maven을 사용하면서 겪고 해결했던 글을 이어쓰고자 한다.