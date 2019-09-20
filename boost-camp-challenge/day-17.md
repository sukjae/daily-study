# [부스트캠프 멤버십 17일차]

## 프로젝트를 완성하지 못한 이유

1. 계획의 부재
2. 지나친 고민
3. 컨디션 저조
4. 빠르게 구현을 해보지 못함(최소한 사용가능한 모습으로 만드는것을 우선시 했어야 함)

### 1. 계획의 부재

지난주까지 스크럼을 통하여 유저 스토리를 구성하고 체크리스트를 만들었다.

그러나, 이 작업에 시간이 지나치게 많이 소요 되는듯 하여 이번 과제에서는 스크럼이나 백로그 등을 구성하지 않았다. 

그러나 이 부분은 나의 큰 착오였다. 체크리스트를 만들지 않으니, 내가 얼마나 했는지, 얼마나 해야 하는지가 명확하게 보이지 않았고, 나태하게 프로젝트를 진행하다가 마지막날에 과제가 쏟아지는 결과로 돌아왔다. 

스크럼이나 백로그 등은 선택이라 할 수 있겠지만, 개발자로서 나침반이 되어줄 체크리스트는 필수적인 요소인듯하다. 

이 부분을 절실히 느꼈으니, 앞으로는 모든 작업에 체크리스트 작성을 최우선으로 진행해야 하겠다. 

### 2. 지나친 고민

과제를 실제로 만들지 않고, 사고로만 실험하여 기획하였다. 

직접 만들어보지 않으니, 과제의 난이도를 파악함에 있어 실수를 하였고, 지나치게 얕잡아 보아 고민을 너무 많이 했다. 

특히 재사용이 가능한 라이브러리화 라는 부분에 지나치게 몰입되어서 오버 엔지니어링이 된것 같다. 이 부분은 리펙토링을 통해 이루어졌어야 했던 것 같다. 

앞으로는 고민을 하기 앞서 만들어 보는 것을 우선시 해야겠다.  

고민과 만드는것이 교차되며 쌓여야 원하는 결과물이 나오는것 같다. 

## 나의 원래 계획

### 1. 재사용이 가능한 라이브러리화

가장 처음에 고민하였던 부분은, 이 과제를 통해 나온 결과물을 어떻게 다른 프로젝트에 접목할 수 있을지 였다. 

특히, ReactJS에 어떻게 접목할 수 있을지 고민하였는데, 결론적으로 좋지 않은 고민이였다.

처음에 나는 이 캐러셀 라이브러리를 유니버셜 캐러셀 이라는 이름으로 JS 를 사용하는 모든 라이브러리에서 활용 가능한 방법을 찾아보고 싶었다. 

당시 든 생각은, VanilaJS에서 사용되는 가장 근본적인 기능들(Event, Event listener, DOM Element)를 사용하면, 이 VanilaJS를 바탕으로 만들어진 모든 라이브러리에서 사용이 가능하지 않을까 라는 나이브한 생각을 갖고있었다. 

그러나 이러한 생각은 다음과 같은 점에서 잘못되었다. 

**모든 라이브러리(React, Vue, Angular)가 동일한 철학을 갖고 움직이는게 아니다.**

가장 크게 잘못 생각하고 있던 부분이다. 내가 설계하고자 했던 라이브러리는 이벤트 드리븐 방식인것 같다. 

즉, 특정 이벤트에 따라 어떤 액션이 트리거되고, 이 결과로 또 다른 이벤트를 받아서 원하는 결과를 (view의 수정) 수행하고자 했다. 

그러나, React의 작동방식은 Event Driven과 전혀 다른 철학을 갖고 있다. React에서 값은 상위에서 하위로 흐르고, view를 수정하는것은 이벤트가 아닌, 값의 변화이다. 

물론, React에서 제공하는 기능(useRef)등으로 event를 componet에 등록할 수 있지만, 이는 React의 설계방식을 무시하는 형태가 되버린다. 

다른 라이브러리도 동일한것 같다. 

결국, 변화에 대해 다른 철학을 갖는 라이브러리들 모두에서 사용 가능한 유니버셜한 라이브러리는, 하나의 통일된 방법으로는 만들수 없는것 같다. 

**DOM element를 추상화?하여 제공하기도 한다.** 

대부분의 라이브러에서 쉽게 DOM Element를 쉽게 빼올 수 없는 것 같다. 

모던한 FE 라이브러리에서는 이 부분이 추상화 되어 있어서, 뒷쪽에서 DOM Element로 변형하는듯 하다. 

그러므로, 이 DOM Element를 직접 등록하고자 하였던 나의 생각은 쉽지 않다. 

**결론**

결국 나는 VanilaJS만을 사용하는 프로젝트에서 재 사용이 가능한 라이브러리를 만들기로 하였다,.

### 2. 컴포넌트는 로직에 대해 알지 못해야 함 (뷰와 로직의 분리)

이전에 작업을 수행하면서, 컴포넌트에(보통은 HTML 엘리먼트) 그 컴포넌트가 수행해야 하는 로직을 넣곤 했다. 그러나 라이브러리의 특성상 사용자가 원하는 컴포넌트에 직접 로직을 넣는 방식은 좋지 않다고 생각했다. 

즉, 사용자는 캐러샐 기능을 사용하기 위해 직접 컴포넌트에(HTML 엘리먼트) 접근하여 캐러셀 로직을 추가하면 안된다고 생각했다. 

그러기 위해서 정해진 어떤 컴포넌트에 접근하여 로직을 추가하는 방식(예를 들어, `getElementById`, `getElementByTagName` 등등) 보다는, 해당 컴포넌트에 대한 레퍼렌스를 캐러셀 라이브러리가 들고 있는 방식이 좋겠다고 생각했다 

