# 기본 자바 상식 

https://gmlwjd9405.github.io/2018/09/17/class-object-instance.html
## 1. 클래스
- 객채를 만들어 내기 위한 설계도/틀
- 연관되어 있는 변수와 메서드의 집합

## 2. 객채 (Object)
- SW 세계에서 구현할 대상
- 클래스에 선언된 모양 그대로 생성된 실체
- '클래스의 instance'라고도 부른다

## 3. 인스턴스 (Instance)
- 설계도를 바탕으로 SW 세계에 구현된 구체적인 실체
    - 즉, 객채를 SW에서 실체화 하면 instace라고 부름
    - 실체화된 instance는 메모리에 할당 됨

```java

public class Dog(){
  String kind;
  String name;
  
  public Dog(String kind, String name){
      this.kind = kind;
      this.name = name;
  }
  
  public void bark() {
  
  }
  
  public void run() {
  
  }
}
Dog dog = new Dog()
dog.name = "뽀삐"
dog.kind = "치와와"

```

- Class = Dog처럼 객채 (Object)를 만들어 내기 위한 틀 
- Object = Class라는 틀안에서 만들어지며, 클래스의 메소드와 속성을 가짐
    - 클래스의 이름과 같은 Method = 생성자
    - 생성자는 객채를 생성할 때 객채를 초기화 해주는 역활을 함
    - 클래스 내에서 사용되는 <b> this </b> 는 객채 내에 또는 클래스 내의 변수를 가르킴 
- <b> private </b> 은 다른 클래스 내에서 private을 접근 할 수 없게 만듬 (민감한 정보 read&write 제한)
    - 접근을 위해서 <b> Getter & Setter </b>를 사용 

```Java

  public class Student() {
    private String name;
    public String grade;
    
    public String getName() { // Getter
        return this.name;
    }
    
    public void setName(String name) { // Setter
        this.name = name
    }
  }
  
  Student student = new Student();
  tutor.name = "Kwnaghun" // 불가능!
  tutor.bio = "senior" // 가능!
  
  System.out.println(student.setName('kwanghun'))
  System.out.println(student.getName) // kwanghun

  ```

- void = return 값 없는 function
- static = 클래스 자체를 객체화 필요 없이 사용

# Dependency 
- 의존성 = B가 바뀔때 A도 바뀔수 도 있으면 의존성이 있다고 표현

## Class 사이의 의존성
- 연관관계 ("Association") -> 객채 참조가 있는 경우
    - "A에서 B에 대해 알수 있다
    ```Java
    Class A {
        private B b;
    }
    ```
- 의존관계 (Dependency) -> 파라미터나 리턴에 타입이 있거나, 메소드에서 인스턴스를 생성하는 경우
    - 일시적으로 관계를 맺는 경우 (연관 관계에서는 A가 B에 대해 영구적으로 알 수 있지만, 의존 관계에서는 일시적으로 A가 B를 알고있다)
    ```Java
    Class A {
        public method(B b){
            return new B();
        }
    }
- 상속관계 (Inheritance) 
    - 상속을 받아 B의 클래스의 필드와 메소드를 참고함
    - 이때 B가 변경될 겨웅 A도 변경 될수 있음
    ```Java
    Class A extends B{
    }
    ```
- 실체화관계 (Realization)
    - 인터페이스를 Implement 하는 관계
    - 상소고간계와 비슷하지만, 상속은 클래스의 구현이 바뀌면 영향을 받음
    - 하지만 실체화 관계는 인터페이스의 시그니처가 바뀌었을 경우에 영향을 받음

## 패키지 사이의 의존성
- 패키지안에 포함된 클래스들 사이에 의존성을 뜻함
- 즉 클래스 파일에서 import하여 다른 클래스 파일에 의존을 할 수 있음

## 설계 시 의종성 관리 규칙들
1. 양방향 의존성을 피해야한다.
    - 두 클래스가 서로 의존하고 있으면, 한 클래스만 수정해도 다른 클래스까지 영향을 끼짐
    - 따라서 양방향을 지양하고 단반향 의존으로 바꿔줘야됨
    - e.g. 양방향 코드
    ```Java
    class A {
        private B b;

        public void setA(B b) {
            this.b = b;
            this.b.setA(this);
        }
    }
    
    class B {
        private A a;
        
        public void setA(A a) {
            this.a = a;
        }
    }
    ```
    - 이러한 코드를 아래와 같이 단반향으로 바꿔줘야됨
    ```Java
    class A {
        private B b;
        public void setA(B b) {
            this.a = b;
        }
    }
    class B {

    }
    ```
    - 이런 식으로 바꿔준다면 A 클래스의 set을 호출해도 B 클래스의 set 메소드를 호출 하지 않아도 됨

2. 다중성이 적은 방향으로 선택해라
    - 1:多 (One-to-Many) 보다 多:1 (Many-to-One) 방식으로 설계
    - e.g. One to Many
    ``` Java
    class A {
        private Collection<B> bs;
    //Collection 이나 Set 같은 것들을 Instance variable로 가지면
    // 다양한 이슈가 발생 할 수 있음 
    }
    class B {
    }
    ```
    - e.g. Many to One
    ``` Java
    class A {
    }
    class B {
        private A a;
    }

3. 의존성이 없다면 제거해야 된다
    - e.g. 의존성이 있는 경우
    ``` Java 
    class A {
        private B b;
    }
    class B {
    }
    ```
    - e.g. 의존성이 없는 경우
    ```Java
    class A {
    }
    class B{
    }
    ```
4. 페키지 사이에 의존성이 사이클 형태이면 제거 해라
    - 패키지 사이의 양방향 의존성 = 사이클
    - 패키지 A,B,C가 A <- B, B <- C, C <- A 같은 관계를 가질시 사이클