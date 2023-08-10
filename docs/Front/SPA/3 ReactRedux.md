---
title: 01_5 ReactRedux
layout: default
parent: SPA
grand_parent: Front
---


![redux3](https://user-images.githubusercontent.com/86187456/206486033-e66766e8-0f0f-4c2e-9b7c-06b146be292a.png)
![redux](https://user-images.githubusercontent.com/86187456/206486044-5c825cda-93f9-44cb-a27a-ba5e729b494e.png)
![redux2](https://user-images.githubusercontent.com/86187456/206486051-07ecdfd2-42bc-4f8d-8443-d7f4f8377797.png)




## redux-설명 (action , Reducer , store , dispatch)

- > Props drilling를 피하기 위한 redux!
- > Component에서 Action Creator를 통해 Action을 생성하고 그 Action을 Dispath함수로 실행시켜준다. 그러면 Store에서 해당 Reducer로 매칭되는 Action이 있는지 확인하고 Store에 저장된 상태를 변경시켜준다.
- > `action` : 특정 기능을 수행,호출 하기 위한 데이터를 표현한 객체
- > `Reducer` : store의 상태의 변화를 주기 위한 함수이다.
- > `store` : 리덕스에서 상태(state)를 저장하는 공간, 한 어플리케이션 당 하나의store를 가지고, store안에는 state와 reducer가 내장되어 있다.
- > `dispatch` : action를 발생시키는 것, dispatch함수에 action 객체를 전달시켜 reduecer에게 상태 변경을 요청하는 역할
- > `Subscribe` : `store` 내장 함수 중 하나 로 `reducer`가 호출 될 때 Subscribe된 함수 및 객체를 호출 한다.
- > 컴포넌트간 State 공유가 편리함(props 대체)

<br />
<br />

---

# Redux의 2가지 규칙
{: .note }
`store`는 단 `하나만을 생성`하는 것을 권장한다. (다양한 reducer를 만들어 다향한 상태를 구분해서 관리)<br />
`상태는 읽기 전용`임 (기존의 상태는 건드리지 않고 `새로운 state를 생성해 상태를 변경` 시켜야함, redux- Toolkit은 immer를 지원하긴 함)

<br />

## Redux 동작

{: .important }
> ui 렌더링 시 UI 컴포넌트는 `STORE`에서 상태를 접근해서 STATE를 받아온다.
> 
> UI에서 STATE 변경 시 `dispatch`함수를 이용해서 `action`를 실행시킨다. `reducer`에게 state 변경 요청
>
> `action`을 받은 `store`는 `reducer`를 실행 하고 `reducer`의 return 값을 state에 저장한다.
> 
> `Subscribe` 된 UI는 STATE 업데이트로 변경된 데이터를 새롭게 랜더링 한다.

<br />

---

<br />

- > `configureStore()`는 별도의 메서드 없이 바로 미들 웨어 추가가 가능 (Redux - ToolTIX) ,<br /> 리덕스 설정을 간편하게 할 수 있도록 도와줌 (redux-thunk 제공)
- > `createSlice`는 `action` , `reducer`를 따로 생성하지 않아도 되도록 도와줌<br />선언한 slice의 name에 따라서 액션 생성자, 액션 타입, 리듀서를 자동으로 생성해줌


{: .important }
> store 생성하기(1)

```js
//store 생성 (state 생성 , state 등록 , state 변경함수 export)
import { configureStore, createSlice } from "@reduxjs/toolkit";

//전역 관리할 STATE 생성 , state 변경 함수 생성
let state = createSlice({
  name: "state이름",
  //저장할 STATE 값(initialState)
  initialState: { name: "kim", age: 20 },
  //state변경 함수 등록(reducers)
  reducers: {
    //파라미터는 기존 state를 뜻함 return값 => state
    //redux는 기본적으로 immer를 지원함
    //파라미터를 사용 시 .payload를 붙여야함
    statechange(state, 첫번째파라미터, 두번째파라미터) {
      return (state.age += 첫번째파라미터.payload);
    },
  },
});

// 위에서 정의한 state에 등록
export default configureStore({
  reducer: {
    아무이름: state.reducer,
  },
});
//state 변경 함수 export  (actions : reducers의 함수들 return)
export let { statechange } = state.actions;
```

## 위 코드 간단 설명

- `initialState를 통해 state의 처음 상태를 정`
- `reducers에서 액션 정의 (state 변경 함수 정의)`
- `export 로 다른 컴포넌트에서 state변경 함수 사용할 수 있도록 함`

<br />
<br />

{: .important }
> redux 사용할 컴포넌트 지정 (index.tsx에 Provider 묶어주기) (2)

```js
<Provider store={store}>
  <React.StrictMode>
    <App />
  </React.StrictMode>
</Provider>
```

{: .important }
> redux state 사용하기 (3) , state 변경(useDispatch)

```js
import { useSelector, useDispatch } from "react-redux/es/exports";
import statechange from "./store.js";
//redux가 관리하는 모든 state값이 return
let a = useSelector((state) => {
  return state;
});

console.log(a.아무이름);

//useDispatch : reducer에게 state변경을 요청함
//useDispatch로 묶어준 후 state 변경
let dispatch = useDispatch();
<button
  onClick={() => {
    useDispatch(statechange);
  }}
></button>;
```

## 위 코드 간단 설명

`useSelector()`로 스토어에서 현재 항태를 값을 가져온다.
`useDispatch()`로 통해 변경되는 값을 스토어에 전달한다.

