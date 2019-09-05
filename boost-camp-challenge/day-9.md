# [부스트캠프 멤버십 9일차]

## 오늘의 배움

Node & Express를 사용하면서, DB연결에 대한 고민을 하였다. 

우리의 경우에는 lowDB를 선택적으로 사용할 수 있었는데, 이에 대한 설정을 하는 부분에서 많이 배웠다. 

### 1. Express의 흐름에서 비동기적으로 무언가를 어떻게 기다릴까?

물론, Async/Await 이나 then 등을 사용하면 된다. 

하지만, 동기적으로 보이는 로직에서 비동기 로직을 어떻게 포함할지 고민이였다. 

이에 대한 해결법은, Middleware를 활용함으로서 해결할 수 있음을 알게 되었다. 

### 예시 코드

```js
var app = express();
const adapter = new FileAsync('db.json')

// 비동기적으로 수행하는 Middleware
app.use(async (req, res, next)=> {
  const db = await low(adapter)
  //...
})
// ... 
```

### 2. app등에서 설정한 객체등을 어떻게 전역적으로 활용할 수 있을까?

처음에는 scope와 closure를 활용하여, 변수를 선언하면 해결될줄 알았다. 

```js
someAsync()
.then(innerVal=>{

  // ...
  app.use(someMethod(innerVal))
})
```

물론, 이렇게도 작동은 하지만 require나 import로 불러온 경우 then안에 포함되어 있어도 `innerVal` 이라는 지역 변수를 모듈등이 공유하지 않는것같다 

방법을 찾던 중 req 에 녹여 전달하는 방법이 생각났다. 

```js
// 비동기적으로 수행하는 Middleware
app.use(async (req, res, next)=> {
  const db = await low(adapter)
  // ...
  req.db = db
  // ...
  next()
})
// ... 
```

이렇게 작성하면, 이후에 나오는 모든 middleware등에서 `req.db`로 접근이 가능해진다. 

하지만, 이렇게 전체적인 App과 밀접한 관계를 갖는 정보의 경우 `app.set` & `app.get` 을 활용함이 더 좋을것 같아서 다음과 같이 최종적으로 정리 하였다. 

```js
// 비동기적으로 수행하는 Middleware
app.use(async (req, res, next)=> {
  const db = await low(adapter)
  // ...
  app.set('db',db)
  next()
})
```

위와 같이 설정한다면, 이후에 나온 모든 middleware에서 `app.get` 으로 접근이 가능하다. 

또한 app이 선언되지 않았더라도 app은 기본적으로 `req` 에 포함되어 있으므로, `req.app.get` 접근이 가능하다. 

## 오늘의 고민들

### Server 측의 테스트에 대한 고민

이전에 기능단위의 테스트를 작성하는 것은 익숙해 졌다. 

하지만, 서버측에서는 이러한 기능단위의 유닛 테스트를 포함하여 통합테스트 등을 더 고민해야 하고,

그 순서가 명확하게 보이지 않아서 고민을 많이 하고 있다. 

FE에서 테스트를 작성하지 못하여 반성 했던 만큼, 이번에는 꼭 테스트를 통한 TDD로 접근을 할것이다. 

### 앞으로의 테스트에 대한 고민

테스트는 한글로 작성해보자 .
  - 어차피 빠른 체크가 목적이므로!

큰거에서 작은걸로 가는게 좋을까? 작은거에서 큰것으로 가는게 좋을까?
- 예를 들어, API를 테스트하는 코드를 작성한 뒤 개별적인 세부 기능에 대해 좁혀가는게 좋을까
- 아니면 반대가 좋을까

기능 명세서와 TDD의 관계는 어떻게 될까
- 스크럼에서 작성하게 될 유저 스토리와 세부 스프린트 작업 사이의 관계는 어떻게 될까
- 지금까지의 느낌으로는, 스크럼을 통해 거시적인 (기술적으로는 조금은 두루뭉실한) 명세를 작성하고,
- 이를 개별 기능 명세서를 통해 구체화 하는 방향으로 고민중이다.

## 추가로 학습해야 할 키워드

- json RPC
- 1차원 데이터 ( B-tree), 2차원 데이터 ( R-tree & R*tree)
- eventEmitter 에서 on 도 있지만, once 도 있다.
    - once 의 경우에 한번 event를 받으면, 리스너가 자동으로 제거된다.
- winston 모듈, 로그를 남기고 & 모으는 방법에 대해 알아보자
- SuperTest를 이용한 API  Testing