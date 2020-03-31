---
title: 부스트캠프 2019 챌린지 1일차
date: "2019-07-15T00:00:00Z"
---

## 주어진 문제를 해결하며 적용한 라이브러리

- **Jest** - testing library
- **JsDoc** - markup language used to annotate JavaScript source code files
- **TypeScript** - It is a strict syntactical superset of JavaScript, and adds optional static typing to the language.
- **ESLint** - static code analysis tool for identifying problematic patterns found in JavaScript code

## 오늘 시도한 것

- TDD (잘 안됬음)
- Unit Testing
- Type Checking
- Airbnb linter
- 영어로 커밋 메시지 작성해 보기
- Refactoring

## 오늘 배운 것

### Jest 넣어보기

- jest 에서 typecheck는 함수를 인자로 받는다.
- 지금은 unit test만 작성 하였지만, 추후에는 다른 테스팅 기법도 시도해 봐야겠다.

[Error is thrown but Jest's `toThrow()` does not capture the error](https://stackoverflow.com/questions/47397208/error-is-thrown-but-jests-tothrow-does-not-capture-the-error)

### type checking 넣어보기 (type script & jsdoc)

- runtime에서 type을 체킹하는 것과 코드상에서 체킹하는것을 비교 고민해봐야 할듯 하다.
- 런타임에서 타입체킹을 강제하지 않기 때문에 따로 에러 헨들링 해줘야 하는것 같다.
- 예시 `if(typeof varFirst !== 'number'){throw new TypeError();}`
- 이렇게 발생한 에러는 Jest에서 다음과 같이 테스트할 수 있다.

```js
test("wrong type testing", () => {
  expect(() => {
    // @ts-ignore
    gcd(["a", "b"])
  }).toThrow(TypeError)
})
```

- 여기서 작은 팁은, 위 테스트가 에러를 가정한 테스트 임으로 VSCode에서는 type error로 계속해서 어필한다. 이를 방지하기 위해 `// @ts-ignore` 을 추가한다. (해당 라인의 코드 바로 위에 작성해야함)

### JSDoc 조금 더 자세히

- Technically, we are going to **use TypeScript but not for compiling** our code.
- Instead we’ll use it to **check** the types of our JavaScript code **during code time** using JSDocs comments and type inference.

즉, JSDoc을 사용함에 있어 TS를 활용하지만 컴파일 단계에서 파일을 변환(ts→js) 하는게 아니라 코딩중에 체크하는 방법이 된다.

**타입을 사용하면 다음과 같은 장점을 얻을 수 있다.**

1. Early detection of type errors
2. Better code analysis
3. Improved IDE support
4. Promotes dependable refactoring
5. Improves code readability
6. Provides useful IntelliSense while coding

또한 JSDoc은 일반적인 JS의 주석의 형태를 띈다는 특징을 갖고 있다.

이 특징은 타입 체크를 위하여 별도의 컴파일이나 변형이 필요치 않다는 것을 의미 한다. 최종 단계에서 minify등을 할시에 자동으로 주석을 날릴 수 있으므로 매력적이다.

typescript와 활용해서 만들 수 있는 큰 시너지중에 하나는 d.ts와 연동이 가능하다는 것이다.

별도의 커스텀 타입을 정의한 뒤 본 JS의 extension을 망가트리지 않는 선에서 활용이 가능하다.

아래 컨텐츠에 자세히 서술되어 있다.

[Type Safe JavaScript with JSDoc - TruckJS - Medium](https://medium.com/@trukrs/type-safe-javascript-with-jsdoc-7a2a63209b76)

### linter 넣어보기 (airbnb)

- 여러 Linter중에 가장 유명하고 기본이 되는 airbnb 스타일로 적용을 하였다.
- 처음에 하나하나 스타일링을 맞추다, 귀찮아서 save시에 autoFix옵션을 설정 하였다 .

  - 이는 VSCode 의 ESLint 패키지에서 설정 가능하며 아래 내용을 참조하자

  [ESLint - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### github 잘못 푸시 했을때

- git으로 원격 repository로 잘못된 branch로 push를 하는 실수를 했다.
- 이로 인해 fork된 나의 repository는 master로 오염되었고, 나에게 할당되어 있던 branch로 이미 작성된 commit message를 옮길 방법이 필요 했다.
- 결국에, commit message를 옮길 방법은 찾았지만, 굳이 master로 내 private forked repository에 올라간 commit을 지울 필요는 없었기에 그냥 놔두었다.
- 아래 답변을 활용하였다.

[Git push to wrong branch](https://stackoverflow.com/questions/6465699/git-push-to-wrong-branch)

###

## 회고

비교적 쉬운 난이도의 과제가 주어지니 코드 외적인 것을 고민할 시간이 많았다.

그로 인해 지금껏 적용해 보지 않았던 것들을 많이 시도해볼 수 있었다.

오늘 적용한 그대로를 쌓아가며 매일 매일 발전하도록 하자.
