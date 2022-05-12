---
layout: post
title:  "Maven package 해보기(manifest(maven-jar-plugin), 의존라이브러리(maven-dependency-plugin) 설정)"
date:   2022-05-12 15:15:15 +0700
categories:  [develope, maven]
---

## [1. Maven commands](https://jenkov.com/tutorials/maven/maven-commands.html#common-maven-commands)

Maven의 Build 관련하여 다양한 명령어들이 아래와 같이 존재한다.

<table class="table table-hover">
      <thead>
        <tr>
          <th>Maven Command</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">Clean</th>
          <td>빌드 시 생성되었던 파일들 삭제(target 폴더 삭제)</td>
        </tr>
        <tr>
          <th scope="row">Validate</th>
          <td>프로젝트가 올바른지 확인하고 필요한 모든 정보를 사용할 수 있는 지 확인</td>
        </tr>
        <tr>
          <th scope="row">Compile</th>
          <td>소스 컴파일</td>
        </tr>
        <tr>
          <th scope="row">Test</th>
          <td>유닛(단위) 테스트를 수행(테스트 실패시 빌드 실패로 처리, 스킵 설정 가능)</td>
        </tr>
        <tr>
          <th scope="row">Package</th>
          <td>실제 컴파일된 소스 코드를 jar, war 등 배포를 위한 패키지로 만듬</td>
        </tr>
        <tr>
          <th scope="row">Verify</th>
          <td>통합 테스트 결과에 대한 검사를 실행하여 충족하는지 확인</td>
        </tr>
        <tr>
          <th scope="row">Install</th>
          <td>패키지를 로컬 저장소에 설치</td>
        </tr>
        <tr>
          <th scope="row">Site</th>
          <td>프로젝트 문서와 사이트 작성, 생성</td>
        </tr>
        </tr>
        <tr>
          <th scope="row">Deploy</th>
          <td>만들어진 package 를 원격 저장소에 release</td>
        </tr>
      </tbody>
</table>

---

## [2. Maven clean package 실행해보기]()
프로젝트 디렉터리에서 mvn clean package 명령어를 입력하면 패키징이 시작된 후 **target이라는 디렉터리가 생성**된다

![package 실행 이미지추가]()

target 디렉터리로 이동하여 java -jar ~.jar 파일을 실행하면 어떤 오류가 생기겠지 target/SampleMavenApp-1.0-SNAPSHOT.jar에 기본 Manifest 속성이 없습니다.
Manifest 설정이 이루어지지 않았기 떄문이다. 이 오류를 해결하기 위해서 maven-jar-plugin라는 플러그인을 dependecny에 설정 추가한다.

```xml
<plugins>
        ...
        <plugin>
            <!-- Build an executable JAR -->
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.2.2</version>
            <configuration>
                <archive>
                    <manifest>
                        <addClasspath>true</addClasspath>
                        <classpathPrefix>lib/</classpathPrefix>
                        <mainClass>com.nhn.HttpServer</mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>
        ...
</plugins>
```

만약 외부 라이브러리를 import 해서 포함시켰다면 해당 `의존 라이브러리`들도 package시 같이 추가되도록 
maven-dependency-plugin라는 플러그인을 dependecny에 설정 추가 해준다.

```xml
<plugins>
        ...
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <executions>
                <execution>
                    <id>copy-dependencies</id>
                    <phase>package</phase>
                    <goals>
                        <goal>copy-dependencies</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>${project.build.directory}/lib</outputDirectory>
                        <overWriteReleases>false</overWriteReleases>
                        <overWriteSnapshots>false</overWriteSnapshots>
                        <overWriteIfNewer>true</overWriteIfNewer>
                    </configuration>
                </execution>
            </executions>
        </plugin>
        ...
</plugins>
```

---

## [3. cmd 창 한글깨짐]()

혹시나 Build시 cmd창에서 한글이 깨져 보인다면 properties에 utf-8 설정을 추가한다.

```xml
<properties>
        ...
    <!-- 한글깨짐 -->
    <project.build.sourceEncoding>utf-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>utf-8</project.reporting.outputEncoding>
        ...
</properties>
```

---
+ 참고문헌
  * <https://jenkov.com/tutorials/maven/maven-commands.html#common-maven-commands>
  * <https://mand2.github.io/til/build-plugs-of-maven/>
