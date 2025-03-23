---
title: 디자인 패턴 - 옵저버 패턴
date: 2025-03-03 08:00:00 +0900
categories: [Design Pattern, Behavioral Pattern]
tags: [Design Pattern, Behavioral Pattern, Java]
---
## [Java] 옵저버 패턴 (Observer Pattern)   

- - -

> 객체 상태가 바뀌면 연관된 객체에게 알림을 보내는 패턴
   
옵저버는 스타크래프트 프로토스 유닛 이름 아닌가요   
그것도 맞긴 한데 지금은 스타가 아니라 디자인 패턴 배울거니까 혼동하지 마시구요   
   
누가 유튜브에 영상을 올리면 그 채널의 구독자에게 알림이 가는 것 처럼   
연관된 객체에 알림을 보내는 패턴이 **옵저버 패턴**입니다   
   
방금 말한걸 예시로 한 번 만들어 볼까요   

- - -
   
## Java 에서 구현

제 유튜브 채널에 영상을 올리면 구독자에게 알림이 가도록 해봅시다   

```java
public interface Channel {
    void addSubscriber(Subscriber subscriber);
    void removeSubscriber(Subscriber subscriber);
    void notifyVideo(String msg);
}

public interface Subscriber {
    void update(String msg);
}
```
구독자가 추가되거나 사라지는 `addSubscriber`, `removeSubscriber`을 만들구요   
`notifyVideo(String msg)`로 구독자에게 알림을 보낼겁니다   

```java
import java.util.ArrayList;
import java.util.List;

public class MyChannel implements Channel {

    List<Subscriber> subscribers = new ArrayList<>();

    public void uploadVideo() {
        System.out.println("[update] 새로운 동영상을 올렸음");
        notifyVideo("구독한 채널에 새 영상이 올라옴");
    }

    @Override
    public void addSubscriber(Subscriber subscriber) {
        subscribers.add(subscriber);
    }

    @Override
    public void removeSubscriber(Subscriber subscriber) {
        subscribers.remove(subscriber);
    }

    @Override
    public void notifyVideo(String msg) {
        subscribers.forEach(subscriber -> subscriber.update(msg));
    }
}
```
`subscribers` 리스트에 제 구독자들을 추가할거고   
영상을 올리면 구독자들에게 새 영상이 올라왔다는 알림을 보낼겁니다   

```java
public class User implements Subscriber {

    String name;

    public User(String name) {
        this.name = name;
    }

    @Override
    public void update(String msg) {
        System.out.println(name + " 유저 수신 : " + msg);
    }
}
```
지금은 `update`에 딱히 넣을 기능은 없으니 그냥 알림 받은대로 출력만 해봅시다   

```java
public class YouTube {
    public static void main(String[] args) {
        MyChannel myChannel = new MyChannel();
        User user1 = new User("kim");
        User user2 = new User("lee");

        myChannel.addSubscriber(user1);
        myChannel.addSubscriber(user2);
        myChannel.uploadVideo();

        myChannel.removeSubscriber(user1);
        myChannel.uploadVideo();
    }
}
/*
[ 출력 결과 ]
[update] 새로운 동영상을 올렸음
kim 유저 수신 : 구독한 채널에 새 영상이 올라옴
lee 유저 수신 : 구독한 채널에 새 영상이 올라옴
[update] 새로운 동영상을 올렸음
lee 유저 수신 : 구독한 채널에 새 영상이 올라옴
*/
```
간단하죠      

- - -
## Observer, Observable
자바에 내장된 옵저버 클래스로 만들어도 될 것 같습니다   
   
![Image](https://github.com/user-attachments/assets/e583fa39-9c4a-4daa-9844-51bb4864e18a)   
![Image](https://github.com/user-attachments/assets/c33c1dcf-df87-470d-95d8-f4572f8483e1)   
들어있는거 보면 아까 만든 예시랑 비슷하죠   

`addObserver`를 보면 `synchronized`가 포함되어 있어서 `Thread-Safe` 인 상태입니다   
> Thread-Safe 상태라는 것은 자바의 쓰레드를 더 공부하고 이해해야 될 것 같은데요   
아쉽게도 글 쓰는 시점에서 제가 아직 이해를 제대로 하지 못한 것 같습니다   

근데 `Observable`은 클래스고 자바에서는 단일 상속만 됩니다   
이미 다른 클래스를 상속중이라면 못쓰겠죠, 게다가 `setChange()`는 `protected`로 되어있구요   
   
이미 상속중인 클래스에 옵저버 패턴을 쓰려면 개발자가 직접 구현해서 써야됩니다   
다행히 위의 예시로 배워봤으니 충분히 직접 구현해서 쓸 수 있지 않을까요   