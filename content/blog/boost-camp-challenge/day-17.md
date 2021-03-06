---
title: 부스트캠프 2019 챌린지 17일차
date: "2019-08-06T00:00:00Z"
---

## 피어 세션 중 생각 정리

### [코드 구성에 대한 고민]

- type array 를 제외한 모든 type은 child:[] 를 갖지 않는다.
  - 이렇게 하는게 조금 더 자연스러운것 같다.
  - array는 child를 갖을 수 있지만, 나머지 자료형은 child를 갖을 수 없다.
- 재귀를 사용하지 않는다.
  - 가정에서 배열이 무한 중첩된 형태도 해석되야 하는데,
  - 재귀가 너무 많이 쌓이게 되면 메모리가 터질것 같았다.
  - 단순히 루프를 돈다.
- 현재 위치는 currentPosition 에 저장한다.
  - 이전까지 접근한 위치는 positions 에 stack 형태로 저장 된다.
  - 이를 통하여, 이전 위치로 가고 싶다면 stack 최상단 요소를 pop() 하여 currentPosition을 대체하면 된다.

### 코드 구조에 대한 고민

### [무한 중첩된 구조]

> 무한한 중첩이 기대되는 상황에서는 반복문을 통한 처리가 적합하다.

- 재귀의 장점
  - 코드가 간결해지고, 변수사용이 줄어드는 장점이 있다.
- 재귀의 단점
  - JS엔진을 구현한 브라우저 별로 Max Call Stack size가 존재한다.
  - 재귀를 사용하여 무한으로 중첩된 구조를 처리한다면, max call stack size를 넘어 애플리케이션이 뻗을 수 있다.

### [에러처리에 대한 흐름제어]

- `throw new Error()`와 `try-catch`를 활용하여 실제 사용자가 에러로 인해 프로그램 종료가 발생하는 상황을 방지 하였다.
- 여기서 중요한 것은, 각각의 기능은 `Error`를 방출하지만, 실제 사용하는 주체가 `try-catch`를 처리해 줘야 한다는 것이다.

## 컴파일러 구조와 나의 구현에 대한 고민

## 챌린지 중 느낀것

### [과제 해결 과정]

오늘도 코드를 작성하기 이전에 이론에 대한 학습을 하였다.

쓰레드와 프로세스 등 내가 잘 모르고 있던 부분에 대한 학습을 하고, 이를 어떻게 구현할지 전략을 세웠다.

미션 수행과 학습에 대한 밸런스를 맞추기 위해 오늘도 4시쯤 과제를 시작하였다.

오늘과 같은 복잡한 미션을 수행하기 앞서 그림을 그리고 생각의 흐름을 작성하는게 도움이 될듯 하여 다음과 같이 구조를 짜 봤다.

그렇게 복잡한 구조는 아니지만, 이를 통하여 내가 나눠야 할 부분들이 명확하게 보였고, 기능의 분리를 할 수 있었다.

기능이 분리되고 각각 맡은 바를 하니 구현도 용이했던것 같다.

### [처음 알게 된 부분 정리]

#### 브라우저에서의 멀티 쓰레딩??

노드에서 쓰레드풀을 4개 생성하여 비동기와 관련된 다양한 로직을 처리하는 것은 알고 있었다.

브라우저에서도 Web api를 통해 이와 비슷한 다중 쓰레드 기능이 제공될 것이라는 생각은 갖고 있었지만, 이 부분을 개발자가 작성할 수 있는지 몰랐다.

웹에서 자바스크립트로 멀티쓰레딩을 구현할 수 있다는 사실에 놀랐고, 자주 보이던 service worker도 이러한 기술을 활용했음을 알 수 있었다.

이렇게 멀티쓰레딩을 위하여 Web Workers API가 사용 되었는데, 메세지 기반으로 굉장히 손쉽게 사용할 수 있었다.

메세지를 주고 받는 부분에 대해서만 생각하고, 이에 대한 쓰레드를 생성하는 과정이 모두 Web Workers API 에서 제공되니, 놀라울 따름이였다.

오늘도 생각보다 일찍 과제가 끝나서 React로 이 부분을 어떻게 구현할 수 있을까에 대한 고민을 하였다.

일단 처음에는 나이브하게 js파일을 읽어 들어 window의 webworker를 활용하려는 계획을 하였지만, 역시나 실패하였다.

생각으로는 빌드 단계에서 JS파일이 합쳐지고, window에 대한 위치가 달라지기 때문이라 짐작 해봤다.

이를 위해 `worker-loader` 모듈을 설치해 주었고, babel을 활용하여 처리해 주니 React에서 깔끔하게 사용이 가능했다.

```
$ npm install worker-loader --save-dev
```

```js
// webpack.config.js
{
  module: {
    rules: [
      {
        test: /\.worker\.js$/,
        use: { loader: "worker-loader" },
      },
    ]
  }
}
```

```js
// App.js
import Worker from "./file.worker.js"

const worker = new Worker()

worker.postMessage({ a: 1 })
worker.onmessage = function(event) {}

worker.addEventListener("message", function(event) {})
```

출처: [https://webpack.js.org/loaders/worker-loader/](https://webpack.js.org/loaders/worker-loader/)

#### setInterval에 대한 착각

오늘 잘못 알고 있던 개념을 나중에 부캠원 부캠원분과 얘기를 나누며 바로 잡을 수 있었다.

내가 갖고 있던 막연한 생각은, `setInterval` 을 하는 동안 js가 실행되는 main thread를 점유할것이고, interval를 너무 짧게 치면 main thread를 너무 자주 점유해서 자원을 활용하기 어렵다고 착각하였는데, 생각해보니 이 부분은 어차피 Web API나 Node에서 비동기로 처리하기 때문에 문제 될게 없었다.

## 회고

역시, 고민을 조금 더 하고 개발을 하는게 바로 개발부터 하는것 보다 훨신 생산적이다.

구현에 대한 고민과 코드 작성에 대한 밸런스를 잘 맞추고, 목표하는 종료 시간에 끝내는 연습을 자주 하도록 하자.
