---
title: 디자인 패턴 - 싱글턴 패턴
date: 2025-02-26 21:15:00 +0900
categories: [Design Pattern, Create Pattern]
tags: [Design Pattern, Creational Pattern, Java]
---
## [Java] 싱글턴 패턴 (Singleton Pattern)
- - -
> 싱글턴 패턴(Singleton Pattern)을 따르는 클래스는, 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고
최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다.   
[위키피디아 - 싱글턴 패턴](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80%ED%84%B4_%ED%8C%A8%ED%84%B4)
   
엄근진의 대표인 위키피디아에 적힌 내용도 생각보다 어렵지 않다 ▶ 이해하기 쉬운 패턴입니다   
매우 짧게 요약하자면 **처음 만들어지는 인스턴스(Instance) 하나**만 쭉 쓴다는 것  
   
예시를 보는 것으로도 해당 패턴을 바로 이해할 수 있을 정도로 쉬우니까 바로 알아봅시다 

### Java 에서 구현   
```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 코드 분석   
1. 생성자가 ```private```로 작성되면서 외부에서 객체를 만들 수 없으니 ```getInstance()```로 꺼내서 사용
2. Singleton 객체는 ```private static Singleton instance;```를 통해 단 하나로 고정
3. ```getInstance()```가 처음 호출되면 인스턴스를 만들고, 그 이후로 호출되면 이미 만든 인스턴스를 반환
   
평소처럼 ```Singleton singleton = new Singleton();```을 쓰지 않는다!   

```java
public class Main {
    public static void main(String[] args) {

        Singleton instance1 = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance();

        System.out.println(instance1);
        System.out.println(instance2);
    }
}
// Output[1] : Singleton@7a79be86
// Output[2] : Singleton@7a79be86
```
   
- ```System.out.println();```으로 출력해보면 같은 객체를 나타낸다는걸 알 수 있다
   
이 부분이 이해가 안된다면 **객체**를 다시 공부해봅시다

### 장점
1. 메모리 아낀다   
```static```으로 만들어진 인스턴스 하나만 쓰니까   
2. 공유하기 쉽다   
이것도 ```static```이라서   
   
### 단점
멀티쓰레드에서 2개 이상 만들어진다던지, 너무 복잡하면 "개방-폐쇄 원칙"을 위배한다던지   
아무튼 **꼭 필요할 때 써야함** ◀ 이게 어려운 것 같습니다