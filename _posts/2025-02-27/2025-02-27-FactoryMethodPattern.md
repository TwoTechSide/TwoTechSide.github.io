---
title: 디자인 패턴 - 팩토리 메서드 패턴
date: 2025-02-27 17:50:00 +0900
categories: [Design Pattern, Creational Pattern]
tags: [Design Pattern, Creational Pattern, Java]
---
## [Java] 팩토리 메서드 패턴 (Factory Method Pattern)   
- - -
> 부모 클래스에서 인터페이스를 제공하고, 자식 클래스에서 구현하는 방식   
   
![Image](https://github.com/user-attachments/assets/fbf06d5f-c3fb-4ad9-830f-1263999fc4f2)   
빌더 패턴, 싱글턴 패턴, 그리고 3번째는 **팩토리 메서드 패턴(Factory Mathod Pattern)**입니다   
UML은 저렇다는데 조금 더 직관적으로 이해해봅시다   
   
### 예를 들며 이해해보자   
공장 하나 있는데 여기서 로봇을 만든다고 합니다   
```java
public interface Robot {
    void work();
}

public abstract class RobotFactory {

    public Robot newInstance() {
        return createRobot();
    }

    protected abstract Robot createRobot();
}
```
일하는 로봇이었군요   
   
```RobotFactory```클래스에서 ```newInstance()```로 로봇을 만들건데 어떤 로봇인지는 ```createRobot()```으로 결정됩니다   
그리고 하위 클래스에서 어떤 로봇을 만들건지 정해줄거구요   

```java
public class HouseRobot implements Robot {
    @Override
    public void work() {
        System.out.println("집안일 하는중");
    }
}

public class HouseRobotFactory extends RobotFactory {
    @Override
    protected Robot createRobot() {
        return new HouseRobot();
    }
}

// HouseRobot 인스턴스를 만드는 경우
HouseRobotFactory houseRobotFactory = new HouseRobotFactory();
Robot robot = houseRobotFactory.newInstance();
```
돈이 없어서 일단 집안일 해주는 로봇을 만들었습니다  
   
이번에는 새로운 사장이 오셔서 요리하는 로봇도 만들어 팔겠답니다   
```CookRobot```을 만들고 요리 로봇 공장도 하나 차려보죠   
   
```java
public class CookRobot implements Robot {
    @Override
    public void work() {
        System.out.println("라면 끓이는 중");
    }
}

public class CookRobotFactory extends RobotFactory {
    @Override
    protected Robot createRobot() {
        return new CookRobot();
    }
}
```
간단하네요   
   
## 장단점
새로운 사장이 ```CookRobot```이라는 로봇을 만들면서 기존 코드를 수정할 필요가 전혀 없었습니다   
이번에는 **개방-폐쇄 원칙**을 지킬 수 있는 장점이 생겼습니다   
   
그런데 코드가 꽤 늘어나면서 복잡해진다는게 단점입니다   
