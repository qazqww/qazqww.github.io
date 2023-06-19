---
layout: post
title: "Java Stream API"
categories: java
tags: [java]
---

## *

Java 8에서 추가된 기능인 Stream API에 대해 정리해보았다.\
원래는 Stream에 대해 잘 몰랐는데, 프로젝트 진행 중 팀원이 사용하는 걸 보고 새롭고 편해보여서 따라 사용해본 적이 있었다. 하지만 제대로 익히고 쓴 건 아니라 내용을 한 번 정리해볼 필요성을 느꼈다.

## 자바 스트림

**기존의 반복문은?**\
길이가 길고 가독성이 떨어지며 코드의 재사용이 불가능하다.

**스트림 API**\
데이터를 추상화하여 다루기 때문에, 다양한 방식의 데이터를 공통된 방법으로 다룰 수 있다.

## 스트림 API의 특징

**1.** 내부 반복을 통해 작업 수행
- 내부 반복이란?\
개발자가 직접 반복문을 작성하지 않고, API가 내부적으로 요소를 반복하면서 처리

**2.** 재사용이 가능한 컬렉션과는 달리 한 번만 사용 가능하며, 원본 데이터는 변경되지 않는다.

**3.** 필터-맵 기반의 연산 API를 사용하여 지연(lazy) 연산으로 성능을 최적화\
- 필터-맵 기반 API란?\
스트림의 중간 연산은 필터링과 매핑에 기반하여 수행\
즉, 특정 조건에 해당하는 요소를 걸러내고, 요소를 다른 형태로 변환한다.\
(밑에 흐름 항목의 그림 참조)

- 지연 연산이란?\
연산 결과가 필요한 시점에만 실제 연산을 수행하는 방식

**4.** parallelStream() 메소드를 통해 손쉬운 병렬 처리 지원

## 흐름

스트림 생성 ⇒ 스트림 중개 연산 (변환) ⇒ 스트림 최종 연산 (사용)

![Untitled](https://www.tcpschool.com/lectures/img_java_stream_operation_principle.png)

## 사용

**컬렉션**

모든 컬렉션의 최상위 클래스인 Collection 인터페이스에 stream() 메소드가 정의되어 있다.

```java
ArrayList<Integer> list = new ArrayList<>();
Stream<Integer> stream = list.stream();
// Stream<Integer> parallelStream = list.parallelStream(); // 병렬 처리 가능한 스트림
stream.forEach(System.out::println);
```

**배열**

Arrays 클래스에서 다양한 형태의 stream() 메소드가 클래스 메소드로 정의되어 있다.

```java
String[] arr = new String[5];
Stream<String> stream = Arrays.stream(arr);
// Stream<String> stream = Arrays.stream(arr, 1, 3); // 배열의 일부분으로 생성
// Stream<String> stream = Arrays.stream(arr).parallel(); // 병렬 처리 가능한 스트림
stream.forEach(e -> System.out.print(e + " "));
```

**람다 표현식**

람다 표현식에 의해 반환되는 값을 스트림으로 생성하기 위해 Stream 클래스에는 iterate()와 generate() 메소드가 정의되어 있다.\
iterate 메소드는 시드값(첫 번째 매개변수)을 람다 표현식(두 번째 매개변수)에 사용하여 반환된 값을 다시 시드로 사용하는 방식으로 무한 스트림을 생성한다.\
generate 메소드는 매개변수가 없는 람다 표현식을 사용하여 반환된 값으로 무한 스트림을 생성한다.

```java
Stream stream = Stream.iterate(2, n -> n + 2); // 2, 4, 6, 8, ...
```

**파일**

파일의 한 행을 요소로 하는 스트림을 생성하기 위해 java.nio.file.Files 클래스에 lines() 메소드가 정의되어 있다.\
java.io.BufferedReader 클래스의 lines() 메소드는 파일뿐만 아니라 다른 입력도 데이터를 행 단위로 읽어올 수 있다.

```java
Stream stream = Files.lines(Path path);
```

**가변 매개변수**

```java
Stream stream = Stream.of(4.2, 2,5, 3.1, 1.9);
```

**연속된 정수**

```java
IntStream stream = IntStream.range(1, 4);
IntStream streamClosed = IntStream.rangeClosed(1, 4); // 4까지 포함
```

**빈 스트림**

```java
Stream stream = Stream.empty();
```
<br>

## 중개 연산

스트림을 전달받아 스트림을 반환한다.\
⇒ 연속으로 연결해서 사용할 수 있다.

| 용도 | 관련 메소드 |
| :----: |:------: |
| 필터링 | filter(), distinct() |
| 변환 | map(), flatMap() |
| 제한 | limit(), skip() |
| 정렬 | sorted() |
| 연산 결과 확인 | peek() |

**필터링**

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 4, 3);

