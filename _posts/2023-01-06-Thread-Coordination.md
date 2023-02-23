---
layout: post
title: "Java 멀티 스레딩 - 3. 스레드 조정"
categories: java
tags: [java, thread]
---

### *
스레드의 종료, 설정 등 제어하는 방법을 알아본다.

### 스레드 종료

#### 왜 종료해야 하는가?
- 리소스 확보\
스레드는 리소스를 사용한다. 아무것도 하지 않을 때에도 메모리와 일부 커널 리소스를 사용한다.\
스레드가 실행중이면 CPU 사이클과 CPU 캐시 공간을 잡아먹는다.\
따라서 스레드가 작업을 완료했는데도 애플리케이션이 작동 중이라면, 사용하지 않는 스레드가 사용하는 리소스를 확보할 수 있도록 정리할 필요가 있다.
- 스레드의 오작동\
응답이 없는 서버에 계속 요청을 보낸다던가, 예상보다 긴 계산을 전송하는 경우
- 애플리케이션 전체 중단\
최소 하나의 스레드만 실행되고 있어도 애플리케이션은 종료되지 않는다. (메인 스레드가 멈춘 경우에도)

#### 방법
- interrupt 메서드\
해당 스레드에 interrupt 신호를 보낸다.

인터럽트당한 스레드에서는 InterruptedException으로 인터럽트 예외를 처리하거나,\
Thread.currentThread().isInterrupted() 신호를 확인하여 명시적으로 처리해야 한다.

이를 처리하지 않은 스레드의 경우, interrupt 신호를 보내도 아무 영향이 없다.

- deamon 스레드\
해당 스레드를 백그라운드에서 실행되는 스레드로 처리한다.\
deamon 스레드는 스레드가 실행 중이어도, 애플리케이션의 종료를 막지 않는다.\
ex) 작업 툴에서 몇 분마다 작업을 자동 저장해주는 스레드
<br>

### 스레드 조정
우리가 신뢰하는 스레드가 예상 시간 안으로 작업을 완료하게 만드는 방법이다.

#### 왜 필요한가?
스레드는 독립적으로 실행되며, 실행 순서를 우리가 통제할 수 없다.\
예를 들어 A스레드가 B스레드에 의존하는 경우, A스레드에서는 B스레드의 완료 여부를 계속적으로 체크해야 하는 상황이 발생하는데\
체크하는 과정에서 CPU 사이클을 계속 낭비하게 되므로 비효율적이다.\
따라서 A스레드를 재우고, B스레드의 작업이 완료될 때 A스레드를 깨우면 된다.

#### 방법
- join 메서드\
해당 메서드에 다른 메서드들을 join하면, 다른 메서드들이 완료될 때까지 해당 메서드는 사이클에서 제외된다.
ex) main 메서드에서 다른 메서드들을 join시킨다 -> main 메서드는 다른 메서드들이 종료될 때까지 대기한다.

※ join(long millis)\
millis 시간동안만 다른 메서드들을 기다린다. 이 시간이 지나도 스레드가 종료되지 않으면 join 메서드가 그냥 반환된다.\
반환된 후에도 작업이 완료되지 않은 스레드는 여전히 작동하므로, 따로 관리(종료 처리 등)해주어야 한다.

프로그래머는 이런 방식으로 스레드의 작업을 잘 조정해야 한다. (방어적 프로그래밍 - 최악의 상황을 고려한 설계)