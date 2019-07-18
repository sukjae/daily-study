# [부스트캠프 4일차]

## 챌린지 중 느낀것

주어진 기능을 완성 한 뒤 시간이 많이 남아 재밌는 도전을 해봤다. 

`극단적 코드 줄이기`

단, 코드의 가독성을 일부러 떨어트리는, 

예를 들어 그 스스로 적당한 이름이 있을 수 있는 count 변수를 임의의 한글자 x로 바꾸는 등의 편법은 안된다

순수히 한가지 목적을 위하여 두가지 수단만을 사용한다. 

1. 불필요한 변수 제거하기
    - 변수명을 극단적으로 줄이는 것은 안되지만, 변수를 제거하는것은 좋은 행동인듯 하다.
2. 불필요한 로직 제거하기
    - 변수를 줄이다보면 클로저등이 필요없어 하나의 함수로 가능 해지기도 한다.
    - 또한, 로직을 줄여 한가지 함수로 두, 세가지 함수가 하는 일을 할 수 있다면 더욱 좋은 코드가 될것 이다.

위 두가지 수단을 염두에 두고 코드 줄이기를 하였다. 

결과는 만족할만한 결과가 나왔다. 

### 변수 줄이기

특히, 클로저에 편의를 위해 저장했던 공통 변수들을 하나씩 제거하니 외부 함수가 전혀 필요 없어졌다. 

이로 인해 두개의 함수로 작성 되었던 코드를, 하나의 함수로도 동일한 기능을 제공할 수 있었다. 

### 로직 변화주기

루프를 돌리던것, 재귀를 돌리던것, reduce를 돌리던것 등 다양한 방법으로 같은 목적을 달성하는 고민을 하였다. 

그 중 가장 간단해지는 로직을 선택하여 구현하니 코드가 깔끔해 졌다.

### 재귀함수 클로져 날리기

> 어디서 찾아본게 아니라, 제 머릿속에서 하나씩 연구하며 내놓은 결과이다. 

어제까지 재귀함수를 사용할때, 외부 함수에 공통 변수를 둔 뒤, 

새로운 내부 함수를 만들고 return으로 재귀함수를 호출하는 식으로 작성 하였다 .

그러한 이유는 외부하수로 return을 하여 재귀를 호출할 방법이 마땅히 떠오르지 않았다. 

아래 코드로 생각해 보자 .

```js
    function outerFunc(){
    	const someVar = 1;
    	function innerFunc(){
    		// ... some logic
    		// .. some recursion logic
    	}
    return innerFunc(someInitialVal)
    }
```

어제까지는 위 코드와 같이 작성 하였는데, 크게 두가지 이유로 그리하였다.

1. 재귀가 도는 동안 공통의 참조가 될 대상인 `someVar` 가 필요하였다. 
2. 내부의 `innerFunc` 을 익명으로 작성하고 싶었지만, 그 경우 재귀를 돌릴 수 없었다. (호출할 대상이 없으니...)

그리하여 타협점을 찾아 `outerFunc` 를 호출하면, 자동으로 innerfunc가 실행되어 재귀 로직을 돌리도록 하였다. 

이 코드가 나쁜 코드는 아니지만, 위 기능에서 공통의 참조 대상이 `someVar`  가 굳이 필요 없다면, 재귀를 돌릴 다른 방법이 필요하다. 

그러한 경우에도 `outerFunc` 가 필요할까 라는 고민을 오늘 많이 하였다. 그 결과 나온 나의 결론은 다음과 같다 

```js
    const dec2bin = (A,B) => ((A < 1) ? B :dec2bin(A-1,[...B,...someMutation]));
```

코드를 보면 알겠지만, 더이상 `outerFunc` 가 필요하지 않다. 

탈출 조건을 재귀함수 내부 초반에 넣어야 한다는 고정관념에서 벗어나니, 

`return` 값에 탈출 조건을 넣는 괴상한 코드를 작성해 볼 수 있었다. 

즉, 위 코드는 재귀의 탈출 조건과 재귀 실행 요건등을 리턴에 모두 압축 시켜놓은 참 신기한 코드가 되었다. 

### 리듀서에서 `accumulator`로 복잡하게 값을 전달할 수 있지만,,, 그러지 말아보자.

코드 작성 중반에 변수들을 줄이기 위해 다음과 같은 로직을 작성 하였었다. 

일단, 이 코드를 작성하게 된 계기는 다음과 같다. 

- `someArrayA` 와 `someArrayB` 가 존재한다.
- `someFunc` 에 `someArrayA` 와 `someArrayB` 를 넣어 어떤 값을 반환 받는데, 이 값은 두가지 정보를 갖고 있다.
    - `someFunc`의 반환값은 객체 형태로서, {S:1, C:1} 이런식이다.
