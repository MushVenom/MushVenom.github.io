---
title: 01_3 useMemoization
layout: default
parent: SPA
grand_parent: Front
---

# React.memo


![REACT MEMO1](https://user-images.githubusercontent.com/86187456/205433976-82df2eeb-bd4d-414e-acdd-1cb8128b0bd6.png)
![REACT MEMO2](https://user-images.githubusercontent.com/86187456/205433979-4bf4852d-9235-4515-b351-9a78e6691c0e.png)

## React React.memo(Memoization)

`prop check를 통해 변화가 있으면 재렌더팅, 없으면 기존의 컴포넌트 사용`


{: .note }
React.memo란 react에서 제공하는 고착(HOC) 컴포넌트이다. <br />
고착 컴포넌트란, 하나의 컴포넌트를 받아서 최적화된 컴포넌트로 반환해주는 함수 <br />
React.memo는 렌더링 될때마다 Prop check를 해서 컴포넌트의 변화가 있는지 없는지 확인을 한다. <br />
무분별하게 사용하면 메모리의 공간을 너무 많이 차지하게 됨

---


## React.memo 사용할 때 (props가 변하지 않으면 불필요하게 자식을 재랜더링하지 않는다.)


{: .new } 
1.컴포넌트가 같은 prop로 자주 렌더링 될 때<br/> 2.컴포넌트가 렌더링 될 때 마다 복잡한 로직을 처리 해야 할 때

---



{: .new } 
memo함수 실행 과정</br>
1.memo라는 고착 컴포넌트는 two라는 컴포넌트를 받는다.</br> 2.다시 memo는 최적화된 two 컴포넌트 return한다.
</br>

> 사용법

```js
import React, { memo } from "react";

///최적화 할 컴포넌트를 memo로 감싸준다.
export default memo(Two);
```

<br />
<br />

{: .new } 
react.memo 예시 (1)

```jsx
import { useReducer, useState, useEffect, useMemo } from "react";
export function One() {
  const [parent, setParent] = useState<number>(0);
  const [child, setChild] = useState<number>(0);
  const parentFun = (): void => {
    setParent(parent + 1);
  };
  const childFun = (): void => {
    setChild(child + 1);
  };

  type name = {
    lastName: string;
    firstName: string;
  };
  const obj: name = useMemo(() => {
    return {
      lastName: "김",
      firstName: "김김",
    };
  }, []);

  console.log("자식 props 변화X 부모만 재 렌더링");
  return (
    <div>
      <div
        style={{ backgroundColor: "bisque", width: "300px", height: "300px" }}
      >
        <button onClick={parentFun}>부모만 재렌더링</button>
        <br />
        <button onClick={childFun}>부모, 자식 재렌더링</button>
        <Two props={child} />
        <Three {...obj} />
      </div>
    </div>
  );
}
```

<br />

---

# React Memo 관련

![memo1](https://user-images.githubusercontent.com/86187456/205335782-791303bf-ecd0-4d90-a50d-20ec187bf5d2.png)


{: .new } 
1.Memoization : 동일한 값을 리턴하는 함수를 여러번 호출할 때 맨 처음 호출한 함수의 리턴값을 메모리에 보관 후 메모리의 저장된 값을 사용하는 것 <br />
2.자주 계산한 값을 cash해서 필요할 때마다 cash에서 꺼내서 사용<br />
3.렌더링 > 함수 호출 > 렌더링 > Memoize된 값을 사용<br />
4.오래걸리는 연산을 메모리에 넣어두고 다시 연산하지 않도록 최적화하는 방법

- 배열-items- 안에 요소의 값이 업데이트 될 때만 callback함수를 호출해서 다시 Memoization를 한다
  
<br /> 
<br />


{: .note-title }
items 값이 변경될 때마다 다시 콜백함수를 계산해서 value에 삽입 
useMemo 구조

```js
const value = useMemo(
    ()=>{retrun 값1},[items]
)

//!값1 이 value값에 들어가게 되고, items의 변수가 변경될 때만 다시 콜백함수를 계산하게됨
```
<br /> 


{: .note-title }
1.useMemo의 구조 (콜백 함수 , 배열)
2.맨 처음 한번만 Memoization 함

```js
const value = useMemo(
    ()=>{},[]
)
```



<br />

{: .note-title }
React usememo 예제 (1)

```jsx
import { useMemo, useState } from "react";
import Two from "./two";

 function One() {
//Memoization 사용
//useMemo 사용 예제

//변수
const [hardNumber,setHardNumber] = useState<number>(1);

//오래걸리는 연산
const hardCalculate = (number:number) : number =>{
  for(let i=0; i<999999999; i++){} //오래걸리는 연산
  return number+10000
}

//hardNumber가 변경 되지 않았다면 hardSum의 값을 재사용한다.
//hardNumber가 변경 되면 콜백함수를 다시 계산해서 hardSum에 넣게 된다.
const hardSum = useMemo(()=>{
  return hardCalculate(hardNumber)
}
,[hardNumber])

        return (
          <div>
            <input type="number" value={hardNumber} onChange={(e)=>{setHardNumber(parseInt(e.target.value))}} />
            <span> + 10000 = {hardSum} </span>
          </div>
        );
      }

export default One;
```


<br />

{: .note-title }
React usememo 예제 (2)

```jsx
import {useEffect, useMemo, useState } from "react";

 function Two() {
   const [number, setNumver] = useState<number>(0);
   const [istrue , setIstrue] = useState<boolean>(true)

   const memo = useMemo(()=>{
      return {
         turetrue : istrue ? "바뀜" : "안바뀜"
      }
   },[istrue])

   useEffect(()=>{
      console.log("useEffect 호출")
   },[memo])
   
     return (
         <div>
            <input type="number" value={number} onChange={(e) => setNumver(parseInt(e.target.value))} />
            <p>{memo.turetrue}</p>
            <button onClick={()=>{setIstrue(!istrue)}}>버튼 </button>
         </div>
    );
}
export default Two;
```


<br />
<br />
<br />

---

# React useCallback 관련

![callback2](https://user-images.githubusercontent.com/86187456/205430567-1c28e202-57be-479d-b600-066df6a09f47.png)


{: .note }
1.Memoization 기법으로 컴포넌트 성능을 최적화하는 도구이다.</br>
2.Memoization기법이란 반복적으로 계산해야하는 값을 cash 해서 메모리에서 cash된 값을 가져오는 기법</br>
3.콜백함수 그자체를 Memoization 하는 것, 필요할 때마다 메모리에 있는 함수를 호출하는 것</br>
4.함수,객체,배열이 참조하는 주소값을 처음값으로 고정

<br />

{: .important }
> 자바스크립트객체(함수,객체,배열) 값은 메모리 주소가 참조
>
> 랜더링될 때 마다 자바스크립트 객체는 새로운 주소를 참조하게 됨

<br />
<br />

{: .important }
> 컴포넌트가 다시 랜더링이 되러다도 calculate함수가 초기화 되는 것을 막을 수 있다.
>
> 컴포넌트가 처음 랜더링이 될때만 함수 객체를 만들고, 다시 랜더링 될때는 전에 만들어둔 함수객체를 재사용하는 기법

```js
function Component(){
    const calculate = useCallback((num)=>{
        return ~~~
    },[item]);

    return <div>컴포넌트</div>
}


```

<br />

>useCallback 구조

```js
function Component(){
    // Memoization 할 함수 선언
    const calculate = useCallback((num)=>{
        return ~~~
    // [item] 의존성 배열 , 의존성 배열의 변할 때 Memoization된 함수를 다시 할당
    },[item]);

    return <div>컴포넌트</div>
}


```


<br />
<br />


{: .note-title }
React usememo 예제 (1)

```jsx
import {useEffect, useCallback, useState } from "react";
 function Two() {
   const [size,setSize] = useState(100);
   const [isDark,setIsDark] = useState(false);
   //박스 사이즈가 변경될 때만 호출됨
   const creatBoxstyle = useCallback(()=>{
      return{
         backgroundColor:"pink",
         width:`${size}px`,
         height:`${size}`
      }
   },[size])
     return (
         <div style={{background:isDark ? "black" : "white"}}>
            <input type="number" value={size} onChange={(e)=>{setSize(parseInt(e.target.value))}}/>
            <button onClick={()=>setIsDark(!isDark)}>사이즈 변경 함수는 호출안됨</button>
            <Three creatBoxstyle={creatBoxstyle}></Three>
         </div>
    );
}
export default Two;
```

<br />


{: .note-title }
React usememo 예제 (2)

```jsx
import { useCallback, useState,useEffect } from "react";
 //button눌렀을때 컴포넌트가 재렌더링 되지만 , changeFun , useCallback 함수는  재선언 되지 않고 유지 된다.
 //inputbox를 변경했을때만 useCallback 험수를 재선언해서 재선언한 주소값을 객체에 저장한다.

 export default function One() {
  const [number,serNumber] = useState<number>(0);
  const [istrue,setIstrue] = useState<boolean>(true);

  const changeFun = useCallback(()=>{
    console.log(`${number}`)
    return;
  },[number])

  useEffect(()=>{
    console.log("useCallback 재할당 됨")
  },[changeFun])

        return (
          <div>
              <input type="number" value={number} onChange={(e)=> serNumber(parseInt(e.target.value))} />
              <button onClick={()=> setIstrue(!istrue)}>{istrue.toString()}</button>
              <br />
              <button onClick={changeFun}>useCallback 변경 안됨</button>
          </div>
        );
      }
```