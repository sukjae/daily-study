---
title: 부스트캠프 2019 챌린지 11일차
date: "2019-07-29T00:00:00Z"
---

## 오전 시간에 느낀점

오늘은 일찍 와서 책을 보았다. 앞부분만 보고 끝까지 읽지 않았던 자바스크립트 노랑이를 읽었다.

이 책을 이번주 안에 1/4를 읽기로 마음을 먹고, 차근차근 책을 읽으니 좋은 내용이 참 많았다.

### 3.7.1 매개변수의 이해

- ES에서 함수는 매개변수의 개수를 따지지 않고, 타입을 체크하지도 않는다.
- 매개변수는 내부적으로 배열로 표현된다
  - 배열로 표현되고, 배열처럼 동작하기는 하지만, `Array`의 인스턴스는 아니다
- 매개변수에 명시적인 이름을 정하지 않고도 접근이 가능하다.
  - 이를 모방하여 오버로딩을 구현할 수 있다.
- 매개변수와 `arguments`(접근자) 는 각각 다른 메모리 공간을 사용하지만, 값의 변화는 반영이 된다.
  - 그러나 이 반영은 단방향이여서 `arguments`를 변화시키면 해당 변수의 값도 변한다.
  - 그러나 반대는 변하지 않는다.
  - 또한 만약 매개변수를 넘기지 않았더라면, `arguments`를 변화준다고 하여도 해당 로컬 변수명을 통해 값에 접근할 수 있는것은 아니다.
  - 그 이유는, 함수 정의에서 정의한 매개변수의 길이를 따라서 `arguments` 배열을 생성하는게 아니라, 함수를 호출할 때 넘긴 이름 붙은 매개변수 목록을 따르기 때문이다.
  - 만약 ,정의한 매개변수를 넘기지 않으면 자동으로 `undefined` 가 할당되고, 변경 되지않는다.

### 3.7.2 오버로딩 없음

- 같은 이름을 갖은 함수를 여러번 정의하면, 마지막 함수가 해당 이름을 소유하게 된다.

### 값을 반환하지 않는 함수는 사실 undefined를 반환하는 것이다.

### 4.1.1 동적 프로퍼티

- 원시값 vs 참조값으로 나뉜다.
- 흥미롭게도 원시값에는 프로퍼티가 없지만(객체가 아니지만) 이를 추가하려 해도 에러가 생기진 않는다. 다만, 사용을 못할뿐
- 동적으로 프로퍼티를 추가할 수 있는 값은 참조값 뿐이다

### 4.1.2 값 복사

- 원시값은 현재 저장된 값을 새로 생성한 다음 새로운 변수에 복사한다.
- 즉, 서로의 값은 완전히 분리되어 있다.
- 참조값 또한 다른 변수로 복사하면, 원래 변수에 들어있던 값이 다른 변수로 복사된다.
- 차이는, 들어있던 값은 객체가 아니고 힙에 저장된 객체를 가리키는 포인터이며
- 이 포인터가 복사 되는것이다.

### 4.1.3 매개변수 전달

- 함수의 매개변수는 모두 `값` 으로 전달된다.
- 함수에 객체들을 매개변수로 전달할 때도, 이 객체를 가리키는 포인터가 복사되어 지역변수로서 작동하는 것이다.

### 4.1.4 타입 판별

- `typeof` 를 사용시에, string, number, boolean, undefined라면 정확한 타입을 알 수 있다.
- 하지만, null, object, array 등은 모두 `object`로 나온다.
  - 특히 null이 primitive value임에도 `object` 로 나오는 것에 주의하자.
  - 이것은 초기 ES에 이렇게 명세되었기 때문이다.
- 그러므로 해당 값이 객체인지 아닌지는 크게 중요치 않다.
- 이 값이 어떤 타입의 객체인지 알아야 하며 이는 `instanceof` 로 알 수 있다.

## 챌린지 중 느낀것

문제가 무언가 이상하긴 했지만, 충분히 있을법한 가정이라 고민을 많이 하였다.

일반적인 방법으로 접근 하였다면, 코드 자체에 변화를 주었겠지만, 오늘 미션의 경우 주어진 코드를 변화시키지 않고 문제를 해결해야 했기 때문에 고민을 많이 했다.

결론을 말하자면, 일반적인 방법으로는 해결이 안되는것 같다.

node를 통해서 process를 빼온뒤 따로 처리하던지

인위적으로 모두를 timeout안에서 async를 주던지

js를 파싱해서 앞뒤로 async를 주던지...

순수한 vanila js로는 도저히 안되는것 같다.

그래도 내가 혹시 모르는게 있어서 안보이는 것은 아닐까 하는 생각에 `Promise` 와 `Generator` 를 다시 한번 훑어볼 수 있는 기회가 되었다.

특히, `await` 를 `Promise` 를 반환하지 않는 값을 할당하였을때, resolved된 Promise가 반환된다는 사실이 충격적으로 다가왔다.

> 오늘 공부한 내용중 `Generator` 부분은 양이 많으므로 추후에 따로 정리하도록 하겠다.

위 난해한 조건을 제외하고는 상대적으로 평이한 미션이였다.

## 회고

안다고 생각하더라도 꼼꼼하게 다시 보자.

잘못 아는 것 만큼 위험한 건 없다.