- 문제는 `someFunc`에 3번째 인자가 존재하는데, 이 인자는 reduce로 순회를 하며 직전에  `someFunc`에서 반환된 객체의 C값이다.
    - 즉, `i`순서에서  `someFunc` 연산의 결과로 `{S:1, C:1}`  객체가 나오는데, `i+1` 순서에서 `i` 때 연산된 C값을 3번째 인자로 넣는다.
- 정리하면,
    - 결과적으로 내가 연산의 끝에 필요한 것은 배열 한개 이다.
    - 하지만, `reduce`등으로 순회를 하며 팔로우(keep track)  해야하는 값은 두가지 이다 (현재까지의 배열(정답) && 직전 단계의 연산 결과(C))

위 가정을 해결하기 위해 초반에는 클로저를 통해 외부와 내부 함수 사이 공용변수를 두었다. 

이 변수의 값을 계속 접근하여 flag를 뒤집듯이 연산을 이어 나갔다. 

```js
    // 초반 코드
    
    const outerFunc ()=>{
    		// ... some logic
    		const res = []; // 외부의 어떤 값
    		let flag = false; // reduce가 순회하며 초기화 되지 않을 저장소
    		const result = someArrayA.reduce((acc, cur, i) => {
    		  const tmpResult = someFunc(cur, someArrayB[i], flag);
    			flag = tmpResult.C
    		  return [...acc.r, tmpResult.S],
    		    ;
    		}, [] );
    		return [...result, someVal]
    }
```

그러나, `outerFunc` 와 내부 저장 변수를 모두 제거하고 싶은 마음에 `flag` 를 일단 없애고 보니 아래 코드와 같은 모습이 나와버렸다.

```js
    // 중반 코드
    const outerFunc ()=>{
    		// ... some logic
    		const result = someArrayA.reduce((acc, cur, i) => {
    		  const tmpResult = someFunc(cur, someArrayB[i], acc.c);
    		  return {
    		    ...acc,
    		    r: [...acc.r, tmpResult.S],
    		    c: tmpResult.C,
    		  };
    		}, { r: [], c: false });
    		return [...result, someVal]
    }
```

`flag` 를 제거하는데에는 성공햇지만, 계속 따라가야할 변수를 잡을 방법을 생각하지 못하여 배보다 배꼽이 더 커진듯하다. 

그러나 아직 `outerFunc` 이 남아있었고, 로직도 너무 더러워서 고민을 많이 하였다. 

그 결과 나온 아이디어가, 다음과 같았다. (최종)

- 어차피 flag와 같이 하나의 값만 참조한다면, 이 값을 배열 끝에 이어 붙이는건 어떨까
- 예를 들어, (괄호 `{}` 가 제거해야 할 대상)

```js
    index = 0
    true false
    
    index = 1
    true {false} false true
    true false true
    
    index = 2
    true false {true} false true
    true false false true
```

즉, 이전 배열의 가장 끝에 존재 하였던 값 (위 코드에서 는 `tmpResult.C` , 즉 이전 `someFunc` 연산의 결과 중 `C` 값.) 을 제거하여 새로운 값을 이어 붙이는 전략이다. 

이 경우, `accumulator` 로 재 생성된 배열이 지속적으로 전달되고, 

우리는 루프마다 `accumulator` 에서 넘겨 받은 배열중에 정확히 어떤 부분에 미래에 참조할 새로운 C값이 있고, 어떤 부분에 과거에 참조한 C값이 있는지 알 수 있다. 

> 결과적으로, 새로 배열을 생성하기 전에 배열의 가장 마지막 엘리먼트가 C값, 즉 제거해야 할 대상이다. 

**이를 통해 최종적으로 나온 결과물은 이러하다.**

> `someFunc` 을 두개의 함수 `someFuncA` 와 `someFuncB` 로 분리 하였다. 

```js
    const innerFunc = (acc, cur, i) => [...acc.slice(0, i), someFuncA(cur, B[i], acc[i]),someFuncB(cur, B[i], acc[i])]
    const outerFunc = (A, B) => A.reduce(innerFunc, []);
```

결과적으로 상당히 깔끔한 코드가 작성되었다. 

위 코드를 한줄로도 작성할 수 있지만, 가독성을 위해 분리해 두었다. 

`reduce` 에 대한 `return` 값만 배열로 하나 존재하는데, 이 안에서 모든 로직이 진행된다. 

## 회고

어제에 이어 `reduce` 에 대한 새로운 관점을 발견하게 되어 기분이 좋다. 

종종 리팩토링을 하며 코드를 줄이기 위한 고민을 많이 해봐야 겠다.
