---
title: 01_6 ReactContextApi
layout: default
parent: SPA
grand_parent: Front
---


# React conText 관련


![context1](https://user-images.githubusercontent.com/86187456/205320212-5bbbf377-33ad-47fd-a49a-e8f9dfa15334.png)
![context2](https://user-images.githubusercontent.com/86187456/205320228-97e1c92d-00fd-4cd3-a69d-c79c6cfcf4bb.png)
![context3](https://user-images.githubusercontent.com/86187456/205320243-5cf3827f-2fa6-45dc-8c23-dc2ac7c68fb8.png)

<br />

{: .note }
> 1.하위의 컴포넌트에서 부모의 props, 데이터를 사용하기 위함
> 2.context를 사용시 컴포넌트를 재사용하기 어려워 질 수 있다.
> 3.다양한 레벨에 있는 많은 컴포넌트에게 부모의 데이터를 전달하기 위함(`전역적인 데이터`)
> 4.`Prop driling`을 피하기 위한 목적이라면 `Component Composition를 먼저 고려 해야 한다.`

<br />

{: .important }
createContext("초기값") -> Context.provider로 묶에 주지 않으면 초기값를 다른 컴포넌트에 건네주게됨

<br />




{: .new }
Context/Contexts.tsx 생성(0)


```js
mport { createContext } from "react";

interface data  {
    name : string;
    boo(): void;
    num:() => string;
}

class datause implements data{
    //입력값 받기
    constructor(public name:string){

    }
    boo(){
        console.log("콘솔에 나타남")
    }
    num(){
        return "화면에 나타남"    
    }
}

export const dataused = new datause("kim")

export const Context = createContext<data | null>(null);
```

<br />




{: .new }
react Context 하위 컴포넌트로 전달 (1)

```jsx
import { Context,dataused } from "../Context/Contexts";
 function One() {
        return (
          <div>
            <div style={{borderBottom:"1px solid black"}} />
            <Context.Provider value={dataused}>
              <Two />
            </Context.Provider>
          </div>
        );
      }

export default One;
```

<br />


{: .new }
전달 받은 context값 사용 (2)

```js
 import { Context } from "../Context/Contexts";
//input focus 예제
 function Two() {
    const data = useContext(Context);
    console.log(data?.boo())
     return (
         <div>
            하위 컴포넌트 context값 전달
            <p>{data?.name}</p>
            <p>{data?.num()}</p>
         </div>
    );
}
export default Two;
```