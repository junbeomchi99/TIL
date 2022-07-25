# Spring 관련 내용

![](2022-07-22-11-02-11.png)





## RDBMS
- 관계형 데이터베이스 (Relational Database)를 생성하고 수정/관리 할수 있는 SW
- e.g. MySQL, PostgreSQL, Oracle Database, H2 DataBase

## JPA
- ORM (Object-Relational Mapping) = Class와 RDB의 테이블을 연결해준다는 뜻
    - 객채 간의 관계를 바탕으로 SQL를 자동으로 생성하여, RDB와 불일치를 해결 
    - 개발자는 메서드로 데이터를 조작가능하게되어 개발에 집중 할수 있음
- JPA = JAVA에서 ORM 기술 표준으로 사용하는 인터페이스 모음
    - JAVA에서 RDB를 사용하는 방식을 정의한 interface
- =JPA framework를 사용해 개발할떄 자바 언어로 프로그램을 짜다가 SQL로 짜는 복잡한 상황 방지, 모두 자바로 제어

## Framework
- 어플리케이션(SW)를 편리하고 효율적으로 제작하기 위해, 뼈대가 되는 Class와 Interface로 구성된 기본 설계 틀

# Spring의 특징 
- (https://velog.io/@emawlrdl/Spring-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC-60k5hr47w2)

## 1. Spring은 IOC 기반
- IOC = Inversion of Control (제어의 역전)
- = 객채의 생성 및 제어권을 사용자가 아닌 Spring에게 맡김. 
- 사용자는 new 연산을 통해 생성한 객채를 사용하지 않고, Spring에 관리당하는 자바 객채 (Bean)을 사용함 
- 일반적인 프로그램:
    - Object (객채) 결정 및 생성 -> 의존성 (dependency) 객체 생성 -> 객채 내의 메소드 호출
        - 각 객채들이 프로그램 흐름 결정, 각 객채를 구성하는 작업에 직접 참여
        - = 모든 작업을 사용자가 제어하는 구조
- IOC:
    - 객채가 자기가 사용할 객채를 선택하거나 생성하지 않음
    - + 자신이 어디서 만들어지고 어떻게 사용되는지 또한 모름
    - 자신의 모든 권한을 다른 대사엥 위임 -> 제어권한을 위임받은 특별한 객채에 의해 결정되고 만들어 짐
        - = 제어의 흐름을 사용자가 컨트롤 하지 않고 위임한 특별한 객체에 모든 것을 맡김
    - = 기존 사용자가 모든 작업을 제어하던 것을 특별한 객체에 모든 것을 위임하여 객채의 생성부터 생명주기 등 모든 객체에 대한 제어권이 넘어간것을 IOC라고 부름

## IOC 컨테이너
기존 방식:
1. 객채 생성
2. 의존성 객채 생성 - <b>클래스 내부에서 생성</b>
3. 의존성 객채 메서드 호출

Spring 방식:
1. 객채 생성
2. 의존성 객채 <b>주입</b> - <b>스스로 만드는 것이 아닌, 제어권을 스프링에게 위임하여 스프링이 만들어 놓은 객채를 주입</b>
3. 의존성 객채 메서드 호출

= Spring이 모든 의존성 객채를 Spring이 실행될떄 만들어주고, 필요한 곳에 주입 시켜 줌으로써, 
Bean들은 Singleton Pattern의 특징을 가지고, 제어의 흐름을 사용자가 컨트롤하는 것이 아닌 스프링이 처리

    https://gyoogle.dev/blog/design-pattern/Singleton%20Pattern.html

    - Singleton Pattern = 전역 변수를 사용하지 않고 <b>객채를 하나만 생성</b> 하도록 하여, 생성된 객채를 <b>어디에서든지 참조할 수 있도록</b> 하는 패턴
    - = instance가 필요할때, 똑같은 instance를 만들지 않고 기존의 instance를 활용
    - 메모리 낭비 방지,다른 클래스의 인스턴스들이 데이터 공유 받는다는 장점


## IOC 구성요소: DI, DL
- DL (Dependency Lookup) - 의존성 검색
    - IOC 컨테이너에서는 객채를 관리하기위해 별도의 저장소에 Bean을 저장함
        - Bean = Spring Container가 관리하는 자바 객체 
    - DL = Bean에 접근하기 위해 (저장소에 저장되어있는) 컨테이너가 제공하는 API를 사용하여 Bean을 Lookup/검색 하는것 
- DI (Dependency Injection) - 의존성 주입
    - 각 클래스 간의 의존성을 자신이 아닌 외부 (컨테이너)에서 주입하는 것
    - = 각 클래스 사이에 필요로 하는 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것 
        - Bean 설정 파일을 바탕으로 의존관계 확인하여 주입 
        - 객채 reference를 container로 부터 주입 받아, 실행 시 동적으로 의존 관계 생성 
        - = 컨테이너가 흐름에 주체가 되어 application code에 의존 관계 주입 

## 2. Spring Framework의 특징 POJO
- POJO = Plain Old Java Object
- getter/setter를 가진 단순 자바 오브젝트
- 의존성이 없고, 추후 테스트 및 유지보수가 편리한 유연성이 장점
    - 객채지향적인 다양한 설꼐와 구현이 가능해짐으로, POJO기반 Framework가 조명 받고있음

## 3. Spring Framework에 특징 AOP
- AOP = Asect Oriented Programming = 관점 지향 프로그래밍 
    - 대부분 SW 개발 process는 OOP (Ojbect Oriented Programming)
        - 관심사가 같은 데이터를 한곳에 모아 분리 + 낮은 결합도를 갖게하여 독립적이고 유연한 모듈로 캡슐화
        - 중복 코드가 많아지고, 가독성, 확장성, 유지 보수성을 떨어트림
- AOP = 핵심기능 / 공통 기능을 분리시켜 핵심 로직에 영향을 끼지치 않게 공통기능을 끼워 넣는 개발 형태
    - 무분별하게 중복되는 코드를 한 곳에 모아 중복 코드 제거
    - 공통기능을 한 곳에 보관함으로써 공통 기능 하나의 수정으로 모든 핵심기능들의 공통기능 수정 가능 
    - = 효율적인 유지보수 + 재활용성 극대화 

- OOP의 흐름
- = Business Logic을 모듈화 함
![](2022-07-22-12-24-06.png)

- AOP의 흐름 (공통 관심을 종단으로 삽입)
    - = 인프라/부가기능을 모듈화함 (e.g. 권한체크, 인증, 예외처리 등)
    - 기능을 Business Logic과 공통 모듈로 구분 -> 개발자가 코드 밖에서 필요할 때 business logic에 삽입하여 실행
    - e.g. 게시판 기능에서 "권한체크, 인증, 예외처리 등"은 필요한 시점에 자동으로 삽입되도록함 
![](2022-07-22-12-24-28.png)

## Spring Framework의 특징 MVC (Model2)
https://velog.io/@seongwon97/MVC-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80
- MVC 패턴 = Model, View, Controller
    - Model = 백그라운드에서 동작하는 로직을 처리
        - Data와 App이 무엇을 할지 정의 = 내부 비즈니스 로직 처리 
        - 모델은 컨트롤러가 호출을 하면 DB와 연동하여 사용자의 입출력 데이터를 다루는 일과 같은 데이터와 연관된 비즈니스 로직 처리
        - = Data 추출, 저장, 삭제 업데이트 등의 역활을 수행
    - View = 사용자가 보게 될 결과 화면을 출력
        - 화면(UI)
        - 사용자와 상호작용하며 컨트롤러로 부터 받은 모델의 결과값을 사용자에게 화면으로 출력
        - Model에서 받은 데이터는 별도로 저장하지 않음
    - Controller = 사용자의 입력처리와 흐름 제어 담당
        - Model과 View 사이를 이어주는 Interface 역활
        - Model이 데이터를 어떻게 처리할지 알려주는 역활
        - = 사용자 View 에서 요청 -> Controller 가 해당 업무 수행하는 Model 호출 -> Model 업무 모두 수행시 -> Controller가 다시 결과를 -> View에 전달
    - View 와 Model은 서로의 존재를 몰라야 되며, Controller가 view,model에 대해 알며 변경을 모니터링해야됨 
    ![](2022-07-22-13-49-35.png)
- 장점
    - 가독성과 코드 재사용 증가
    - 각 구성요소 독립시켜 분업을 가능하게 함
    - 개발 후에도 유지보수성 및 확장성 보장
- 한계
    - 다수의 view 와 Model이 Controller을 통해 연결되며 불필요하게 커지는 현상도 발생할수 있음 (Massive-View-Controller 현상)

## MVC패턴에 모델1, 모델2 방식
- Model 1 방식
![](2022-07-22-13-53-12.png)
    - 사용자 요청을 JSP가 전부 처리. 
    - 웹브라우저 사용자의 요청을 받은 JPS는 Java Bean or Service Class를 사용하여 웹브라우저가 요청한 작업을 처리하고 그 결과를 출력해줌
- Model 2 방식
![](2022-07-22-13-54-06.png)
    - 웹브라우저 사용자의 요청을 Sublet이 받음
    - 서블릿은 웹브라우저 요청을받아 View로 보여줄것인지 Model로 보내줄것인지 정하여 전송
    - 모델2 = 실질적으로 보여지는 HTML (view)와 Java 소스를 분리 해놓았기 때문에 model1에 비해 개발 확장이 쉽고 유지보수도 쉬움 