---
published: true
layout: post
title: "[java] 모던 자바 인 액션 - 2장 동작의 파라미터화 코드 전달하기 (1)"
category: java
tags: [java]
comments: true
classes: wide

---

#동작 파라미터화 코드 전달하기 (1)

### 동작 파라미터화 ? 

* 아직은 어떻게 실행할 것인지 결정하지 않은 코드 블록을 의미한다.



## 2.1 변화하는 요구사항에 대응하기

변화하는 요구사항에 유연하게 대응하는 방법을 예제를 통해 살펴보자.

다음은 농장 재고 목록 애플리케이션에 녹색 사과를 필터링 하는 기능을 추가한 것이다.

### 1. 첫 번째 요구사항 : 녹색 사과 필터링

```java
// 사과 색
enum Color { RED, GRREN}
```

```java
// 녹색 사과를 필터링 하는 메소드.
public static List<Apple> filterGreenApples(List<Apple> inventory) {
  List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory) {
    if (GREEN.equals(apple.getColor())) {
      result.add(apple);
    }
  }
  return result;
}
```

이와 같이 구현했는데 빨간 사과도 필터링 하고 싶어졌다. 위의 메서드를 복사해 조건만 RED로 바꿀 수 있지만 앞으로 더 요구사항이 다양해지면 적절하지 못한 코드라 할 수 있다. 이런 상황에서는 **거의 비슷한 코드가 반복 존재한다면 그 코드를 추상화한다.** 는 조건을 따르도록 하자.



### 2. 두 번째 : 색을 파라미터화 하기

색을 파라미터화 하는 메서드로 코드를 추상화 해보자.

```java
// 색으로 사과를 필터링 하는 메소드.
public static List<Apple> filterApplesByColor(List<Apple> inventory, Color color) {
  List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory) {
    if (apple.getColor().equals(color)) {
      result.add(apple);
    }
  }
  return result;
}
```

이제 색과 관련된 조건은 모두 필터링 할 수 있게 되었다.

그런데 여기에 무게로 사과를 구분 할 수 있게 해달라라는 요구사항이 들어온다면 어떻게 해야할까?

```java
// 무게로 사과를 필터링 하는 메소드.
public static List<Apple> filterApplesByWeight(List<Apple> inventory, int weight) {
  List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory) {
    if (apple.getWeight() > weight) {
      result.add(apple);
    }
  }
  return result;
}
```

다음과 같은 메소드를 추가하면 된다. 하지만 이 코드는 filterApplesByColor와 중복되는 부분이 많다. 이는 소프트웨어 공학의 __DRY(don't repeat yourself)__ 원칙을 어기는 것이다.



### 3. 세 번째 : 모든 속성으로 필터링

```java
public static List<Apple> filterApples(List<Apple> inventory, Color color, int weight, 	 	boolean flag) {
	List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory) {
    // 오.. 이건 아닌데..
    if ((flag && apple.getColor().equals(color)) || (!flag && apple.getWeight() > weight)){
      result.add(apple);
    }
  }
  return result;
}
```

정말 안좋은 예시다.. flag가 무얼 의미하는지 명확하지 않으며 앞으로 요구사항이 추가될 때도 유연하게 대처할 수 없다.

이런 경우 __동작 파라미터화__ 를 사용해 유연하게 대처할 수 있다. 



## 2.2 동작 파라미터화

우리는 사과를 구분할 때 사과의 어떤 속성에 기초해서 true/false 값을 결정한다. 이렇게 참 또는 거짓을 반환하는 함수를 **프레디케이트**라고 한다. 

일단 선택 조건을 결정하는 인터페이스를 정의하자.

```java
// ApplePredicate는 사과 선택 전략을 캡슐화했다.
public interface ApplePredicate {
	boolean test (Apple apple);
}
```

그리고 이를 구현하는 여러 ApplePredicate를 정의 할 수 있다.

```java
// 150이상의 무거운 사과 선택
public class AppleHeavyWeightPredicate implements ApplePredicate {
  public boolean test(Apple apple) {
    return apple.getWeight() > 150;
  }
}
// 초록색 사과 선택
public class AppleGreenColorPredicate implements ApplePredicate {
  public boolean test(Apple apple) {
    return apple.getColor().equals(GREEN);
  }
}
// 150 이상의 빨간색 사과 선택
public class AppleRedAndHeavyWeightPredicate implements ApplePredicate {
  public boolean test(Apple apple) {
    return apple.getWeight > 150 && apple.getColor().equals(RED);
  }
}
```

그리고 ApplePredicate를 인자로 받는 filter메서드를 만든다.

```java
// ApplePredicate를 인자로 받는다.
public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
	List<Apple> result = new ArrayList<>();
	for (Apple apple : inventory) {
    // 조건을 만족하는지 확인
    // 프레디케이트 객체로 사과 검사 조건을 캡슐화했다.
		if (p.test(apple)) {
			result.add(apple);
		}
	}
	return result;
}
```

위 조건에 따라 filter메서드가 다르게 동작하는데 이를 **전략 디자인 패턴 (strategy design pattern)** 이라고 한다.

전략 디자인 패턴은 각 알고리즘을 캡슐화하는 알고리즘 패밀리(ApplePredicate)를 정의해둔 다음에 런타임에 알고리즘을 선택 (전략 - AppleHeavyWeightPredicate, AppleGreenColorPredicate ...)하는 기법이다.

이렇게 동작 파라미터를 이용하면 한 개의 파라미터로 다양한 동작을 결정할 수 있다.





