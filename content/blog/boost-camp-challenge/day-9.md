---
title: 부스트캠프 2019 챌린지 9일차
date: "2019-07-25T00:00:00Z"
---

## 피어 세션 중 느낀것

### 셀 스크립트가 필요한 이유

- 다수의 반복되는 명령어를 하나의 스크립트 파일에 넣어 실행하여, 개발자의 수고를 덜어준다.
- 다수의 반복되는 명령어를 수기로 입력시, 실수로 인하여 명령어를 잘못 입력할 수 있는데, 이를 코드로 관리 한다면 실수를 방지할 수 있다.
- 스크립트로 사용가능한 다양한 언어들이 있는데 (예를 들어 파이썬, js 등), 이와 달리 셀 스크립트로 작성 시 셀에서 부가적인 도구 설치 없이 바로 사용이 가능하다는 장점이 있다.

### 타입스크립트가 필요한 이유

- 자바스크립트의 특성상 타입을 강하게 제한하지 않는다. 따라서 동적으로 타입이 변형되기도 하고, 이는 개발자가 원치 않은 결과로 이어지기도 한다.
- 사용될 수 있는 타입을 정의함으로서 원치 않은 결과를 방지하고, 통일된 규격을 맞춰 여러명이 함께 코딩을할 수 있다.
- 또한, 타입체크를 코드 작성시 실시간으로 진행하게 되면, 컴파일시 발생할 수 있는 문제들을 사전에 어느정도 방지가 가능하며 동시에 안내에 따라 타입에 대해 큰 신경 없이 코드를 작성할 수 있다.

## 챌린지 중 느낀것

오늘은 OOP에 대해 집중적으로 학습하였다.

OOP개념을 다시 돌아보기엔 부족한 시간이기에 구현하기 급급하였지만,

지난번에 FP로 작성하였던 코드를 다시 한번 OOP로 구현할 수 있는 좋은 기회였다.

특히 옵저버 패턴에 대해 간단하기 구현할 수 있었던 점이 굉장히 도움이 되었다.

또한, 지난번과 다른 자료구조를 택함으로서 로직이 훨씬 깔끔해졌다.

### Observable

아직 Observable 을 제대로 이해한것은 아니다, 그러나 하나의 인스턴스를 서로 공유하면서 함수를 호출할 수 있다는 점이 새롭게 다가왔다.

Observable 을 구현함에 있어서 subject와 observer를 기존의 코드와 최대한 분리하려 고민을 하였다.

클래스에 subjet클래스를 상속받아서 구현하고, observer를 리스너가 위치한 코드에서 생성한 뒤 리스너가 위치한 코드에서 subject의 메소드를 호출하여 구현하였다.

```js
// 이렇게 constructor 안에 메소드를 넣을 필요가 없었다.
const Subject = class {
  constructor() {
    this.observers = []
    this.subscribeObserver = observer => {
      this.observers.push(observer)
    }
    this.notifyAllObservers = () => {
      for (let i = 0; i < this.observers.length; i++) {
        this.observers[i].runFunc(i)
      }
    }
  }
}
// 옵져버 == 리스너
const Observer = class {
  constructor(func) {
    // 여기선, 하나의 함수만 연결할 수 있다.
    // 여기에 Observer에서 구현한 코드를 그대로 넣어도 똑같이 작동 했겠지만,
    // 코드 내에 다른 공간에서도 이와 같은 옵져버를 사용할 수 있도록 분리 하였다.
    // 즉, subject --- observer(==exe)를
    // subject --- observer --- exe 로 삼등분 한 느낌이다
    this.runFunc = func
  }
}

module.exports = {
  Subject,
  Observer,
}
```

```js
// Subject 부분, 옵저버가 주시해야 할 코드의 위치. 여기서 시그널을 발생시킴.
const { Subject } = require('./Observable');
function someFunc() {
  ...
  this.subscribeObserver(observer1);
  ...
  this.someAction(){
    ...
    this.notifyAllObservers();
  }
}

const subject = new Subject();
someFunc.prototype = Object.create(subject);
someFunc.prototype.constructor = someFunc;
```

