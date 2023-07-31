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

```
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

```
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

```
Dispatch(Action) ----> Reducer(State, Action)

::dispatch가 state를 action으로 변경을 요청 -> Reducer가 state를 action으로 변경
```



>사용법

```
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



