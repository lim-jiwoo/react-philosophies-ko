# react-philosophies-ko
🧘 리액트 코드를 작성할 때 생각할 것들 🧘 [mithi/react-philosophies](https://github.com/mithi/react-philosophies) 의 한국어 번역본 _(마지막 업데이트: 2023.04.08)_

## 목차

0. [소개글](#-0-소개글)
1. [최소한의 원칙](#-1-최소한의-원칙)
2. [행복을 위한 디자인](#-2-행복을-위한-디자인)
3. [성능 팁](#-3-성능-팁)
4. [테스팅 원칙](#-4-테스팅-원칙)
5. [다른 사람들이 공유한 인사이트](#-5-다른-사람들이-공유한-인사이트)

## 🧘 0. 소개글

`react-philosophies` 는:

- `React` 코드를 작성할 때 제가 생각하는 것들입니다.
- 다른 사람이나 제 코드를 검토할 때 항상 염두에 두고 있습니다.
- 가이드라인일 뿐이며, 반드시 따를 필요는 없습니다.
- 지속적으로 개정될 수 있는 문서로, 제 경험이 쌓일수록 발전할 예정입니다.
- 대부분 기본적인 [리팩토링](https://en.wikipedia.org/wiki/Code_refactoring) 방법, [SOLID](https://en.wikipedia.org/wiki/SOLID) 원칙, 그리고 [익스트림 프로그래밍](https://en.wikipedia.org/wiki/Extreme_programming) 아이디어의 변형 기술입니다... 그저 `React`에 맞춰 적용한 것뿐이죠 🙂

`react-philosophies` 는 제 코딩 여정 중 발견한 여러 곳에서 영감을 받았습니다.

다음은 그중 몇 가지입니다:

- [Sandi Metz](https://sandimetz.com/)
- [Kent C Dodds](https://kentcdodds.com)
- [Zen of Python (PEP 20)](https://www.python.org/dev/peps/pep-0020/), [Zen of Go](https://dave.cheney.net/2020/02/23/the-zen-of-go)
- [trekhleb/state-of-the-art-shitcode](https://github.com/trekhleb/state-of-the-art-shitcode), [droogans/unmaintainable-code](https://github.com/Droogans/unmaintainable-code), [sapegin/washingcode-book](https://github.com/sapegin/washingcode-book/)

## 🧘 1. 최소한의 원칙

### 1.1 컴퓨터가 나보다 똑똑한 경우를 인식하자

1. [`ESLint`](https://eslint.org/)로 코드를 정적 분석하자. [`rule-of-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks) 및 `exhaustive-deps` 규칙을 활성화하여 `React` 특정 오류를 잡을 수 있다.
2. ["strict"](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) 모드를 활성화하자. 2023년이니까. 
3. [모든 의존성을 솔직하게 명시하자](https://overreacted.io/a-complete-guide-to-useeffect/#two-ways-to-be-honest-about-dependencies). `useMemo`, `useCallback` 및 `useEffect` 에서 뜨는 `exhaustive-deps` 경고/오류를 고치자. 불필요한 리렌더링 없이 콜백을 항상 최신으로 유지하기 위해 ["최신 ref 패턴"](https://epicreact.dev/the-latest-ref-pattern-in-react) 을 사용해볼 수 있다.
4. 컴포넌트를 표시하기 위해 `map`을 사용할 때 [반드시 키를 추가하자](https://epicreact.dev/why-react-needs-a-key-prop).
5. [훅은 항상 최상위 레벨에서만 호출하자](https://reactjs.org/docs/hooks-rules.html). 반복문이나 조건문, 중첩 함수 내부에서 훅을 호출하지 말자.
6. "Can't perform state update on unmounted component" 경고를 이해하자. [facebook/react/pull/22114](https://github.com/facebook/react/pull/22114)
7. 애플리케이션의 여러 레벨에 [error boundaries](https://reactjs.org/docs/error-boundaries.html) 를 추가해 ["죽음의 하얀 화면(white screen of death)"](https://kentcdodds.com/blog/use-react-error-boundary-to-handle-errors-in-react) 를 방지하자. 또한 원한다면 이를 사용해 [Sentry](https://sentry.io) 같은 오류 모니터링 서비스에 알림을 보낼 수도 있다. ([React 에서 에러 처리하기](https://brandondail.com/posts/fault-tolerance-react))
8. 콘솔에 오류나 경고가 뜨는 데는 다 이유가 있다.
9. [`tree-shaking`](https://webpack.js.org/guides/tree-shaking/)을 기억하자!
10. [Prettier](https://prettier.io/) (또는 그 대안) 은 코드 형식을 자동으로 수정해 매번 일관된 스타일을 유지해준다. 더 이상 이에 대해 생각할 필요가 없다!
11. [`Typescript`](https://www.typescriptlang.org/) 나 [`NextJS`](https://nextjs.org/) 같은 프레임워크는 삶을 더 쉽게 만들어준다.
12. 오픈 소스 리포지토리이거나, 여유가 있다면 [Code Climate](https://codeclimate.com/quality/) (또는 비슷한 툴) 을 강력 추천한다. 자동 감지된 코드 스멜(code smell) 은 현재 애플리케이션의 기술 부채를 줄이는 데 큰 동기부여가 된다!

### 1.2 코드는 필요악이다

> "최선의 코드는 아무 코드도 적지 않는 것이다. 당신이 만든 새로운 한 줄의 코드는 곧 디버깅이 필요한 코드, 읽고 이해해야 하는 코드, 지원해야 하는 코드가 된다." ― Jeff Atwood

> "나의 가장 생산적인 날 중 하나는 천 줄의 코드를 삭제한 날이었다." ― Eric S. Raymond

더보기: [Write Less Code - Rich Harris](https://svelte.dev/blog/write-less-code), [Code is evil - Artem Sapegin](https://github.com/sapegin/washingcode-book/blob/master/manuscript/Code_is_evil.md)

#### 1.2.1 의존성을 추가하기 전에 한 번 더 생각하자

의존성을 추가할수록, 더 많은 코드를 브라우저로 불러와야 한다. 해당 라이브러리를 훌륭하게 만드는 기능을 실제로 사용하고 있는지 자문하자.

<details>
    <summary><strong><em>🙈  정말 필요한가?</strong> 불필요할 수도 있는 의존성/코드 예시</em></summary>

<br/>

1. [`Redux`](https://redux.js.org/) 가 정말 필요할까? 그럴 수도 있지만, React 자체가 이미 [상태 관리 라이브러리](https://kentcdodds.com/blog/application-state-management-with-react) 임을 잊지 말자.

2. [`Apollo client`](https://www.apollographql.com/docs/react/) 가 정말 필요할까? Apollo client 에는 수동 정규화와 같은 멋진 기능이 많지만, 번들 크기를 많이 증가시킨다. Apollo client 에만 존재하는 기능을 사용하는 게 아니라면, [`react-query`](https://react-query.tanstack.com/comparison) 나 [`SWR`](https://github.com/vercel/swr) 같이 더 작은 라이브러리를 고려해보자 (혹은 아예 사용하지 않는 것도 방법이다).

3. [`Axios`](https://github.com/axios/axios) 는 어떨까? Axios 는 `fetch` 내장함수로는 쉽게 대체할 수 없는 기능을 제공하는 멋진 라이브러리이다. 하지만 만일 Axios 를 사용하는 유일한 이유가 단지 API 가 더 깔끔하게 생겼기 때문이라면, fetch 를 래퍼로 감싸 사용하는 것을 고려해보자 ([`redaxios`](https://github.com/developit/redaxios) 나 직접 구현해서). 애플리케이션이 Axios 의 최고의 기능들을 정말로 사용하고 있는지 생각해보자.

4. [`Decimal.js`](https://github.com/MikeMcl/decimal.js/) 는? [Big.js](https://github.com/MikeMcl/big.js/) 나 [다른 작은 라이브러리](https://www.npmtrends.com/big.js-vs-bignumber.js-vs-decimal.js-vs-mathjs) 로 충분할 수도 있다.

5. [`Lodash`](https://lodash.com/)/[`underscoreJS`](https://underscorejs.org/) 는? [you-dont-need/You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)

6. [`MomentJS`](https://momentjs.com/) 는? [you-dont-need/You-Dont-Need-Momentjs](https://github.com/you-dont-need/You-Dont-Need-Momentjs)

7. 테마(라이트/다크 모드)를 위해 `Context` 가 필요하지 않을 수도 있다. 대신 [`css 변수`](https://epicreact.dev/css-variables) 사용을 고려해보자.

8. 심지어 `Javascript` 마저 필요하지 않을 수도 있다. CSS 는 강력하다. [you-dont-need/You-Dont-Need-JavaScript](https://github.com/you-dont-need/You-Dont-Need-JavaScript)
<br/>

</details>

#### 1.2.2 똑똑하려 하지 말자. YAGNI!

> "미래에 내 소프트웨어에 어떤 일이 생길까? 아, 이런 일이나 저런 일이 생길 수도 있겠군. 어차피 이 부분을 작업하고 있으니 미리 다 구현해버리자. 미래를 대비해서."

필요 없을 거다 (**Y**ou **A**ren't **G**onna **N**eed **I**t)! 언제나 실제로 필요할 때 구현하고, 미리 예상해서 구현하지 말자. 코드는 적을 수록 좋다! ([Martin Fowler: YAGNI](https://martinfowler.com/bliki/Yagni.html), [C2 Wiki: You Arent Gonna Need It!](https://wiki.c2.com/?YouArentGonnaNeedIt))

관련 섹션: [2.4 중복은 잘못된 추상화보다 훨씬 저렴하다](#-24-중복은-잘못된-추상화보다-훨씬-저렴하다)

### 1.3 한번 방문한 코드는 더 나은 상태로 떠나라

**1.3.1 코드 스멜을 감지하고 필요하면 조치를 취하자** 

뭔가 잘못된 부분을 발견했다면, 즉시 고치자. 만약 쉽게 고칠 수 없거나 당장 고칠 시간이 없다면, 적어도 문제에 대한 간결한 설명을 담은 주석이라도 달자 (`FIXME` 또는 `TODO`). 다른 모두가 문제가 있다는 걸 알 수 있도록 하자. 당신이 신경을 쓰고 있다는 걸 알릴 뿐 아니라, 다른 사람들도 이와 같은 문제를 발견했을 때 똑같이 하도록 독려할 수 있다.

<details>
    <summary><strong>🙈 쉽게 잡을 수 있는 코드 스멜 예시</strong></summary>

<br/>

- ❌ 매우 많은 인수를 받는 메서드 또는 함수
- ❌ 이해하기 어려운 boolean 로직
- ❌ 한 파일 내에 너무 많은 줄의 코드가 있는 것
- ❌ (서식이 다르더라도) 구문상(syntactically) 동일한 중복되는 코드 
- ❌ 이해하기 어려운 함수 또는 메서드
- ❌ 많은 함수나 메서드를 가진 클래스/컴포넌트
- ❌ 단일 함수나 메서드 내에 너무 많은 줄의 코드가 있는 것
- ❌ 반환문이 많은 함수 또는 메서드
- ❌ 일치하진 않지만 구조가 동일한 중복되는 코드 (예: 변수명이 다름)

</details>

코드 스멜은 반드시 코드를 수정해야 함을 의미하진 않는다. 그저 같은 기능을 더 나은 방법으로 구현할 수도 있음을 알려줄 뿐이다

**1.3.2 가차없이 리팩터링하라. 단순함이 복잡함보다 낫다**

> CL _(역: 구글에서 PR을 가리키는 용어)_ 이 필요 이상으로 복잡한가? CL 의 모든 레벨에서 다음을 확인하라 — 각 줄이 너무 복잡한가? 함수가 너무 복잡한가? 클래스가 너무 복잡한가? "너무 복잡하다"라는 건 일반적으로 "코드를 읽는 사람이 빨리 이해할 수 없다"는 것을 의미한다. 또는 "개발자가 이 코드를 호출하거나 수정하려 할 때 버그를 발생시킬 확률이 높다"는 것을 의미하기도 한다. ― [Google Engineering Practices: What to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

**💁‍♀️ TIP: [복잡한 조건문](https://github.com/sapegin/washingcode-book/blob/master/manuscript/Avoid_conditions.md) 을 단순화하고, 가능하다면 빨리 리턴(early return)하자**

<details>
    <summary>🙈 빠른 리턴 예시 </summary>

```tsx
# ❌ Not-so-good

if (loading) {
  return <LoadingScreen />
} else if (error) {
  return <ErrorScreen />
} else if (data) {
  return <DataScreen />
} else {
  throw new Error('This should be impossible')
}

# ✅ BETTER

if (loading) {
  return <LoadingScreen />
} 

if (error) {
  return <ErrorScreen />
}

if (data) {
  return <DataScreen />
}

throw new Error('This should be impossible')
```

</details>

**💁‍♀️ TIP: 반복문보다 연쇄적인 고차 함수(chained HOF)를 선호하자**

뚜렷한 성능 차이가 없고 가능하다면, 전통적인 반복문을 연쇄된 고차 함수(`map`, `filter`, `find`, `findIndex`, `some` 등)로 대체하자. [Stackoverflow: 반복문 대신 함수를 사용했을 때 장점은 무엇인가요?](https://stackoverflow.com/questions/34402198/what-is-the-advantage-of-using-a-function-over-loops)

### 1.4 더 잘할 수 있다

**💁‍♀️ TIP: 상태(state)를 의존성으로 넣어줄 필요가 없을 수도 있다. 대신 콜백 함수를 전달할 수 있다**

`useEffect` 나 `useCallback` 훅의 의존성 배열에 (`useState` 의) `setState` 나 (`useReducer` 의) `dispatch` 는 넣지 않아도 된다. React 가 이들의 안정성을 보장하기 때문에 ESLint 는 불평하지 않을 것이다.

<details>
    <summary>🙈 예시</summary>

```tsx
❌ Not-so-good
const decrement = useCallback(() => setCount(count - 1), [setCount, count])
const decrement = useCallback(() => setCount(count - 1), [count])

✅ BETTER
const decrement = useCallback(() => setCount(count => (count - 1)), [])
```

</details>

**💁‍♀️ TIP: 만약 `useMemo` 나 `useCallback` 에 의존성이 없다면, 뭔가 잘못 사용하고 있는 것일 수 있다**

<details>
    <summary>🙈 예시</summary>
 
 <br />
 
 ```tsx
 ❌ Not-so-good
const MyComponent = () => {
    const functionToCall = useCallback(x: string => `Hello ${x}!`,[])
    const iAmAConstant = useMemo(() => { return {x: 5, y: 2} }, [])
    /* 여기서 functionToCall 와 iAmAConstant 사용 */
}
        
✅ BETTER 
const I_AM_A_CONSTANT =  { x: 5, y: 2 }
const functionToCall = (x: string) => `Hello ${x}!`
const MyComponent = () => {
    /* 여기서 functionToCall 와 iAmAConstant 사용 */
}
````
</details>

**💁‍♀️ TIP: 커스텀 컨텍스트를 훅으로 감싸면 더 보기 좋은 API 를 만들 수 있다**

더 보기 좋을 뿐 아니라, 이제 두 개가 아닌 하나만 import 하면 된다.

<details>
    <summary>🙈 예시</summary>

 <br />
 
```tsx
❌ Not-so-good
// 매번 두 개를 모두 import 해야 한다
import { useContext } from "react"
import { SomethingContext } from "some-context-package"

function App() {
  const something = useContext(SomethingContext) // 나쁘지 않지만, 개선할 수 있다
}
```

```tsx
✅  Better  
// 파일 하나에 다음 훅을 선언한다
function useSomething() {
  const context = useContext(SomethingContext)
  if (context === undefined) {
    throw new Error('useSomething must be used within a SomethingProvider')
  }
  return context
}
  
// 매번 하나만 import 하면 된다
import { useSomething } from "some-context-package"

function App() {
  const something = useSomething() // 더 보기 좋다
}  
```

</details>

**💁‍♀️ TIP: 컴포넌트를 코딩하기 전에 어떻게 사용될지 생각하자**

[API 작성은 어렵다](http://sweng.the-davies.net/Home/rustys-api-design-manifesto). 더 나은 API 를 디자인하는 데 도움이 되는 유용한 기법 중 하나는 [`README Driven Development`](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html) 이다. [RDD](https://rathes.me/blog/en/readme-driven-development/) 를 맹목적으로 따르자는 의미는 아니지만, 아이디어는 훌륭하다고 생각한다. 경험상 구현하기 전에 API (컴포넌트가 어떻게 사용될지) 를 먼저 작성하면, 대부분 그렇지 않았을 때보다 더 잘 디자인된 컴포넌트가 만들어지곤 했다.

## 🧘 2. 행복을 위한 디자인

> "컴퓨터가 이해할 수 있는 코드는 바보라도 작성할 수 있다. 좋은 개발자는 사람이 이해할 수 있는 코드를 작성한다." ― Martin Fowler

> "읽는 데 걸리는 시간 대 쓰는 데 걸리는 시간의 비율은 10 대 1 이상이다. 우린 새로운 코드를 적기 위해 늘 옛날 코드를 읽는다. 따라서 빨리 일하고 싶다면, 코드를 쉽게 작성하고 싶다면, 읽기 쉽게 만들어라." ― Robert C. Martin [(그의 정치적 견해에 동의한다는 것은 아님)](https://www.getrevue.co/profile/tech-bullshit/issues/tech-bullshit-explained-uncle-bob-830918)

**TL;DR**

1. 💖 중복되는 상태를 제거해 복잡한 상태 관리를 피하자.
2. 💖 바나나만 전달하자. 바나나를 들고 있는 고릴라와 전체 정글을 다 같이 전달하지 말고. (props 로 원시 값을 전달하자)
3. 💖 컴포넌트를 작고 단순하게 유지하자 - 단일 책임 원칙!
4. 💖 중복은 잘못된 추상화보다 훨씬 저렴하다 (너무 이른 / 부적절한 일반화를 지양하자)
5. 합성 (composition) 을 사용해 프로퍼티 드릴링 (prop drilling) 을 피하자 ([Michael Jackson](https://www.youtube.com/watch?v=3XaXKiXtNjw)). `Context` 가 모든 상태 공유 문제에 대한 답은 아니다.<br>
_(역: 리액트 context 에 넣는 것은 prop 이어야 하며, Context 를 잘 사용한 경우는 잘 보지 못했고, 대부분의 경우 내 애플리케이션이 아닌 라이브러리 코드에 대해 사용하는 것이 옳았다고 말한다)_
6. 거대한 `useEffect` 를 작은 독립적인 `useEffect` 들로 나누자. ([KCD: Myths about useEffect](https://epicreact.dev/myths-about-useeffect))
7. 로직을 훅과 헬퍼 함수로 추출하자.
8. `useCallback`, `useMemo`, `useEffect` 의 의존성으로 가능한 한 원시 값만 사용하자.
9. `useCallback`, `useMemo`, `useEffect` 에 너무 많은 의존성을 두지 말자.
10. 상태의 일부 값이 다른 상태 값 또는 이전 상태 값에 의존하는 경우, 다수의 `useState` 보다 `useReducer` 를 사용하면 더 간소해진다.
11. `Context` 를 전체 애플리케이션에 전역으로 사용할 필요는 없다. `Context` 는 컴포넌트 트리에서 최대한 낮은 위치에 배치해라. 변수, 주석, 상태 (그리고 그냥 전반적으로 코드) 를 연관된/사용되는 곳에 최대한 가까이 위치시키는 것과 동일하다.

### 💖 2.1 중복되는 상태를 제거해 복잡한 상태 관리를 피하자

중복된 상태가 있으면 일부 상태가 동기화되지 않을 수 있다. 복잡한 상호작용 시퀀스를 따라가다 보면 상태 값 갱신을 잊을 수 있기 때문이다. 동기화 버그를 피하는 것 외에도, 이해하기도 더 쉽고 코드 양도 줄일 수 있다. 더보기: [KCD: Don't Sync State. Derive It!](https://kentcdodds.com/blog/dont-sync-state-derive-it), [Tic-Tac-Toe](https://epic-react-exercises.vercel.app/react/hooks/1)

##### 🙈 예시 1

<details>
    <summary><strong> 📝🖊️ 비즈니스 요구사항 / 문제 설명 보기 </strong></summary>


---

당신은 직각 삼각형의 다음 속성을 표시해야 한다.
- 세 변의 길이
- 둘레
- 넓이

삼각형은 API 로 가져온 두 숫자 `{a: number, b: number}` 로 구성된 객체이다. 두 숫자는 직각 삼각형의 짧은 두 변을 나타낸다.

---

</details>

<details>
 <summary> ❌ 좋지 않은 솔루션 </summary>

```tsx
const TriangleInfo = () => {
  const [triangleInfo, setTriangleInfo] = useState<{a: number, b: number} | null>(null)
  const [hypotenuse, setHypotenuse] = useState<number | null>(null)
  const [perimeter, setPerimeter] = useState<number | null>(null)
  const [areas, setArea] = useState<number | null>(null)

  useEffect(() => {
    fetchTriangle().then(t => setTriangleInfo(t))
  }, [])

  useEffect(() => {
    if(!triangleInfo) {
      return
    }
    
    const { a, b } = triangleInfo
    const h = computeHypotenuse(a, b)
    setHypotenuse(h)
    const newArea = computeArea(a, b)
    setArea(newArea)
    const p = computePerimeter(a, b, h)
    setPerimeter(p)

  }, [triangleInfo])

  if (!triangleInfo) {
    return null
  }

  /*** show info here ****/
}
````

</details>

<details>
  <summary> ✅ 더 "나은" 솔루션 </summary>

```tsx
const TriangleInfo = () => {
  const [triangleInfo, setTriangleInfo] = useState<{
    a: number;
    b: number;
  } | null>(null)

  useEffect(() => {
    fetchTriangle().then((r) => setTriangleInfo(r))
  }, []);

  if (!triangleInfo) {
    return
  }

  const { a, b } = triangeInfo
  const area = computeArea(a, b)
  const hypotenuse = computeHypotenuse(a, b)
  const perimeter = computePerimeter(a, b, hypotenuse)
 
  /*** show info here ****/
};
```

</details> 
  
##### 🙈 예시 2

<details>
    <summary><strong> 📝🖊️ 비즈니스 요구 사항 / 문제 설명 보기 </strong></summary>


---

당신은 다음 조건을 만족하는 컴포넌트를 디자인해야 한다:

1. 고유한 점들의 목록을 API 로 가져온다.
2. `x` 또는 `y` 로 정렬할 수 있는 버튼이 있다 (오름차순)
3. `maxDistance` 를 변경할 수 있는 버튼이 있다 (`10` 씩 증가하며, 초기값은 `100` 이다)
4. 원점 `(0, 0)` 에서 현재 `maxDistance` 보다 더 떨어져 있지 않은 점들만 표시한다
5. 목록에 100개의 항목만 있다고 가정한다 (따라서 최적화는 걱정할 필요가 없다). 많은 수의 항목을 처리하는 경우에는 `useMemo` 로 일부 계산을 메모이제이션 할 수 있다.

---

</details>

<details>
  <summary> ❌ 좋지 않은 솔루션 </summary>
  
```tsx
type SortBy = 'x' | 'y'
const toggle = (current: SortBy): SortBy => current === 'x' ? : 'y' : 'x'

const Points = () => {
  const [points, setPoints] = useState<{x: number, y: number}[]>([])
  const [filteredPoints, setFilteredPoints] = useState<{x: number, y: number}[]>([])
  const [sortedPoints, setSortedPoints] = useState<{x: number, y: number}[]>([])
  const [maxDistance, setMaxDistance] = useState<number>(100)
  const [sortBy, setSortBy] = useState<SortBy>('x')
  
  useEffect(() => {
    fetchPoints().then(r => setPoints(r))
  }, [])
  
  useEffect(() => {
    const sorted = sortPoints(points, sortBy)
    setSortedPoints(sorted)
  }, [sortBy, points])

  useEffect(() => {
    const filtered = sortedPoints.filter(p => getDistance(p.x, p.y) < maxDistance)
    setFilteredPoints(filtered)
  }, [sortedPoints, maxDistance])

  const otherSortBy = toggle(sortBy)
  const pointToDisplay = filteredPoints.map(
    p => <li key={`${p.x}|{p.y}`}>({p.x}, {p.y})</li>
  )

  return (
    <>
      <button onClick={() => setSortBy(otherSortBy)}>
        Sort by: {otherSortBy}
      <button>
      <button onClick={() => setMaxDistance(maxDistance + 10)}>
        Increase max distance
      <button>
      Showing only points that are less than {maxDistance} units away from origin (0, 0)
      Currently sorted by: '{sortBy}' (ascending)
      <ol>{pointToDisplay}</ol>
    </>
  )
}

````
</details>

<details>
  <summary> ✅ 더 "나은" 솔루션 </summary>

```tsx

// NOTE: You can also use useReducer instead
type SortBy = 'x' | 'y'
const toggle = (current: SortBy): SortBy => current === 'x' ? : 'y' : 'x'

const Points = () => {
  const [points, setPoints] = useState<{x: number, y: number}[]>([])
  const [maxDistance, setMaxDistance] = useState<number>(100)
  const [sortBy, setSortBy] = useState<SortBy>('x')

  useEffect(() => {
    fetchPoints().then(r => setPoints(r))
  }, [])
  

  const otherSortBy = toggle(sortBy)
  const filtedPoints = points.filter(p => getDistance(p.x, p.y) < maxDistance)
  const pointToDisplay = sortPoints(filteredPoints, sortBy).map(
    p => <li key={`${p.x}|{p.y}`}>({p.x}, {p.y})</li>
  )

  return (
    <>
      <button onClick={() => setSortBy(otherSortBy)}>
        Sort by: {otherSortBy} <button>
      <button onClick={() => setMaxDistance(maxDistance + 10)}>
        Increase max distance
      <button>
      Showing only points that are less than {maxDistance} units away from origin (0, 0)
      Currently sorted by: '{sortBy}' (ascending)
      <ol>{pointToDisplay}</ol>
    </>
  )
}
````

</details>

### 💖 2.2 바나나만 전달하자. 바나나를 들고 있는 고릴라와 전체 정글을 다 같이 전달하지 말고

> "바나나를 원했는데 바나나를 들고 있는 고릴라와 정글 전체를 얻었다." ― Joe Armstrong

이러한 함정에 빠지지 않으려면, 가능한 한 원시 값 (`boolean`, `string`, `number` 등) 을 props 로 전달하는 것이 좋다. (`React.memo` 로 최적화하고 싶을 때도 원시 값을 넘기는 게 좋다)

> 컴포넌트는 딱 자기 일을 수행할 수 있을 만큼만 알고, 그 이상은 알지 못해야 한다. 컴포넌트는 되도록 다른 컴포넌트가 무엇이고 무엇을 하는지 모르는 채 협력할 수 있어야 한다.

이렇게 하면 컴포넌트가 느슨하게 결합하여 두 컴포넌트 간의 의존도가 낮아진다. 느슨하게 결합한 컴포넌트는 다른 컴포넌트에 영향을 미치지 않고 변경, 교체 또는 제거하기 쉬워진다. [stackoverflow:2832017](https://stackoverflow.com/questions/2832017/what-is-the-difference-between-loose-coupling-and-tight-coupling-in-the-object-o)

##### 🙈 Example

<details>
    <summary><strong> 📝🖊️ 비즈니스 요구사항 / 문제 설명 보기 </strong></summary>


---

당신은 두 컴포넌트 `Summary` 와 `SeeMore` 를 표시하는 `MemberCard` 컴포넌트를 만들어야 한다.
`MemberCard` 는 `id` 를 prop 으로 받는다. `MemberCard` 는 `id` 를 받아 해당 `Member` 정보를 반환하는 `useMember` 훅을 사용한다.

```ts
type Member = {
  id: string
  firstName: string
  lastName: string
  title: string
  imgUrl: string
  webUrl: string
  age: number
  bio: string
  /****** 100 more fields ******/
}
```

`SeeMore` 컴포넌트는 `member` 의 `age` 와 `bio` 를 표시해야 한다. `member` 의 `age` 와 `bio` 를 표시하거나 숨기는 토글 버튼을 포함한다. 

`Summary` 컴포넌트는 `member` 의 사진을 표시한다. 또한 `title`, `firstName` 및 `lastName` 을 표시한다 (예: `Mr. Vincenzo Cassano`). `member` 의 이름을 클릭하면 `member` 의 개인 사이트로 이동한다. `Summary` 컴포넌트에는 다른 기능도 있을 수 있다. (예: 컴포넌트를 클릭할 때마다 폰트, 이미지 크기 및 배경색이 무작위로 변경된다. 간단히 "랜덤 스타일링 기능" 이라고 부르자)

---

</details>

<details>
  <summary>❌ 좋지 않은 솔루션</summary>
  
```tsx

const Summary = ({ member } : { member: Member }) => {
  /*** include "the random styling feature" ***/
  return (
    <>
      <img src={member.imgUrl} />
      <a href={member.webUrl} >
        {member.title}. {member.firstName} {member.lastName}
      </a>
    </>
  )
}

const SeeMore = ({ member }: { member: Member }) => {
  const [seeMore, setSeeMore] = useState<boolean>(false)
  return (
    <>
      <button onClick={() => setSeeMore(!seeMore)}>
        See {seeMore ? "less" : "more"}
      </button>
      {seeMore && <>AGE: {member.age} | BIO: {member.bio}</>}
    </>
  )
}

const MemberCard = ({ id }: { id: string })) => {
  const member = useMember(id)
  return <><Summary member={member} /><SeeMore member={member} /></>
}

````

</details>


<details>
  <summary>✅ 더 "나은" 솔루션</summary>

```tsx

const Summary = ({ imgUrl, webUrl, header }: { imgUrl: string, webUrl: string, header: string }) => {
  /*** include "the random styling feature" ***/
  return (
    <>
      <img src={imgUrl} />
      <a href={webUrl}>{header}</a>
    </>
  )
}

const SeeMore = ({ componentToShow }: { componentToShow: ReactNode }) => {
  const [seeMore, setSeeMore] = useState<boolean>(false)
  return (
    <>
      <button onClick={() => setSeeMore(!seeMore)}>
        See {seeMore ? "less" : "more"}
      </button>
      {seeMore && <>{componentToShow}</>}
    </>
  )
}


const MemberCard = ({ id }: { id: string }) => {
  const { title, firstName, lastName, webUrl, imgUrl, age, bio } = useMember(id)
  const header = `${title}. ${firstName} ${lastName}`
  return (
    <>
      <Summary {...{ imgUrl, webUrl, header }} />
      <SeeMore componentToShow={<>AGE: {age} | BIO: {bio}</>} />
    </>
  )
}
````

</details>

`✅ 더 "나은" 솔루션`에서, `SeeMore` 와 `Summary` 를 `Member` 에서뿐만 아니라 다른 데서도 사용할 수 있다는 것에 주목하자. `CurrentUser`, `Pet`, `Post` 등 해당 기능이 필요한 모든 객체에서 사용할 수 있다.

### 💖 2.3 컴포넌트를 작고 단순하게 유지하자

**단일 책임 원칙이란?**

> 컴포넌트는 **단 하나의** 일만 해야 한다. 또한 가능한 가장 작은 단위의 유의미한(useful) 일을 해야 한다. 컴포넌트는 그 목적을 충족하는 책임만을 져야 한다.

여러 책임을 지는 컴포넌트는 재사용하기 어렵다. 일부 동작만 재사용하려는 경우에도, 필요한 것만 가져오는 것이 거의 불가능하다. 또한, 다른 코드와 엮여있는 경우가 많다. 나머지 애플리케이션과 분리된 하나의 일만 하는 컴포넌트는 부작용 없이 변경할 수 있으며 중복 없이 재사용할 수 있다.

**컴포넌트가 단일 책임을 지고 있는지 어떻게 알 수 있을까?**

> 컴포넌트를 한 문장으로 설명해보라. 한 가지 책임만 가지고 있다면 쉽게 설명할 수 있을 것이다. '그리고' 나 '또는' 같은 단어를 사용한다면 해당 컴포넌트는 이 테스트를 통과하지 못할 가능성이 높다.

컴포넌트의 상태, props, 훅, 그리고 컴포넌트 내부에 선언된 변수 및 메서드를 검사하고 (너무 많지 않아야 한다) 자문하자: 이들이 실제로 컴포넌트의 목적을 달성하기 위해 함께 작동하는가? 일부가 그렇지 않다면, 이들을 다른 곳으로 옮기거나 큰 컴포넌트를 더 작은 단위로 분해하는 방안을 고려해보자.

(위 단락은 2015년에 작성한 필자의 글을 바탕으로 한다: [Three things I learned from Sandi Metz’s book as a non-Ruby programmer](https://medium.com/@mithi/review-sandi-metz-s-poodr-ch-1-4-wip-d4daac417665))

##### 🙈 Example

<details>
    <summary><strong> 📝🖊️ 비즈니스 요구사항 / 문제 설명 보기 </strong></summary>


---

당신은 특정 카테고리의 아이템을 구매할 수 있는 특수한 종류의 버튼을 표시해야 한다. 예를 들어, 사용자는 가방, 의자 및 음식을 선택할 수 있다.

- 각 버튼은 아이템을 선택하고 "저장"할 수 있는 모달을 연다.
- 사용자가 특정 카테고리에서 선택한 항목을 "저장"한 경우, 해당 카테고리는 "예약"된다.
- 예약된 경우, 해당 버튼에는 체크마크(∨)가 표시된다.
- 해당 카테고리가 예약된 상태라도 예약을 수정(아이템을 추가하거나 삭제함)할 수 있어야 한다.
- 사용자가 버튼 위를 hover하면 `WavingHand` 컴포넌트를 함께 표시해야 한다.
- 특정 카테고리에 아이템이 없을 때 버튼을 비활성화해야 한다.
- 사용자가 비활성화된 버튼 위를 hover하면 툴팁에 "사용 불가능"이라는 메시지가 표시돼야 한다.
- 카테고리에 아이템이 없으면 버튼의 배경은 회색이어야 한다.
- 해당 카테고리가 예약된 상태라면 버튼의 배경은 초록색이어야 한다.
- 해당 카테고리에 아이템이 있고 예약되지 않은 상태라면 버튼의 배경은 빨간색이어야 한다.
- 카테고리에 해당하는 버튼에는 고유한 라벨 및 아이콘이 있어야 한다.

---

</details>
   
<details>
    <summary>❌ 좋지 않은 솔루션</summary>

```tsx
type ShopCategoryTileProps = {
  isBooked: boolean
  icon: ReactNode
  label: string
  componentInsideModal?: ReactNode
  items?: {name: string, quantity: number}[]
}

const ShopCategoryTile = ({
  icon,
  label,
  items
  componentInsideModal,
}: ShopCategoryTileProps ) => {
  const [openDialog, setOpenDialog] = useState(false)
  const [hover, setHover] = useState(false)
  const disabled = !items || items.length  === 0
  return (
    <>
      <Tooltip title="Not Available" show={disabled}>
        <StyledButton
          className={disabled ? "grey" : isBooked ? "green" : "red" }
          disabled={disabled}
          onClick={() => disabled ? null : setOpenDialog(true) }
          onMouseEnter={() => disabled ? null : setHover(true)}
          onMouseLeave={() => disabled ? null : setHover(false)}
        >
          {icon}
          <StyledLabel>{label}<StyledLabel/>
          {!disabled && isBooked && <FaCheckCircle/>}
          {!disabled && hover && <WavingHand />}
        </StyledButton>
      </Tooltip>
      {componentInsideModal &&
        <Dialog open={openDialog} onClose={() => setOpenDialog(false)}>
          {componentInsideModal}
        </Dialog>
      }
    </>
  )
}
```

</details>
          
<details>
    <summary>✅ 더 "나은" 솔루션</summary>

```tsx
// split into two smaller components!

const DisabledShopCategoryTile = ({ icon, label }: { icon: ReactNode, label: string }) => {
  return (
    <Tooltip title="Not available">
      <StyledButton disabled={true} className="grey">
        {icon}
        <StyledLabel>{label}<StyledLabel/>
      </Button>
    </Tooltip>
  )
}

type ShopCategoryTileProps = {
  icon: ReactNode
  label: string
  isBooked: boolean
  componentInsideModal: ReactNode
}

const ShopCategoryTile = ({
  icon,
  label,
  isBooked,
  componentInsideModal,
}: ShopCategoryTileProps ) => {
  const [openDialog, setOpenDialog] = useState(false)
  const [hover, setHover] = useState(false)

  return (
    <>
      <StyledButton
        disabled={false}
        className={isBooked ? "green" : "red"}
        onClick={() => setOpenDialog(true) }
        onMouseEnter={() => setHover(true)}
        onMouseLeave={() => setHover(false)}
      >
        {icon}
        <StyledLabel>{label}<StyledLabel/>
        {isBooked && <FaCheckCircle/>}
        {hover && <WavingHand />}
      </StyledButton>
      {openDialog &&
        <Dialog onClose={() => setOpenDialog(false)}>
          {componentInsideModal}
        </Dialog>
      }}
    </>
  )
}
```

</details>

참고: 위 예시는 실제 프로덕션에서 본 컴포넌트를 단순화한 버전이다.

<details>
    <summary>❌ 좋지 않은 솔루션</summary>

```tsx
const ShopCategoryTile = ({
  item,
  offers,
}: {
  item: ItemMap;
  offers?: Offer;
}) => {
  const dispatch = useDispatch();
  const location = useLocation();
  const history = useHistory();
  const { items } = useContext(OrderingFormContext)
  const [openDialog, setOpenDialog] = useState(false)
  const [hover, setHover] = useState(false)
  const isBooked =
    !item.disabled && !!items?.some((a: Item) => a.itemGroup === item.group)
  const isDisabled = item.disabled || !offers
  const RenderComponent = item.component

  useEffect(() => {
    if (openDialog && !location.pathname.includes("item")) {
      setOpenDialog(false)
    }
  }, [location.pathname]);
  const handleClose = useCallback(() => {
    setOpenDialog(false)
    history.goBack()
  }, [])

  return (
    <GridStyled
      xs={6}
      sm={3}
      md={2}
      item
      booked={isBooked}
      disabled={isDisabled}
    >
      <Tooltip
        title="Not available"
        placement="top"
        disableFocusListener={!isDisabled}
        disableHoverListener={!isDisabled}
        disableTouchListener={!isDisabled}
      >
        <PaperStyled
          disabled={isDisabled}
          elevation={isDisabled ? 0 : hover ? 6 : 2}
        >
          <Wrapper
            onClick={() => {
              if (isDisabled) {
                return;
              }
              dispatch(push(ORDER__PATH));
              setOpenDialog(true);
            }}
            disabled={isDisabled}
            onMouseEnter={() => !isDisabled && setHover(true)}
            onMouseLeave={() => !isDisabled && setHover(false)}
          >
            {item.icon}
            <Typography variant="button">{item.label}</Typography>
            <CheckIconWrapper>
              {isBooked && <FaCheckCircle size="26" />}
            </CheckIconWrapper>
          </Wrapper>
        </PaperStyled>
      </Tooltip>
      <Dialog fullScreen open={openDialog} onClose={handleClose}>
        {RenderComponent && (
          <RenderComponent item={item} offer={offers} onClose={handleClose} />
        )}
      </Dialog>
    </GridStyled>
  )
}
```

</details>

### 💖 2.4 중복은 잘못된 추상화보다 훨씬 저렴하다

너무 이른/부적절한 일반화를 지양하자. 간단한 기능 구현을 위해 큰 오버헤드가 필요하다면, 다른 옵션을 고려하자. 
다음 글을 강력히 추천한다: [Sandi Metz: The Wrong Abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction).

> 복잡함의 한 유형은 오버 엔지니어링(over-engineering)이다. 개발자들이 코드를 필요 이상으로 일반화(generic)하거나 현재 시스템에서 필요하지 않은 기능을 추가한 경우다. 개발자들이 미래에 해결해야할지도 모르는 문제가 아니라, 현재 해결해야 하는 문제를 해결하도록 권장하자. 미래의 문제는 실제로 당면해 문제의 형태와 요구사항을 구체적으로 볼 수 있을 때 해결하면 된다. ― [Google Engineering Practices: What to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

더보기: [KCD: AHA Programming](https://kentcdodds.com/blog/aha-programming), [C2 Wiki: Contrived Interfaces](https://wiki.c2.com/?ContrivedInterfaces)/[The Expensive Setup Smell](https://wiki.c2.com/?ExpensiveSetUpSmell)/[Premature Generalization](https://wiki.c2.com/?PrematureGeneralization)
        
## 🧘 3. 성능 팁

> "너무 이른 최적화는 만악의 근원이다." ― Tony Hoare
        
> "한 번의 정확한 측정이 천 개의 전문가 의견보다 낫다." ― Grace Hopper

**TL;DR**

1. **느리다고 생각한다면, 벤치마크(benchmark)로 증명하자.** _"불확실한 상황에서 추측하려는 유혹을 견뎌라."_ [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) (Chrome extension) 의 프로파일러가 당신의 친구다!
2. `useMemo` 는 주로 비싼 계산을 위해서만 사용하자.
3. 리렌더링을 줄이기 위해 `React.memo`, `useMemo`, `useCallback` 를 사용할 거라면 의존성이 많지 않아야 하며, 대부분 원시 값이어야 한다.
4. `React.memo`, `useMemo`, `useCallback` 가 당신이 의도했던 대로 동작하는지 확인하자. (정말 리렌더링을 방지하고 있는가? 사용한다면 의미 있는 성능 향상이 이뤄질 것을 실증적으로 입증할 수 있는가? [메모이제이션이 때로는 앱 성능을 더 악화시킬 수 있으니](https://kentcdodds.com/blog/usememo-and-usecallback) 주의하자!)
5. 눈을 깜빡일 때마다 스스로 때리는 짓은 멈추자. ([fix slow renders before fixing rerenders](https://kentcdodds.com/blog/fix-the-slow-render-before-you-fix-the-re-render)) <br>
_(역: 누군가 눈을 깜빡일 때마다 스스로를 때리라고 한다면 어떻게 할 것인가? 때리지(slow render) 않기 위해 눈을 덜 깜빡이려(less re-render) 노력하지 말고, 때리는 걸 멈추고 마음껏 깜빡이자. 즉, 리렌더링을 줄이는 것보다 느린 렌더링을 없애는 데 집중하자는 내용이다)_
6. 상태를 사용하는 곳에 최대한 가깝게 두면 코드를 읽기 훨씬 쉬워질 뿐 아니라 앱이 더 빨라진다. (state colocation) _(역: [React 최적화하기](https://wizvee.netlify.app/react-js-app-faster) 읽어보기)_
7. `Context` 는 논리적으로 나눠야 하며, 하나의 context provider 에 너무 많은 값을 넣지 말아야 한다. 컨텍스트의 어떤 한 값이라도 바뀐다면, 실제론 바뀐 값을 사용하지 않더라도 해당 컨텍스트를 소비하는 모든 컴포넌트가 리렌더링 되기 때문이다.
8. `state` 와 `dispatch` 함수를 분리해 `context` 를 최적화할 수 있다.
9. [`lazy loading`](https://nextjs.org/docs/advanced-features/dynamic-import) 과 [`bundle/code splitting`](https://reactjs.org/docs/code-splitting.html) 을 이해하자.
10. 거대한 리스트는 윈도잉(window) 하자. ([`tannerlinsley/react-virtual`](https://github.com/tannerlinsley/react-virtual) 나 비슷한 도구를 사용하자)
11. 대개 번들 크기가 작을수록 앱이 빨라진다. 생성한 코드 번들을 [`source-map-explorer`](https://create-react-app.dev/docs/analyzing-the-bundle-size/) 또는 [`@next/bundle-analyzer`](https://www.npmjs.com/package/@next/bundle-analyzer) (NextJS 사용시) 같은 도구를 사용해 시각화할 수 있다.
12. 폼(form) 패키지를 사용하려면 개인적으로 [`react-hook-forms`](https://react-hook-form.com/) 를 추천한다. 좋은 성능과 좋은 개발자 경험의 균형을 잘 맞췄다고 생각한다.

<details>
    <summary><strong>성능에 대해 읽어보면 좋을 KCD 글들</strong></summary>


- [KCD: State Colocation will make your React app faster](https://kentcdodds.com/blog/state-colocation-will-make-your-react-app-faster)
- [KCD: When to `useMemo` and `useCallback`](https://kentcdodds.com/blog/usememo-and-usecallback)
- [KCD: Fix the slow render before you fix the re-render](https://kentcdodds.com/blog/fix-the-slow-render-before-you-fix-the-re-render)
- [KCD: Profile a React App for Performance](https://kentcdodds.com/blog/profile-a-react-app-for-performance)
- [KCD: How to optimize your context value](https://kentcdodds.com/blog/how-to-optimize-your-context-value)
- [KCD: How to use React Context effectively](https://kentcdodds.com/blog/how-to-use-react-context-effectively)
- [KCD: One React Mistake that is slowing you down](https://epicreact.dev/one-react-mistake-thats-slowing-you-down)
- [KCD: One simple trick to optimize React re-renders](https://kentcdodds.com/blog/optimize-react-re-renders)

</details>

## 🧘 4. 테스팅 원칙

> "테스트를 작성해라. 너무 많이는 말고. 대부분 통합(integration)으로." ― Guillermo Rauch

**TL;DR**

1. 테스트는 항상 소프트웨어가 실제 사용되는 모습을 닮아야 한다.
2. 구현적인 세부 사항을 테스트하지 않도록 유의하자 - 사용자는 사용하지도, 보지도, 알지도 못할 것들이다
3. 아무것도 망가뜨리지 않았다는 확신이 테스트를 해도 들지 않는다면, 테스트의 (단 하나뿐인) 소임을 다하지 못한 것이다.
4. 코드를 리팩토링한 후 동일한 사용자 동작에 대해 테스트를 거의 변경할 필요가 없다면 올바른 테스트를 구현한 것이다.
5. 프론트엔드는 100% 테스트 커버리지를 달성할 필요는 없다. 70% 정도면 충분할 것이다. 테스트는 생산성을 높여야지, 작업을 늦춰선 안 된다. 테스트 유지 관리는 작업 속도를 늦출 수 있다. 특정 시점 이후엔 테스트를 추가하여 얻는 이점이 급속도로 줄어들 것이다.
6. 개인적으로 [Jest](https://jestjs.io/), [React testing library](https://testing-library.com/docs/react-testing-library/intro/), [Cypress](https://www.cypress.io/), 그리고 [Mock service worker](https://github.com/mswjs/msw) 를 좋아한다.

<details>
    <summary><strong>테스트에 대해 읽어보면 좋을 KCD 글들</strong></summary>

- [KCD: Testing Implementation Details](https://kentcdodds.com/blog/testing-implementation-details)
- [KCD: Stop mocking fetch](https://kentcdodds.com/blog/stop-mocking-fetch)
- [KCD: Common mistakes with React Testing Library](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)
- [KCD: Making your UI tests resilient to change](https://kentcdodds.com/blog/making-your-ui-tests-resilient-to-change)
- [KCD: Write fewer, longer tests](https://kentcdodds.com/blog/write-fewer-longer-tests)
- [KCD: Write tests. Not too many. Mostly integration.](https://kentcdodds.com/blog/write-tests)
- [KCD: How to know what to test](https://kentcdodds.com/blog/how-to-know-what-to-test)
- [KCD: Common Testing Mistakes](https://kentcdodds.com/blog/common-testing-mistakes)
- [KCD: Why you've been bad about testing](https://kentcdodds.com/blog/why-youve-been-bad-about-testing)
- [KCD: Demystifying Testing](https://kentcdodds.com/blog/demystifying-testing)
- [KCD: UI Testing Myths](https://kentcdodds.com/blog/ui-testing-myths)
- [KCD: Effective Snapshot Testing](https://kentcdodds.com/blog/effective-snapshot-testing)

</details>

## 🧘 5. 다른 사람들이 공유한 인사이트

If you'd like to share some of the things you think about when you write React code that I didn't touch upon, you can submit a PR and add them to this section. Thanks for taking the time to share your ideas!

### [Josh W Comeau](https://www.joshwcomeau.com/) 

> 비슷하지만 개인적으로 더 강력하다고 느끼는 원칙은 지나치게 강력한 권한을 가진 _setters_ 를 전달하지 않는 것이다. 예를 들어, `setUser` 함수 대신 `updateEmail` 함수를 전달하는 것이 올바르다고 생각한다. 컴포넌트가 변경하면 안 되는 것을 _변경하는_ 걸 방지하기 위해서다.

> 컴포넌트는 단일 책임을 져야 할 뿐만 아니라, 추상화 스펙트럼에서 자기 위치가 어디인지 확실히 해야 한다. 한쪽 끝엔 `<Button>`, `<Heading>`, `<Modal>` 같은 일반적인 구성 요소가 있고, 다른 쪽 끝에는 `<Homepage>` 나 `<ContactForm>` 같은 일회성 템플릿이 있다. 모든 컴포넌트는 이 스펙트럼에서 명확한 위치에 있어야 한다. `<Button>` 컴포넌트는 _user_ prop 을 가지면 안 된다. 사용자는 더 높은 추상화 개념이고, `Button` 은 낮은 추상화 컴포넌트이기 때문이다.

(Note, the above is taken from his response to my email... I really appreciate that he took the time to share his ideas 🙂)
