---
layout: post
title:  "SSL, Mixed content에 대한 코딩해결 방안"
date:   2021-08-08 23:57:10 +0700
categories:  develope
---

##  Mixed Contetn Error 발생 이유?

웹 브라우저들은 보안상의 이유로 javascript 나 ajax로 다른 도메인의 서버에 요청하는 것을 보안위반으로 간주하고 차단한다.(익스는 허용, 크롬은 차단)
![git-plus](https://rlftmdtp.github.io/static/img/posts/Mixed Contetn Error.jpg)

https 홈페이지에서 http홈페이지로 전송될떄 프로토콜이 다르므로 CORS에 포함됨 (다만 http->https는 가능)


 [Cross-Origin Resource Sharing(CORS) 정보](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

따라서 Mixed content : ~ This request has been blocked; the content must be served over HTTPS 에러로 감지

[https를 사용하는 사용자가 http로 이동하는 것은 안전하지 않은 리소스 활성화는 다양한 공격과 보안에 대한 사용자의 기대를 위반하기 떄문](ttps://stackoverflow.com/questions/35178135/how-to-fix-insecure-content-was-loaded-over-https-but-requested-an-insecure-re)


##  해결방안
https에서 http로 전송할 수 있는 근본적인 해결방은 없음 호출되는 서버를 SSL로 구성하거나 각 브라우저별 인터넷 옵션을 설정해야한다
서버에 SSL를 설정했다면 http로 접속할시 httpS로 변경하는 코드 추가
```java
String url = request.getRequestURL().toString();
if(url.startsWith("http://") < 0) {
    url = url.replaceAll("http://", "https://");
    response.sendRedirect(url);
}
```


코드를 통해 https로 변경 되었지만 현재 프로젝트 내에서 http로 이동하는 페이지가 존재한다면 역시나 Mixed contetn~ 에러가 발생하므로
```java
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests" />
```
[위의 메타그는 현재 나의 페이지의 http 요청을 자동으로 https 요청으로 일괄적으로 변경하여 전송하라는 뜻이다.](https://web.dev/fixing-mixed-content/)