// filter() : 조건에 맞는 요소만 걸러냄
stream.filter(e -> e > 3).forEach(System.out::println);

// distinct() : 중복 제거
stream.distinct().forEach(System.out::println);
```

**변환**

```java
Stream<String> stream = Stream.of("ABC", "JAVA", "SCRIPT", "STREAM");
stream.map(e -> e.toLowerCase()).forEach(System.out::println);

// flapMap() : 중첩된 스트림의 각 요소를 합쳐 하나의 스트림으로 변환
stream.flatMapToInt(e -> e.chars()).forEach(System.out::println);
```

**제한**

```java
IntStream stream = IntStream.range(0, 10);

// limit() : 해당 개수만큼의 요소만으로 이루어진 스트림 반환
stream.limit(6).forEach(System.out::println);

// skip() : 해당 개수만큼의 요소를 제외한 스트림 반환
stream.skip(2).forEach(System.out::println);
```

**정렬**

```java
Stream<String> stream = Stream.of("SCRIPT", "STREAM", "ABC", "JAVA");
stream.sorted().forEach(System.out::println);
stream.sorted(Comparator.reverseOrder()).forEach(System.out::println);
```

**연산 결과 확인**

주로 디버깅 목적으로 사용한다.

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
stream.peek(System.out::println)
				.map(n -> n * n)
				.forEach(System.out::println);
```
<br>

## 최종 연산

각 요소를 소모하여 결과를 표시한다.\
최종 연산을 마친 스트림은 더 이상 사용할 수 없다.


| 용도 | 관련 메소드 |
| :-----: | :------: |
| 출력 | forEach() |
| 소모 | reduce() |
| 검색 | findFirst(), findAny() |
| 검사 | anyMatch(), allMatch(), noneMatch() |
| 통계 | count(), min(), max() |
| 연산 | sum(), average() |
| 수집 | collect() |

**출력**

```java
Stream<String> stream = Stream.of("Java", "C++", "Python", "Swift");
stream.forEach(System.out::println);
```

**소모**

스트림의 각 요소를 하나의 값으로 만드는 연산 수행\
첫 번째 요소와 두 번째 요소를 연산한 뒤, 그 결과값을 세 번째 요소와 연산. 이를 반복한다.

```java
Stream<String> stream = Stream.of("Java", "C++", "Python", "Swift");
Optional<String> str = stream.reduce((a, b) -> a + " / " + b);
str.ifPresent(System.out::println);

// 결과값 : Java / C++ / Python / Swift
```

**검색**

```java
Stream<String> stream = Stream.of("Java", "C++", "Python", "Swift");

// findFirst() : 스트림의 첫 번째 요소 반환
Optional<String> str = stream.sorted().findFirst();

// findAny() : 스트림의 임의의 요소 반환
Optional<String> str = stream.sorted().findAny();
str.ifPresent(System.out::println);
```

**검사**

```java
IntStream stream = IntStream.range(0, 10);

// anyMatch() : 일부 요소가 조건을 만족할 경우 true
System.out.println(stream.anyMatch(e -> e == 5)); // true

// allMatch() : 모든 요소가 조건을 만족할 경우 true
System.out.println(stream.allMatch(e -> e == 5)); // false

// noneMatch() : 모든 요소가 조건을 만족하지 않을 경우 true
System.out.println(stream.noneMatch(e -> e > 10)); // true
```

**통계**

```java
IntStream stream = IntStream.of(100, 500, 300, 900);
System.out.println(stream.count()); // 4
System.out.println(stream.max().getAsInt()); // 900
System.out.println(stream.min().getAsInt()); // 100
```

**연산**

```java
IntStream stream = IntStream.of(100, 500, 300, 900);
System.out.println(stream.sum()); // 1800
System.out.println(stream.average().getAsDouble()); // 450.0
```

**수집**

스트림의 요소를 수집하여 컬렉션(또는 다른 형태)으로 변환한다.
<br><br>

## Collectors의 메소드 예시

**1.** 스트림을 배열이나 컬렉션으로 변환 : toArray(), toList(), toSet(), toMap(), toCollection() 등\
**2.** 통계나 연산 등 수행 : counting(), maxBy(), minBy(), summingInt(), averagingInt() 등\
**3.** 소모 등 수행 : reducing(), joining()\
**4.** 그룹화와 분할 : groupingBy(), partitioningBy()