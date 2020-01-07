---
published: true
layout: post
title: "[java] 모던 자바 인 액션 - 2장 동작의 파라미터화 코드 전달하기 (2)"
category: java
tags: [java]
comments: true
classes: wide

---

## 동작 파라미터화 코드 전달하기 (2)
-------------------
## 2.3 복잡한 과정 간소화

### 익명클래스

```java
List<Apple> redApples = filterApples(inventory, new ApplePredicate() {	// 익명 클래스 구현
    public boolean test(Apple apple) {
      return RED.equals(apple.getColor());
    }
})
```

익명클래스도 문제점이 있다. 여전히 많은 공간을 차지하며 많은 프로그래머들이 익명 클래스 사용에 익숙하지 않다.



### 람다 표현식 

```java
List<Apple> result = filterApples(inventory, (Apple apple) -> RED.equals(apple.getColor()));
```

위의 익명클래스보다 훨씬 간결한 코드가 된다.



### 리스트 형식으로 추상화

```java
public static <T> List<T> filter(List<T> list, Predicate<T> p) { // 형식 파라미터 T
    List<T> result = new ArrayList<>();
    for (T e : list) {
        if (p.test(e)) {
            result.add(e);
        }
    }
	return result;
}
```



```java
// 다양한 타입의 리스트에 필터 메서드를 적용할 수 있다.
List<Apple> redApples = filter(inventory, (Apple apple) -> RED.equals(apple.getColor()));
List<Integer> evenNumbers = filter(numbers, (Integer i) -> i % 2 == 0);
```



## 2.4 실전 예제

### Comparator로 정렬하기 

Comparator를 구현해 sort메서드의 동작을 다양화할 수 있다.

```java
// java.util.Comparator
public interface Comparator<T> {
		int compare(T o1, T o2);
}

// 사과를 무게로 정렬하기
inventory.sort(Appple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
```



### Runnable로 코드 블록 실행하기

Runnable을 이용해서 다양한 동작을 스레드로 실행할 수 있다.

```java
 // java.lang.Runnalble
 public interface Runnable {
 		void run();
 }

Thread t1 = new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello world!");
    }
});

Thread t2 = new Thread(() -> System.out.println("Hello world!"));
```



