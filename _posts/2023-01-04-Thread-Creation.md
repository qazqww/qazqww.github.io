---
layout: post
title: "Java 멀티 스레딩 - 2. 스레드 생성"
categories: java
tags: [java, thread]
---

### *
스레드를 생성하는 방법과 기본적인 스레드 명령어를 알아본다.

### 스레드 생성
#### 1. 인스턴스 생성
```Java
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        // 새로운 스레드에서 실행될 코드
    }
})
```

Thread 객체로 새로운 인스턴스를 생성하는 간단한 방법이다.\
람다를 활용하여 더 간단하게 표현할 수도 있다.

#### 2. Thread 클래스 확장(상속)
```Java
class NewThread extends Thread {
    @Override
    public void run() {
        // 새로운 스레드에서 실행될 코드
    }
}
```

이 경우 this로 스레드 관련 데이터와 메서드에 접근할 수 있다.\
객체지향의 다형성을 활용할 수 있는 방법이다.
<br>

이 외에도 Runnable 인터페이스를 상속하여 Thread 생성 시 생성자의 매개변수로 넣어주는 방법이 있는데, 이 경우 확장이 더욱 용이해진다.
<br>

### 기본 명령어

thread.start()\
스레드를 실행한다. JVM이 새 스레드를 생성하여 운영체제에 전달한다.

thread.setName(String name)\
스레드에 직접 이름을 부여할 수 있다.

thread.setPriority(int priority)\
스레드의 우선 순위를 설정한다.\
Thread.MAX_PRIORITY 같은 상수로 관리해줄 수도 있다.

thread.setUncaughtExceptionHandler(UncaughtExceptionHandler u)

```Java
thread.setUncaughtExceptionHandler(new Thread.UncaughtExceptionHandler() {
    @Override
    public void uncaughtException(Thread t, Throwable e) {
        // 에러 처리 내용
    }
})
```
에러가 발생하면 전체 스레드가 중지될 수 있으므로, 전체 스레드에 해당하는 예외 핸들러를 지정한다.

Thread.sleep(long millis)\
현재 스레드를 설정해놓은 시간동안 스케줄링에서 제외한다.
<br>

### 기타
```Java
List<Thread> threads = new ArrayList<>();
for (Thread thread : threads) {
    thread.start();
}
```

위와 같이 리스트로 묶어서 스레드를 관리할 수 있다.