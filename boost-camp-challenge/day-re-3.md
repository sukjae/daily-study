# [부스트캠프 멤버십 다시 3일차]

## 오늘의 고민들
### React에서 useEffect의 실행 흐름 이해하기

다음과 같은 컴포넌트는, 결국 몇번의 "work" 로그를 찍을까?

```js
import React, { useEffect, useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

function App() {
  const [a, b] = useState(0);

  console.log("work");

  useEffect(() => {
    b(1);
  });
  return <h1>{a}</h1>;
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

정답: 3번

그렇다면, 다음은?

```js
import React, { useEffect, useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

function App() {
  const [a, b] = useState(0);

  console.log("work");

  useEffect(() => {
    b(1);
  },[]);
  return <h1>{a}</h1>;
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

정답: 2번!

오늘 다른 부캠원분께서 위와 같은 코드에서 어떻게 작동할지 여쭈어 보았다. 

당시에는 나도 useEffect 두번째 인자의 default value 가 무엇인지 명확히 알지 못하여 선뜻 말하기 어려웠다. 

documentation에 따르면 다음과 같이 나와있다. 
> By default, effects run after every completed render, but you can choose to fire them only when certain values have changed.

출처: https://reactjs.org/docs/hooks-reference.html#useeffect

이에 대한 실제 구현 코드를 읽지는 못했지만, 매 render마다 App함수가 돈다는 것을 알 수 있다. 
물론, 위와같이 App함수를 임의로 실행시켜 어떤 로직을 처리하는것은 좋지 않다. 그러나, 이러한 질문들로 인해 useEffect에 대해 더 이해를 할 수 있는 좋은 기회였다. 

결국, useEffect의 두번째 인자가 없으면, `매 render가 종료`될 때 마다 useEffect가 실행된다. 

반면, useEffect의 두번째 인자로 빈 배열을 넣으면, `mount 와 unmount` 상황에서만 실행된다. 

**그러므로 맨 첫번째 코드를 해석하면 다음과 같을것 같다.**

1. App함수로 진입
2. useState 초기화 및 분해
3. **console.log `1회`**
4. useEffect callback등록
5. render시작 (이때 a === 0)
6. render 종료, mount 종료
7. render가 완료되었으므로 useEffect로 전달된 callback 실행! 
8. callback을 통해 state a의 값을 0에서 1 로 전환 
9. 전후로 state가 갱신 되었으므로, 다시 App으로 진입
10. 어떠한 방식으로 인해 useState를 sync 맞추고
11. **console.log `2회`**
12. useEffect callback등록
13. 전후 state가 변했으므로, render시작 (이때 a === 1)  
14. render 종료, update 종료
15. render가 완료되었으므로 useEffect로 전달된 callback 실행! 
16. callback을 통해 state a의 값을 1에서 1 로 전환 
17. 전후로 state가 갱신 되었으므로, 다시 App으로 진입
18. 어떠한 방식으로 인해 useState를 sync 맞추고
19. **console.log `3회`**
20. useEffect callback등록
21. 전후 state가 동일하므로, render 안함(shouldComponentUpdate() 영향)  
22. .... 대기


**반면, 두번째 코드를 해석하면 다음과 같을것 같다.**

1. App함수로 진입
2. useState 초기화 및 분해
3. **console.log `1회`**
4. useEffect callback등록
5. render시작 (이때 a === 0)
6. render 종료, mount 종료
7. mount가 완료되었으므로 useEffect로 전달된 callback 실행!
8. callback을 통해 state a의 값을 0에서 1 로 전환
9. 전후로 state가 갱신 되었으므로, 다시 App으로 진입
10. 어떠한 방식으로 인해 useState를 sync 맞추고
11. **console.log `2회`**
12. useEffect callback등록
13. 전후 state가 변했으므로, render시작 (이때 a === 1)
14. render 종료, update 종료
15. .... 대기


## 오늘의 회고

항상 나는 베스트 프렉틱스로 문제를 접근하고, 이러한 안티패턴처럼 생각되는 방식은 생각을 아예 하지 않으려 하였다. 그냥, 리엑트 스럽지 않다는 핑계로... 

그러나, 오늘 이 경험을 통해 새롭게 생각을 바꾸게 되었다. 베스트 프렉티스처럼 예상 가능한 상황이 아닌, 내 예상과 달리 움직이는 전혀 예상치 못한 상황을 생각하며 더 많이 배울 수 있는것 같다. 종종 이러한 이상한(?) 시도를 많이 해야겠다. 이러한 과정속에서 그 흐름을 더욱 명확하게 이해할 수 있을것 같다. 

오늘 또 들게 된 생각은, 부스트 캠프원들이 정말 많은 노력을 하고 계신다는 생각이 들었다. 개개인이 모두 하나에 몰두하여 굉장한 퀄리티의 성과를 내곤 한다. 이를 통하여 나 또한 자극을 받을 수 있었고, 더 열심히 해야겠다는 생각을 강하게 갖게 하였다. 

앞으로 남은 프로젝트 기간 주도적으로 참여하여 의미있는 결과물을 완성하고 싶다. 
