---
title: 01_1 useEffect,useReducer
layout: default
parent: SPA
grand_parent: Front
---

# React useEffect 

{: .note }
클래스형 컴포넌트에서는 생명주기 메소드를 사용할 수 있었는데, 이를 함수형 컴포넌트에서도 사용할 수 있다.
componentDidMount , componentDidUpdate , componentWillUnmount 라이프 사이클 대체가 가능

## React useEffect 사용

- 컴포넌트가 마운트 되었을 때
- 컴포넌트가 업데이트 되었을 때
- 마운트 해제(화면에서 사라질 때) 되었을 때

  <br/>
![effect1](https://user-images.githubusercontent.com/86187456/205477412-f496d198-d56a-4668-986d-8b52fa7817b3.png)

  <br/>
  <br/>

{: .important-title } 
> useEffect의 CLeanUp 작업 
> 마운트 시 아벤트 리스너를 통해 event를 추가했다면 언마운트 시 event를 삭제해야한다. (event 삭제 안할 시 메모리 누수 발생 가능) 
> componentWillUnmount 기능 == return 함수 

<br />

```js
  useEffect(() => {
    //컴포넌트가 맨처음 렌더링 됐을 때만 실행되는 콜백 (의존성배열이 [])
    const timer = setInterval(() => {
      console.log("타이머 ㅅㅈ");
    }, 1000);

    //컴포넌트가 언마운트 될때 화면에서 사라질때 실행되는 콜백 (의존성배열 [])
    return () => {
      clearInterval(timer);
      console.log("타이머 ㅈㄹ");
    };
  }, []);

```

</br>

> 사용법

```js
useEffect (()=>{
  //렌더링 될때 마다 실행될 콜백함수
})


useEffect (()=>{
  //마운트 될때만 첫번째 랜더링에만 실행되는 콜백 함수
},[])


useEffect(()=>{
  //의존성 배열이 변경될 때마다 실행될 콜백 함수
},[의존성 배열])
```


<br />
<br />


# useState 비동기 동작

- 리액트로 개발을 할 때 useState 훅을 이용하면 컴포넌트에 상탯값을 쉽게 관리 할 수 있다.
- useState 훅이 반환하는 배열의 두 번째 요소는 `상태값 변경 함수`다.

```js
const [state, setState] = useState(0);
```

- 리액트는 상탯값 변경 함수가 호출되면 해당 컴포넌트를 리렌더링한다. 아래는 리액트 컴포넌트가 리렌더링되는 조건이다.

```
1. state가 변경되었을 때
2. props가 변경되었을 때
3. 부모 컴포넌트가 렌더링 될 때
4. forceUpdate()를 실행할 때
```

- 리렌더링 과정에서 자식 컴포넌트도 같이 렌더링된다. 그래서 리액트는 렌더링 최적화를 위해 상탯값 변경을 비동기, 배치로 처리한다.

<br />

### 배치란?

- 배칭은 React가 더 나은 성능을 위해 여러 개의 state 업데이트를 하나의 리렌더링 (re-render)로 묶는 것을 의미한다.
- 아래와 예시를 보자.

```jsx
const App = () => {
  const [number, setNumber] = useState(0);

  const onClick = (e) => {
    setNumber(number + 1); // 아직 업데이트 X
    setNumber(number + 1); // 아직 업데이트 X
  };

  // 리액트는 onClick 함수가 완전히 종료되야 리렌더링 함

  console.log("render");
  return (
    <>
      <p>{number}</p>
      <button onClick={onClick}>더하기</button>
    </>
  );
};
```

- 위 코드를 보면 onClick 함수를 호출하면 number 상탯값을 두 번 증가시키려고 했다. 하지만 우리의 의도와 달리 number의 값은 1만 증가한다.

  - 이는 상탯값 변경 함수가 비동기, 배치로 동작하기 때문이다. 리액트는 효율적으로 렌더링하기 위해 여러 개의 상탯값 변경 요청을 배치로 묶어서 처리한다.
  - 따라서 onClick 함수가 호출되어도 console.log("render")도 1번만 출력된다.

- 리액트가 상탯값 변경 함수를 동기로 처리하면 하나의 상탯값 변경 함수가 호출될 때마다 화면을 다시 그리기 때문에 성능 이슈가 생길 수 있다.
  - 상탯값 변경 함수가 동기로 동작하지만 매번 화면을 다시 그리지 않는다면, UI데이터와 화면 간의 불일치가 발생해서 혼란스러울 수 있다.

<br />

### 해결책

- 이러한 상태 변화가 1번만 일어나는 것을 방지하고, 비동기로 동작하지만 상탯값 변경 함수가 호출 될 때마다 해당 상태를 변경하고 싶으면 상탯값 변경 함수로 `콜백 함수`를 넣는 방식이 있다.

```jsx
const App = () => {
  const [number, setNumber] = useState(0);

  const onClick = (e) => {
    setNumber((prev) => prev + 1);
    setNumber((prev) => prev + 1);
  };

  console.log("render");
  return (
    <>
      <p>{number}</p>
      <button onClick={onClick}>더하기</button>
    </>
  );
};
```

- 위와 같이 작성하면 배치 처리되서 1회만 리렌더링 되는 것은 맞는데, 기존과 다르게 onClick이 1회 호출하면, 상태값은 1만 증가하는 것이 아닌 2가 증가한다.
- 이유는 상탯값 변경 함수로 입력된 함수는 `자신이 호출되기 직전의 상탯값을 매개변수`로 받는다. 이 코드에서는 첫 번째 호출에서 변경된 상탯값이 두 번째 호출의 콜백함수 prev 인수로 사용된다. 따라서 이렇게 작성하면 number는 2가 증가한다.

<br />

### 근본적으로 왜 1만 증가하는 걸까?

```js
function useState(initialState) {
  var dispatcher = resolveDispatcher();
  return dispatcher.useState(initialState);
}
```

- React의 useState 함수이다. 이 함수는 `resolveDispatcher`라는 함수가 반환하는 객체의 useState라는 메서드를 실행하여 반환되는 값을 리턴한다.

```js
function resolveDispatcher() {
  var dispatcher = ReactCurrentDispatcher.current;

  {
    if (dispatcher === null) {
      error("Invalid hook call...");
    }
  }

  return dispatcher;
}
```

- resolveDispatcher 함수는 다시 `ReactCurrentDispatcher`라는 객체의 `current` 속성을 반환한다.

```js
var ReactCurrentDispatcher = {
  /**
   * @internal
   * @type {ReactComponent}
   */
  current: null,
};
```

- 이때 주목할 점은 `ReactCurrentDispatcher`가 객체라는 점이다.
- 객체이기 때문에 동일한 key 값에 대하여 이전의 값을 계속해서 덮어쓴다. 결국에는 마지막 명령어만 수행되는 셈이다. 아래처럼.

```js
const num = 1;
const obj1 = { num: 1 };
const obj2 = Object.assign(
  {},
  { num: num + 1 },
  { num: num + 1 },
  { num: num + 1 }
);

console.log(obj); // { num: 1 }
console.log(obj2); // { num: 2 }
```


<br />
<br />

---

# React uselayouteffect 

![생명구조](https://user-images.githubusercontent.com/86187456/205478873-e7620139-b401-467d-92d1-5708e05f52ab.png)


{: .note }
> 1.stateUpdate 때문에 화면 깜빡임이 발생할 때 사용하는 hook
>
> 2.네트워크 , 캐시를 통해 변수를 받아서 렌더링 할 때 약간의 버벅거림이 생김(깜빡 거림)
>
> 3.render -> Browser paint screen -> cleanup Effects (useEffect는 화면을 그린 후 실행됨)
>
> 4.reder -> useLayoutEffect -> Browser paint screen (화면을 그리기 전에 실행됨)

<br />


{: .important }
> `정리`
>
> useEffect의 이펙트는 DOM이 화면에 그려진 이후에 호출된다.
>
> `useLayoutEffect의 이펙트는 DOM이 화면에 그려지기 전에 호출된다`.
>
> 따라서 렌더링할 상태가 `이펙트 내에서 초기화되어야 할 경우`, 사용자 경험을 위해 useLayoutEffect를 활용하자!


<br />

> useLayoutEffect 사용법

```jsx
  const [name, setName] = useState("");

  useLayoutEffect(() => {
    //네트워크 통신...
    setName("김제현");
  });

    //useEffect -> 사용하면 useEffect 보다 Browser paint screen 먼저 일어나 화면 깜빡임이 생김
    //useLayoutEffect -> 사용하면 Browser paint screen 보다 useLayoutEffect 먼저 일어나 깜빡임이 사라짐
    <>
      <div>ㅎㅇㅎㅇㅎㅇ{name}</div>
      <div>ㅎㅇㅎㅇㅎㅇ{name}</div>
      <div>ㅎㅇㅎㅇㅎㅇ{name}</div>
      <div>ㅎㅇㅎㅇㅎㅇ{name}</div>
      <div>ㅎㅇㅎㅇㅎㅇ{name}</div>
    </>

```


---

<br />
<br />

---

# useReducer


{: .note }
useReducer는 State(상태)를 관리하고 업데이트하는 Hook인 useState를 대체할 수 있는 Hook이다.<br/>
useReducer는 State 업데이트 로직을 분리하여 컴포넌트의 외부에 작성하는 것을 가능하게 함으로써, 코드의 최적화를 이루게 해준다.

## React useReducer
1. 여러개의 하위 값을 포함하는 복잡한 state를 사용할 때 사용 
2. useReducer (Dispatch , Action , Reducer) 3가지로 구성됨
3. 여러상황마다 각 각 다르게 하나의 state 변경이 필요할 때 사용 (swich문 분기 처리-reducer)
3. reducer의 내용이 바뀔 때 마다 화면의 재렌더링
4. swich 문과 혼합해서 사용, 실수를 줄일 수 있음

<br />
<br />

![Reducer1](https://user-images.githubusercontent.com/86187456/205430489-85dc82ae-75d3-49bb-9b74-d69185c4ae59.png)



{: .new }
Reducer, dispatch , action

<br />

{: .highlight }
- Reducer = state를 업데이트 시켜주는 역할 , state를 변경하고자 한다면 Reducer를 이용해서 변경
- dispatch = state 업데이트를 위한 요구, Reducer에 state 변경을 요구하는 역할
- action = 요구의 내용 ,  dispatch가 Reducer에게 state 변경을 요구할 때 변경될 state값

<br />

```js
Dispatch(Action) ----> Reducer(State, Action)

::dispatch가 state를 action으로 변경을 요청 -> Reducer가 state를 action으로 변경
```



>사용법

```js
//action의 타입
 type Action = { type: 'INCREASE' } | { type: 'DECREASE' }

//state count 초기값 = 0 
const [count, dispatch] = useReducer(reducer, 0);


// dispatch를 호출 실행되는 reducer 함수
 function reducer(state: number, action: Action): number {
  switch (action.type) {
    
    //전달 되는 action의 type마다 state변경값을 다르게 
    //return값이 count state값으로 변경되고 재 렌더링됨
    case 'INCREASE':   
      return state + 1;
    
    case 'DECREASE':    
      return state - 1;

      //이상한 action이 들어왔을때 error
    default:
      throw new Error('Unhandled action');
  }
}

//dispatch 호출 후 action으로 state를 변경하는 함수
const onIncrease = () => dispatch({ type: 'INCREASE' });
const onDecrease = () => dispatch({ type: 'DECREASE' });
```



