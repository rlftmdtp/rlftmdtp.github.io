---
layout: post
title:  "Maven package 해보기(manifest(maven-jar-plugin), 의존라이브러리(maven-dependency-plugin) 설정)"
date:   2022-05-12 15:15:15 +0700
categories:  [develope, java]
---

## [1. try-catch, throws 중 어떤걸 사용하는게 더 적절할까요?](https://jenkov.com/tutorials/maven/maven-commands.html#common-maven-commands)

생애 첫 라이브 코딩 후 코드리뷰를 진행 했었다. 자동완성으로 throws를 선언하고 json 라이브러리를 활용하는 참고 코드에서 복붙을 하다보니 throws와 try-catch 둘 다 선언이 되어있었다.

**면접관님께서 try-catch 와 throws를 언제 적절하게 쓰이는가에 대해 물어보았다.** try-catch와 throws의 차이는 알고있지만
어떤 상황에서 적절하게 사용해야할지 평소 의문만 가지고 결론을 못 내렸었다.

그래서 이번에 한번 정리를 해보고자 한다. 예외처리의 적용지점은 어디인가 ?

일단 try-catch문에 대해서 찾아보면 부정적인 정보밖에 없다. 

* 코드의 가독성이 떨어지게 된다
* 불필요한 예외처리 구문에 의해 성능저하 등의 이슈를 발생시킨다
* 유지보수하기 어려워 진다. (try-catch지옥)

심지어 클린코드의 책에는 아래와 같은 구절이 있다고 한다.
>try/catch 블록은 원래 추하다. 코드 구조에 혼란을 일으키며, 정상 동작과 오류 처리 동작을 뒤섞는다.

그러면 try-catch문을 안쓰고 throws만 사용하면 되는거 아닌가...?? 물론 어딘가에서는 최종적으로 try-catch문으로 처리를 해야할 것이다.
그래서 어떤상황에 try-catch를 사용하고 throws를 사용하는게 더 적절할지 알아보고자 한다.

---

## [2. 오류(Error)와 예외(Exception)]()

일단 try-catch문을 사용하기전에 오류와 예외의 차이부터 알아보자. 

* 오류(Error) : 응용프로그램이 시스템 리소스 부족과 같은 **실행되는 환경**에서 발생하는 현상입니다. 
오류는 런타임에 발생하며 컴파일에서 알 수 없기 떄문에 프로그래머의 권한 밖이다.


* 예외(Excption) : 응용프로그램 **코드**로 인해 발생하는 상태이다 따라서 try-catch 블록을 사용하여 예외를 처리하고 에외로부터
애플리케이션을 복구 할 수 있다.

이처럼 예외 발생할 시 프로그래머가 에측되는 상황에 대해서 적절하게 미리 처리를 할 수 있도록 try-catch문 활용하여 코딩을 하는 것이다.
````java
try {
        BufferedReader br = new BufferedReader(...);
        method1();  // 여기서 예외 발생 시 try안에 뒷 코드는 실행되지 않는다. 
                    // 이 특징으로 인해 뒤에 언급할 try-cath를 어떻게 활용할지도 생각 해 볼 수 있다.
        method2(); 
        } catch(예외1) {
        ...
        } catch(예외2) {
        ...
        ...
        }finally{
            br.close();
            // 예외의 발생 유무에 상관없이 무조건 실행
            // 그렇기 떄문에 BufferedReader와 같은 객체의 close()선언
            // 자바7 에서는 try-with-resource 구문으로 try(...)에서 선언된 객제들의 자원을 쉽게 해제할 수 있다.
        }
````

## [3. JAVA에서의 오류(Error)와 예외(Exception) 처리]()


Java에서 오류(Error)와 예외(Exception)는 아래의 이미지와 같이 처리되도록 구현되어 있다.