그러기 위해서 캐러셀 라이브러리에 `addComponenet` 라는 메소드를 만들었고, 사용자는 이 메소드의 인자로 컴포넌트(HTML 엘리먼트)를 전달하면 되는 방식이다. 

이 `addComponent`에 Element가 들어가게 되면, 캐러셀 라이브러리는 캐러셀 작동에 필요한 값을 html custom attribute로 넣는다.(`data-carousel-index`, `data-carousel-direction`)

그 후, 캐러셀의 작동을 위한 CustomEvent와 사용자가 캐러셀 작동을 원하는 추가적인 이벤트(`click`, `mouseover`등 DOM Event)를 해당 엘리먼트에 등록한다. 

마지막으로, 이 엘리먼트에 대한 레퍼렌스를 캐러샐의 내부 배열에 저장한다. 

```js
/**
 *
 * @param {HTMLElement} htmlElement
 * @param {Function} cb
 * @param {Object} option
 * @param {Object} events
 */
addComponent = (
    htmlElement,
    cb,
    option = { index: this.elementList.length, direction: null },
    events = { click: true, mouseover: false },
  ) => {
    if (!isElement(htmlElement)) return;
    if (option.index) {
      htmlElement.setAttribute(this.INDEX_ATTR, option.index);
    }
    if (option.direction) {
      htmlElement.setAttribute(this.DIRECTION_ATTR, option.direction);
    }
    // adding default carousel event name
    htmlElement.addEventListener(this.EVENT_NAME, cbWrapper(this.handleActivation, cb));

    // adding DOM api default events : click, mouseover,...
    Object.keys(events).forEach(eventName => {
      if (events[eventName]) {
        htmlElement.addEventListener(eventName, cbWrapper(this.handleIndex, cb));
      }
    });

    // adding given element to internal array to send event later
    this.elementList.push(htmlElement);
  };
```

위 Carousel 클래스(UniCar) 사용 방법 (JS에서)

```js
const carousel = new UniCar();
const cb = ()=>{} // some callback

// target element, without direction
const card = document.getElementById('card'); 
carousel.addComponent(card, cb, { index: val, direction: null });

// target element, with direction
const rightButton = document.getElementById('right-button');
carousel.addComponent(rightButton, cb, { direction: 1 });
```

### 3.  하나의 이벤트에 대해서 다수의 컴포넌트가 반응해야 한다.

처음에는, 이 목표를 달성하기 위해서 Event의 capturing을 사용하려 하였다. 

상위의 엘리먼트에서 이벤트가 발생하면, 이 이벤트가 전파되어 하위의 모든 엘리먼트에 이벤트가 잡힐 줄 알았다. 

그러나, 생각과는 다르게 Event의 capturing은 Event가 발생한 곳 (CurrentTarget)까지만 찍고 다시 올라오는 것으로 확인 되었다. 

그러므로, 상위의 엘리먼트에서 이벤트를 Dispatch 시키더라도, 더 깊이 들어가는게 아니라 최상위에서 해당 엘리먼트까지만 들어갔다가 나온다. 

![](https://www.w3.org/TR/2007/WD-DOM-Level-3-Events-20071221/images/eventflow.png)

*이미지 출처: [https://www.w3.org/TR/2007/WD-DOM-Level-3-Events-20071221/events.html](https://www.w3.org/TR/2007/WD-DOM-Level-3-Events-20071221/events.html)*

따라서, **[하나의 이벤트에 대해서 다수의 컴포넌트가 반응해야 한다]** 를 수행하기 위해서 다른 방법을 이용해야 하였고, Observer 패턴과 비슷한 방식으로 진행하였다. 

캐러셀 클래스 내부에는 Dispatch를 해야하는 대상 Element들이 배열로 저장되어 있다. 

이 배열을 순회하면서 하나하나씩 CustomEvent를 Dispatch 함으로서 원하는 결과를 만들 수 있었다. 

```js
emmitEvent = () => {
  // send current index stored inside Carousell class
  const event = new CustomEvent(this.EVENT_NAME, {
    detail: {
      currentIdx: this.currentIdx,
    },
  });

  // loop elements which needed to be dispatched
  this.elementList.forEach(element => element.dispatchEvent(event));
};
```

다만, 이 방법에도 문제가 있는데, 여기에서 dispatchEvent 는 sync하게 돌아간다. 

그러므로, 처음에 구성하였던 async하게 동시다발적으로 이벤트가 전달되는 방법과는 차이가 있다. 

> Unlike "native" events, which are fired by the DOM and invoke event handlers asynchronously via the event loop, dispatchEvent invokes event handlers synchronously. All applicable event handlers will execute and return before the code continues on after the call to dispatchEvent.

출처: MDN

## 앞으로의 계획(현 프로젝트 중심)

1. 캐로셀의 로직 부분(커맨더)만 따로 분리하여 오픈
2. DOM CustomEvent의 명세에 따르면, Dispatch 작업은 sync하게 움직인다. 이를 async하게 바꿔주는 작업이 필요
3. UI 부분의 우선순위는 가장 뒤로...

## 앞으로의 계획(부스트 캠프를 임하며)

1. 좀더 구체적인 시간 분배
    - 1일차에는 전반적인 스케줄에 대한 계획 수렴 (체크 리스트 필수!)
    - 2일차에 최소한의 작동하는 형태로 구현
    - 3일차에 리펙토링
2. 컨디션 되돌리기
    - 충분한 휴식 필요
3. **작업 템포 올리기**
    - 데드라인을 금요일이 아닌, 수요일로 바꾼다.