# [부스트캠프 멤버십 다시 2일차]

## 오늘의 고민들
### React에서 hooks가 어떻게 HOC를 대체하는가

React에서는 보통 cross-cutting concerns(공통의 관심사)를 해결하기 위해서 사용되었다. 

Component끼리 공통의 로직 혹은 값을 공유하기 위해 사용되었고, 이러한 정보를 사용하고자 하는 컴포넌트에 주입하는 형태로 사용되었다. 반면 이로 인해, 값과 로직의 출처를 알기 어렵고, 과도한 복잡성이 동반된다. 

React의 공식문서를 참고하면, Hooks의 개념을 통해 이 HOC 패턴을 걷어낼 수 있다고 한다. 


> Do Hooks replace render props and higher-order components?
> Often, render props and higher-order components render only a single child. We think Hooks are a simpler way to serve this use case. There is still a place for both patterns (for example, a virtual scroller component might have a renderItem prop, or a visual container component might have its own DOM structure). But in most cases, Hooks will be sufficient and can help reduce nesting in your tree.

출처: https://reactjs.org/docs/hooks-faq.html#do-hooks-replace-render-props-and-higher-order-components


주의해야 할 점은, Hooks를 통해 HOC나 render props가 불필요해 진것은 아니다. 여전히 Class components의 용도가 존재하듯이 HOC나 render props의 방식으로 해결해야 하는 문제들이 존재한다. 단지, 대부분의 경우에 Hooks를 통한 방법이 simpler한 방법일 뿐이다. 

그렇다면, 어떻게 Hooks가 HOC를 대체할 수 있는 것일까?

나의 생각은 이러하다. 

기존의 Class 방식에서는 state와 event handler들을 클래스 외부로 빼내어 전달할 방법이 마땅치 않았다. (redux 등 제외) 그런 상황에서 class로 거대하게 감싸서 공통으로 사용될 값 또는 로직을 전달해야 했고, 이게 HOC 방법이다. 

반면, Hooks는 react가 기본적으로 제공하는 hooks를 활용하여 custom hooks를 제작할 수 있다. 
이러한 custom hooks는 또 다시 별도의 파일(함수)로 분리가 가능하다. 즉, 원하는 로직이 하나의  Class Component에 종속되던 방식에서 벗어나서 독립된 로직으로 존재할 수 있다. 

hooks는 이러한 방식으로 cross-cutting concern을 해결하고, 더 간결하고 재사용 가능한 해결책을 보인것이다. 


## 오늘의 회고

오늘 다시 나의 모습으로 돌아올 수 있었다. 

주어진 여러 선택지 속에서 끊임없이 고뇌하며, 내 중심과 가치를 잊지 않고자 최선의 선택을 하였다. 

지금 이 선택을 통하여, 미래에 보다 성장한 모습이 되고 싶다. 

앞으로 남은기간 최선을 다하여 최고의 결과물을 내도록 하겠다. 