![자바예외계층관계](https://rlftmdtp.github.io/static/img/posts/20220531/JavaExceptionhierarchy.gif)

1. RuntimeException(컴파일 성공 후 **실행**시 발생되는 예외 = 발생할 수도 있고 안할 수도 있다 = Unchecked Exception)
2. Exception(**컴파일**시 발생되는 예외 = 프로그램 작성 시 에측가능한 예외 작성 = Checked Exception)

RuntimeException 예시로는 0으로 나누는 것, Null 발생과 같은 발생할 수도 있고 안 할 수도 있는 상황으로 try-catch를 강요하지 않는다.
반면 Exception은 읽을 파일이 존재하지않는 경우과 같은 예측가능한 에러이기 때문에 컴파일러가 **try-catch 예외 처리를 강제**한다.

코드로 직접 예시를 보자
### RuntimeException
```java
class FoolException extends RuntimeException { }

public class Sample {
    public void sayNick(String nick) {
        if("fool".equals(nick)) {
            throw new FoolException();
        }
        System.out.println("당신의 별명은 "+nick+" 입니다.");
    }

    public static void main(String[] args) {
        Sample sample = new Sample();
        sample.sayNick("fool");
        sample.sayNick("genious");
    }
}
```

### Exception
```java
class FoolException extends Exception { }

public class Sample {
  public void sayNick(String nick) {
    try {
      if("fool".equals(nick)) {
        throw new FoolException();
      }
      System.out.println("당신의 별명은 "+nick+" 입니다.");
    }catch(FoolException e) {
      System.err.println("FoolException이 발생했습니다.");
    }
  }

  public static void main(String[] args) {
    Sample sample = new Sample();
    sample.sayNick("fool");
    sample.sayNick("genious");
  }
}

```
RuntimeException에서 Exception으로 변경했을 경우 예외처리를 컴파일러가 강제하였기 떄문에 try-catch로 처리를 하였다.

## [4. 예외 던지기(throws)]()
예외를 발생한 곳에서 말고 호출한 곳에서 처리하도록 예외를 다른 곳으로 던질 수 있는 방법이 있다.
메서드명 뒤에 throws를 추가 하면 된다.
```java
public void method() throws Exception{}
```

위의 Samle.class를 throws로 수정하면 sayNick()메서드에서 사용하였던 try-cath문은 사라지게되고 main()메서드에
try-catch문이 추가된다.
### throws로 변경
```java
class FoolException extends Exception { }

public class Sample {
  public void sayNick(String nick) throws FoolException {
    if("fool".equals(nick)) { // 기존에 사용하였던 try-catch가 호출했었던 main()메서드로 이동하였다.
      throw new FoolException();
    }
    System.out.println("당신의 별명은 "+nick+" 입니다.");
  }

  public static void main(String[] args) {
    Sample sample = new Sample();
    try {
      sample.sayNick("fool"); // sayNick()이 throws하였으므로 try-catch 로 처리한다.
      sample.sayNick("genious");
    } catch (FoolException e) {
      System.err.println("FoolException이 발생했습니다.");
    }
  }
```

## [5. 그래서 언제 예외를 던지고 언제 예외를 처리 할껀데?]()

그렇다면 예외처리를 sayNick() 메서드에서 하는것이 좋을까? 아니면 main() 메서드에서 하는것이 좋을까?
(부제 : [계속 throws만 하다보니 main()메서드에 throws를 했다. 이건 누가 처리할까?]())

sayNick 메소드에서 예외를 처리하는 경우에는 두 가지 메서드가 모두 수행된다.
```java
sample.sayNick("fool");
sample.sayNick("genious");
```

물론 sample.sayNick("fool"); 수행 시에는 FoolException이 발생하지만 다음 문장인 sample.sayNick("genious"); 역시 수행이 된다.

하지만 main() 메서드에서 예외 처리를 한 경우에는 두번 째 문장인 sample.sayNick("genious");가 수행되지 않는다.  왜냐하면 이미 첫번 째 문장에서 예외가 발생하여 catch 문으로 빠져버리기 때문이다.
```java
 public static void main(String[] args) {
        Sample sample = new Sample();
        try {
        sample.sayNick("fool"); 
        sample.sayNick("genious");  // 이 메서드는 수행되지 않는다.
        } catch (FoolException e) {
        System.err.println("FoolException이 발생했습니다.");
        }
}
```
`프로그램의 수행여부를 결정`하기 때문에 Exception을 처리하는 위치는 매우 중요하며 그래서 면접관님께서 질문을 하였던 것 같다.

## [6. 결론]()
애플리케이션이 커질수록 예외는 여러 곳에서 발생 할 수도 있고 이것을 공통적으로 한곳에 모아서 처리를하면 좋아질 것이다. 여기에
비즈니스적 요소의 예외까지 추가가 될 것이다.(예를 들면 유효기간이 지난 할인 쿠폰을 쓴다던가...)

그렇다면 최종적으로 어디에서 예외를 처리해 줄 것인가? 정답이 존재하는 것은 아니지만공통적으로 여러가지 메소드를 사용하는 시작 지점이 좋을 것이라 생각된다.

예를 들어 그냥 자바라면 main()메서드가 될 수도 있고 Spring Framework라면 어떤 서비스를 시작하게 되는 Controller가 될 수도 있을 것이다.
그렇기 떄문에 Controller에서의 try-catch를 없애고 모든 예외를 핸들링 할 수 있도록 @ControllerAdvice과 같은 어노테이션이 존재하는 나름의 이유라고 생각한다.

---
+ 참고문헌
  * https://wikidocs.net/229
  * https://cheese10yun.github.io/spring-guide-exception/
  * https://www.educative.io/edpresso/what-is-the-difference-between-errors-and-exceptions-in-java