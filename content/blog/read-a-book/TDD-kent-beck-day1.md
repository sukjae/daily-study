---
title: Test Driven Development 1
date: "2019-09-23T00:00:00Z"
---

| 원제      | Test Driven Development: by example |
| --------- | ----------------------------------- |
| 제목      | 테스트 주도 개발                    |
| 저자      | Kent Beck                           |
| ISBN      | 9788966261024                       |
| 독서 기간 | 2019-09-23, 18:00-19:00             |
| 독서 량   | 1장 - 4장                           |

## 이해가 잘 안갔던 부분들

### 동치성, 동일성, 동질성 (p62,63)

- 세가지의 단어가 혼재되어 이해가 잘 안갔었음. 결국 모두 같은 의미로 사용되었다고 판단하였음

### 자바 문법을 잘 몰랐음 (p56)

```java
Dollar times(int multiplier){
  return new Dollar(amount * multiplier)
}
```

여기서 앞의 Dollar는 return값을 의미하고, 반환값으로 Dollar의 instance를 생성하여 전달한다.

### int amount = 5 \* 2 가 왜 중복인가? (p49)

책의 내용중에 아래와 같은 코드오 함께, 다음과 같은 문구가 나온다.

```java
Dollar
int amount = 5 * 2;
```

> 여기에서 10은 다른 어딘가에서 넘어온 값이다. ... 이제 5와 2가 두 곳에 존재한다. 따라서 우린 무자비하게 이 중복을 제거해야 한다.

여기서 이해가 안 갔던 부분은, 위의 5 \* 2가 어떤 부분에서 중복적으로 나오는지 였다.

지금까지의 짐작으로는 assert하는 부분에 존재하는 결과 값 10 또한 5\*2의 연산 결과임으로 중복이라 보는것 같다.

그렇다면, 이 부분은 어떻게 바꿔야 하는가...
아직 잘 모르겠다.

## 새로이 알게 된 내용들

### Stub (p43)

- 메소드의 input과 output만 적는 식으로 하여, 이 메소드가 호출되는 경우에 최소한 컴파일이 되도록 껍데기만 만들어 두는것

### 값 객체 패턴(value object pattern) (p59)

- 객체를 값처럼 사용한다.
- 객체의 인스턴스 변수가 생성자를 통해서 일단 설정된 후에는 결코 변하지 않도록 한다.
- 독립된 값을 갖음을 보장한다. (어찌보면, private과 같은 느낌, 그러나 값이 변할 수 없는 의미에서 const와도 비슷한 느낌)

### TDD에서의 삼각측량

- 책에서 이 부분에 대한 설명은 매우 부족했다. 뒷 부분에 다뤄질지 모르지만, TDD의 핵심 전략임에도 앞에서 다뤄지는 내용이 너무 적었다.
- 이 부분에 대해서 다른 블로그들을 검색하였고, 그 중에서 가장 그럴사한 내용을 정리하였다.

> Indirect measurement: Derive the design from few known examples of its desired external behavior by looking at what varies in these examples and making this variability into something more general

> Using at least two sources of information: start with the simplest possible implementation and make it more general only when you have two or more examples

[출처:feelings-erased](http://feelings-erased.blogspot.com/2013/03/the-two-main-techniques-in-test-driven.html#targetText=Kent%20describes%20triangulation%20as%20the,the%20position%20of%20a%20unit.)

위 내용들로 내가 받은 느낌은 다음과 같다.

테스트를 작성해야 하는데, 명확한(일반적인) 테스트 케이스와 구현해야 할 모델이 명확히 보이지 않을때 주로 사용하는것 같다.

모델의 잘 알려진 일반적인 결과들(예를 들어, 1+1은 2임을 많이들 안다)을 통하여 간접적으로 보다 일반적인 모델을 찾아가는 과정인것 같다.

또, 이를 위해서 하나의 큰 일반적인 사실만을 사용하는게 아니라, 가장 작은 단위의 정보들(일반적인 결과들)을 두개 이상 조합하여 일반적인 모델과 테스트 케이스를 만드는것을 목표로 하는듯 하다.

## 중요한 포인트

### TDD의 리듬 (p39)

1. 재빨리 테스트를 하나 추가한다.
2. 모든 테스트를 실행하고 새로 추가한 것이 실패하는지 확인한다.
3. 코드를 조금 바꾼다.
4. 모든 테스트를 실행하고 전부 성공하는지 확인한다.
5. 리펙토링을 통해 중복을 제거한다.

### 천재와 멍청이 사이에 존재하는 우리에게 필요한 단순한 법칙 (p37)

1. 어떤 코드건 작성하기 전에 실패하는 자동화된 테스트를 작성하라.
2. 중복을 제거하라.
