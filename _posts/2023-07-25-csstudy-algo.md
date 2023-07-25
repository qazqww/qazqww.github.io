---
layout: post
title: "CS 기초지식 - 01. 알고리즘"
categories: cs, algorithm
tags: [sort]
---

# *

알고리즘 문제는 꾸준히 잘 풀어나가고 있고, 코딩테스트의 합격률도 좋은 편이다. 하지만 면접에서 물어보는 기초 지식(자료 구조나 정렬)은 학부 시절 이후로 딱히 정리한 적이 없어서, 질문이 들어와서 술술 말할 자신은 없다. 그래서 다시금 짚고 넘어가고자 하였다.

## 거품 정렬 (Bubble Sort)

배경 지식 : 맨 앞부터 붙어있는 수들을 비교하여 가장 큰 수를 찾아나가는 과정을 반복한다. 거품이 밑바닥(앞)부터 커져가며 수면(끝)에 도달하는 과정과 비슷하여 이런 이름을 가졌다.

**정의**\
서로 인접한 두 원소를 비교하고, 조건에 맞도록 자리를 교환하며 정렬하는 알고리즘\
시간복잡도는 O(n^2), 공간복잡도는 O(n)

```java
static void bubbleSort(int[] arr) {
	for (int i = 0; i < arr.length; i++) {
		for (int j = 0; j < arr.length - i - 1; j++) {
			if (arr[j] > arr[j + 1]) {
				int temp = arr[j + 1];
				arr[j + 1] = arr[j];
				arr[j] = temp;
			}
		}
	}
}
```

**장단점**

| 장점 | 단점 |
| --- | --- |
| 추가적인 메모리 공간이 필요하지 않다. | 시간복잡도가 비효율적이다. |
| 안정 정렬이다. | 교환 연산이 많이 일어나게 된다. |

※ 안정 정렬 : 동일한 키를 가진 요소들의 순서가 정렬 후에도 유지됨

## 선택 정렬 (Selection Sort)

배경 지식 : 맨 앞부터 뒤의 수들과 차례로 비교하여 교환하는 과정을 반복하여 앞부터 채워나가는 정렬 방법?\
⇒ 틀림

**정의**\
원소를 넣을 위치를 선택하고, 그 자리에 오는 값을 선택하는 알고리즘\
시간복잡도는 O(n^2), 공간복잡도는 O(n)

```java
static void selectionSort(int[] arr) {
	for (int i = 0; i < arr.length; i++) {
		int min = Integer.MAX_VALUE;
		int index = -1;
		for (int j = i; j < arr.length; j++) {
			if (min > arr[j]) {
				min = arr[j];
				index = j;
			}
		}
		int temp = arr[i];
		arr[i] = arr[index];
		arr[index] = temp;
	}
}
```

**장단점**

| 장점 | 단점 |
| --- | --- |
| 추가적인 메모리 공간이 필요하지 않다. | 시간복잡도가 비효율적이다. |
| 교환 연산이 일어나는 횟수가 적다. | 불안정 정렬이다. |

## 삽입 정렬 (Insertion Sort)

배경 지식 : 새로운 배열을 생성하고, 정렬 대상 배열의 원소들을 차례로 삽입한다. 삽입 시 배열의 처음부터 훑으며 비교해가며 자신이 들어갈 위치를 찾아 삽입한다.\
⇒ 굳이 새로운 배열을 생성할 필요 없음

**정의**\
2번째 원소부터 시작하여, 그 앞의 원소들과 비교하여 삽입할 위치를 지정한 후, 원소들을 옮겨 자리를 확보하고 삽입하는 알고리즘\
시간복잡도는 O(n) ~ O(n^2), 공간복잡도는 O(n)

```java
static void insertionSort(int[] arr) {
	for (int i = 1; i < arr.length; i++) {
		int temp = arr[i];
		for (int j = i; j >= 0; j--) {
			if (j == 0) {
				arr[j] = temp;
				break;
			}
			if (temp > arr[j - 1]) {
				arr[j] = temp;
				break;
			}
			else {
				arr[j] = arr[j - 1];
			}
		}
	}
}
```

**장단점**

| 장점 | 단점 |
| --- | --- |
| 대부분 이미 정렬된 경우에 효과적이다. | 시간복잡도가 비효율적이다. |
| 다른 O(n^2) 알고리즘에 비해 빠르다. |  |
| 추가적인 메모리 공간이 필요하지 않다. |  |
| 안정 정렬이다. |  |