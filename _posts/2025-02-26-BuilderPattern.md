---
title: 디자인 패턴 - 빌더 패턴
date: 2025-02-26 22:00:00 +0900
categories: [Design Pattern, Create Pattern]
tags: [Design Pattern, Creational Pattern, Java]
---
## [Java] 빌더 패턴 (Builder Pattern)
- - -
> 복잡한 객체를 단계별로 만들 수 있는 생성 패턴(Creational Pattern)   

**생성자**에 익숙하다면 어렵지 않은 내용일겁니다   
잘 모른다면 생성자, 생성자 오버로딩을 직접 써보고 오면 될 것 같습니다   

### 원래 객체를 만들 때   
예를 들어, [ 나이, 이름, 주소 ]같은 개인 정보를 담는 ```User```객체가 있으면   
생성자는 아마 이렇게 써야합니다    
```java
public User(String name, int age, String address) {
    this.name = name;
    this.age = age;
    this.address = address;
}
```
주소가 안정해진 상태로 해당 객체를 만드려면 생성자 오버로딩(Overloading)을 해야될겁니다   
   
근데 만약 이름을 모를 수도 있고, 나이를 모를 수도 있고, 주소를 모를 수도 있다면요   
생성자를 무자비하게 늘리던 아니면 객체 만들 때 ```new User("홍길동", null, "주소")```같은걸 써야됩니다   
- ```new User("홍길동", null, null)```
- ```new User("null, 20, "어딘가")```
- ```new User("홍길동", null, "어딘가")```   
   
생성자를 늘리자니 복잡하고, 객체를 만들 때 어디에 무슨 값을 넣는건지도 모르겠고   
아무튼 이렇게만 봐도 단점이 딱 보입니다   
   
이럴 때 빌더 패턴을 쓰면 보기 좋습니다   
- - -
### 빌더 패턴으로 객체 만들기

```java
public class User {

    private String name;
    private int age;
    private String email;

    public User(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }

    // Builder 클래스
    public static class Builder {
        private String name;
        private int age;
        private String email;

        public Builder name(String name) {
            this.name = name;
            return this;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public User build() {
            return new User(name, age, email);
        }
    }
}
```
   
> 근데 왜 private인가요 ▶ Getter, Setter을 알아봅시다   

기존의 생성자는 빠지고 ```User```클래스 안에 ```Builder```라는 static 클래스가 생겼습니다   
이걸로 제 정보를 담은 ```User``` 객체를 하나 만들어봅시다   

```java
User user = new User.Builder()
        .name("2TS")
        .age(100)
        .email("TwoTechSide@gmail.com")
        .build();
```

```new User("2TS", 100, "TwoTechSide@gmail.com");```에서 위 코드처럼 바뀌었네요   
넣고 싶은 값 넣고 마지막으로 ```build()```까지 해주면 User 객체가 완성됩니다   
   
- - -

### 장점
어떤 값을 넣는건지 알아보기 쉽습니다   
그리고 null이나 기본값 때문에 피곤하게 코드를 수정할 필요도 없습니다   

### 단점   
필수로 넣어야 하는 변수가 있어도 null이 허용되어버립니다   
빌더 객체까지 추가로 만들어야 합니다   

- - -

## 스프링에서 빌더 패턴을 쓰려면

```java
@Builder
@Getter
public class User {
    private String name;
    private int age;
    private String email;
}
```
정말 쉽죠   
보통 ```@Getter```과 함께 사용됩니다   
   
당연히 Lombok이 있어야 하니까 ```build.gradle > dependencies```에 이거 추가해줍시다   
```
annotationProcessor 'org.projectlombok:lombok'
```
