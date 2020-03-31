---
title: TS에서 Combining destructuring with parameter properties
date: "2020-03-31T00:00:00Z"
---

## 문제

현재 작성하고 있는 entities 의 클래스들은 아래와 같은 형태를 띄고 있다.

```js
interface HiData {
  my: string;
  name: string;
  is: string;
}
class Hi {
  private _my: string;
  private _name: string;
  private _is: string;
  constructor(hiData: HiData) {
    const { my, name, is } = hiData;
    this._my = my;
    this._name = name;
    this._is = is;
  }
  get my(){
    return this._my
  }
  get name(){
    return this._name
  }
  get is(){
    return this._is
  }
}
```

여기서, `my`, `name`, `is` 를 private으로 만들고, getter를 설정하기 위해 무수히 많은 코드가 중복된다.
너무 장황해져서, 이를 줄일 수 있는 방법을 찾고자 한다.
(결론을 말하자면, 아직 방법을 선택하지는 못했다...)

## 제약사항

1. 모든 properties는 private이 되어야 함
2. constructor 의 parameter는 객체의 형태여야 함

private을 고수하는 이유는, 외부에서 변경이 안되게 함이다.(아예 immutable 하게 readonly로 가져갈 고민도 하는 중)

object로 constructor에 넘기고자 하는 이유는 해당 클래스의 사용 편리성등이다. (인자의 순서를 딱 맞춰야 하는 불편함 등)

## 중복을 줄일 수 있는 부분

private 키워드를 생성하는 부분과 constructor에서 동일한(유사한) 이름에 할당하는 부분을 줄여볼 수 있을 것이다.
그에 대한 이유에는 typescript의 [parameter-properties](https://www.typescriptlang.org/docs/handbook/classes.html#parameter-properties)에 있다.

> Parameter properties are declared by prefixing a constructor parameter with an accessibility modifier or readonly, or both. Using private for a parameter property declares and initializes a private member; likewise, the same is done for public, protected, and readonly.

### parameter-properties 코드 샘플

```js
// before
class Octopus {
  private name:string;
  constructor(name: string) {
    this.name = name
  }
}

// after
class Octopus {
    constructor(private name: string) {}
}
```

중복된 코드가 상당히 줄어 들었음을 알 수 있다.

그러나 이 방법은 constructor 의 parameter로 destructuring 하였을때는 적용할 수 없다는 단점이 있다.
이에 대한 토론은 다음에서 확인 해 볼 수 있다. [TS Issue 5326](https://github.com/Microsoft/TypeScript/issues/5326)

즉, 위 방법으로는 나의 2번 제약 사항을 충족할 수 없다.

이에 대해 위 이슈에서 여러 아이디어들이 오고가는데, 그 중 많이 보이는 방법은 `Object.assign`을 사용하여 `this`와 연결하는 것이다.

### `Object.assign`을 사용하여 `this`와 연결하는 코드 샘플

```js
interface ExampleArgs {
  firstArg: string;
  otherArg: number;
}

export default class Example {
  constructor(kwargs: ExampleArgs) {
    return Object.assign(this, kwargs)
  }
}
export interface Example extends ExampleArgs {}
```

이것은 나의 제약사항 중 2번째를 충족하지만, 다시 1번의 제약사항을 지킬 수 없게 된다. (public이 됨)

## 결론

결국 마땅한 방법을 찾지 못했다.

나름 개인적으로 꼭 필요하다 생각한 두 조건이므로 그대로 유지하고, 중복 되더라도 일단은 코드를 장황하게 쓰기로 결정하였다. 이슈를 subscribe 해두어 기능이 추가되면 적용해야 겠다.

혹시 이에 대한 좋은 해결방법이나 제약사항에 오류가 보이면 댓글 부탁드립니다~