```js
// Observer 부분. 시그널이 반영될 코드
const { Observer } = require('./Observable');
const someClass = class {
  constructor(someFunc) {
    this.someAction = async () => {
      ...
      // 옵저버가 시그널을 발생시키면 실행되어야 할 코드.
    };
    // 실행해야 할 코드를 옵져버에 넣어 인스턴스를 생성한다.
    // 이제 이 옵져버는 위 함수를 복사하여 갖고있다.(__proto__로 연결한건 아닌것 같다.)
    const observer = new Observer(this.someAction);

    // someFunc는 넘겨받은 인스턴스이다.
    // 위에서 someFunc에 prototype으로 Subject의 인스턴스인 subject를 연결 하였다.
    // 이를 통해서 우리는 someFunc.__proto__.subscribeObserver() 로 접근이 가능한것 같다.
    // 생각해보면, 객체를 인자로 넘길때는 레퍼렌스 참조와 같이 원래의 객체가 넘어오므로, 이 인스턴스
    // 또한 위해서 만들어진 인스턴스 그 자체인듯 하다.
    someFunc.subscribeObserver(observer);
  }
};
```

### Node에서 http.get 부분...

`http.get` 은 `axios` 나 `fetch` 처럼 친절하지 않다.

request를 보내고, response를 받을때 받은 response는 buffer의 형태로 흐르듯 날라온다.

어찌보면 http의 통신이 사실 그러하기 때문에 당연한 현상이지만, 직관적으로는 결과값은 최종값이여야 할것같다. 쉽게 사용하려면 `axios` 등을 사용하고, `http.get` 으로 간단하게 구현 하려면 다음과 같이 하면 된다.

```js
const options = {
  hostname: "localhost",
  port: 8090,
  path: "/api",
}
http
  .get(options, res => {
    let body = ""
    // 여기서 on data 이벤트가 발생하는 동안 계속해서 buffer로 날라온다.
    res.on("data", chunk => {
      body += chunk
    })
    // 여기서 on end 이벤트가 발생하는 순간,
    // 우리는 모든 response가 왔다 파악하고, 그 다음 액션을 취하면 된다.
    res.on("end", () => {
      // 이제 body에는 모든 buffer가 쌓여서 하나의 데이터 뭉치가 되었다.
      // 이를 바로 사용하면 된다.
      console.log(todolist)
    })
  })
  .on("error", e => {
    console.log(`Error: ${e.message}`)
  })
```

### 소통의 중요성

자바스크립트에 `Class` 는 그저 신택틱 슈거에 불가하고, 이를 기존의 함수로 구현할 수 있는것을 안다. 따라서 챌린지 미션에서 클래스에 대한 안내가 명확하지 않아 초반에 잘못된 구현을 하였다.

미션에는 다음과 같은 내용이 포함되어 있었다. `ES Classes 형태로 구현해야 한다.` , `ES6 Classes 패턴을 사용할 수 없으며` , `ES Classes문법으로 구현한다` , `ES Classes를 사용`

그냥 단순히 생각하면 클래스를 사용 하라는 것과 사용하지 말라는 두개로 보일 수 있다. 그런데 자세히 보면 위 내용은 다른 의미를 내포할 수 있다.

예를 들어, `Classess 형태로 구현` 이라는 부분은 `class` 가 제공하는 기능을 모방하여 구현 하라고 해석될 수 있다. 즉, `class 키워드` 를 사용하지 말고 `class 기능을 구현 하라` 로 볼 수 있다. `패턴` 도 같은 맥락에서 해석이 가능하다.

또한, `Classes 문법` 와 `Classes` 는 `class 키워드` 를 사용하라로 표현될 수 있다.

이렇듯, 단어 선택이 매우 중요함을 알 수 있다. 실제 협업을 진행할 때도 서로간의 오해가 없도록 자주 소통하는 습관을 가져야 겠다.

## 회고

### 잘못한 부분

집에 돌아와 잘못 생각한 부분들이 있었는데, 그중 가장 큰 부분은 contructer 에 method를 넣었다는 점이였다. 그냥 메소드로 구현하고, constructer안에서 this를 바인딩 해주면 될 부분을 거추장 스럽게 작성한것 같아 아쉽다.

### 만족스러운 부분

오늘 한것중에 만족스러운 부분은, 구현을 함에 있어 불필요한 부부을 많이 제외했다는 점이다.

예를 들어, 일전에는 JSON 파일을 입출력시 계속해서 덮어쓰었다. 이번에는 단순히 로컬 메모리에 올려서 프로그램을 수해하였다. 파일을 작성하는 코드가 메인 코드에서 사라지니 로직이 깔끔해졌다.
