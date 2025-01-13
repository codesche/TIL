
## TIL 주제
- 스프링 부트

## 주요 내용

### 스프링 부트란?
- 스프링 프레임워크를 더 빠르고 더 쉽게 사용할 수 있도록 도와주는 툴
- production-grade(제품 수준)의 독립적인(stand-alone) 스프링 기반 애플리케이션을 빠르고 쉽게 만들 수 있도록 해준다.
- 스프링 플랫폼 및 서드파티 라이브러리에 대한 기본 설정을 제공한다.

### 스프링 부트의 목적
- 빠르고 폭넓은 사용성을 제공한다.
- 일일히 설정을 할 필요가 없으며 얼마든지 설정을 커스터마이징할 수 있다.
- 비즈니스 로직에 필요한 기능들을 구현하는 데 있어 필요한 기능들을 제공한다.
- 더 이상 XML 설정과 code generation(조사 필요)을 쓰지 않는다.

### 프로젝트 생성
- Intellij Professional에 있는 기능인 Spring Initializr을 사용하는 방법이 있다.
- start.spring.io 를 이용하여 기본 구조를 내려받는 방법이 있다.
- Maven 혹은 Gradle로 프로젝트를 생성한 뒤 의존성을 직접 주입하는 방법이 있다.

### 프로젝트 빌드
- IDE에서 빌드하거나 mvn, gradle에서 빌드 가능하다.
- Maven을 이용해 빌드한 하 .jar 파일을 생성하는 과정이 있다.

```
mvn package
java -jar target/<your-project-name>.jar
```

### 스프링 부트 프로젝트 구조
- Maven 기본 프로젝트 구조와 동일
  - 소스 코드(/src/main/java)
  - 소스 리소스(/src/main/resource)
  - 테스트 코드(/src/test/java)
  - 테스트 리소스(/src/test/resource)
- 메인 애플리케이션의 위치
  - 기본 패키지(예시)
    ```
        /src/main/java/jaehoon/kim/springinit/Application.class
    ```

### 스프링 부트의 장점
- 설정을 하는 시간을 줄일 수 있다. 즉, 다른 업무의 능률을 더욱 높일 수 있다.
- 의존성 관리가 줄어들어 개발자가 해야 할 일들이 줄어든다.
- 의존성 관리를 할 때 버전 명시를 해주지 않아도 된다.

## 정리
스프링 부트는 스프링 프레임워크를 좀 더 간편하고 사용자 친화적으로 활용할 수 있게 해주는 툴이다. 