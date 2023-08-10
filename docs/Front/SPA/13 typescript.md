---
title: 02_3 TypeScript(React)
layout: default
parent: SPA
grand_parent: Front
---

# TypeScript(React)

{: .highlight } 
> - App.tsx 타입 지정

<br />

```jsx
const App:React.FC = () => {
  return (
    <div className="App">
    </div>
  )
}
```


<br />
<br />
<br />

---

{: .highlight } 
> - `props_type` 전달하기

<br />

> model/resturant.ts 타입 정의하기

```ts
// 타입 지정하기
type Restaurant = {
  name : string;
  category : string;
  address: Address;
  menu: menu;
}

type Address = {
    city: string;
    detail: string;
    zipCode: Number;
}

type menu = {
    name:string;
    price:number;
    category:string;
}
```

> 데이터 타입 사용하기 prop를 이용해서 전달하기

```js
let data : Restaurant = {
  name: "식당",
  category: "western",
  address: {
    city : "seoul",
    detail : "somewhere",
    zipCode: 2345215
  },
  menu : [
    {name:"rose pasta",price:2000,category:"pasta"},
    {name:"rose pasta",price:2000,category:"pasta"}
    ]
}

const App:React.FC = () => {
  return (
    <div info={data} className="App">
      <Store info={data}/>
    </div>
  )
}
```

> props 전달 받기

```js
// props를 받기 위해 interface 지정
interface OwnProps {
  info:Restaurant
}
//FC에서 props 받는 방법
const Store:React.FC<OwnProps> = (props) => {
  return (
  )
}
```




<br />
<br />
<br />

---

{: .highlight } 
> - `useState` 제네릭 문법 사용하기
>   - useState를 호출할 당시의 타입 지정 `<>`

<br />

```js
const [data,setData] = useState<Restaurant>(data)
//Restaurant 타입만 들어올 수 있음
setData()
```


<br />
<br />
<br />

---

{: .highlight } 
> - 함수를 prop로 전달하는 방법
>   - setState 함수를 전달하는 방법

<br />

> App.js 함수를 전달하는 쪽

```js
const App :React.FC = () => {
  const [data , setDate] = useState<Restaurant>(data);
  const changeAddress = (address:Address) => {
    setDate({...data,address:address});
  }
  return (
    <div>
      <Store info={data} changeAddress={changeAddress}/>
    </div>

  )
}
```

> Store.jsx 함수를 받는 쪽

```js
// props를 받기 위해 interface 지정
interface OwnProps {
  info:Restaurant,
  changeAddress(address:Address):void
}
//FC에서 props 받는 방법
const Store:React.FC<OwnProps> = ({info}) => {
  return ()
}
```


<br />
<br />
<br />

---

{: .highlight } 
> - `type omit 기능`
>   - 기존 타입에서 하나의 타입을 빼는 것

<br />

```js
export type Address = {
  city : string;
  detail : string;
  zipcode : Number;
}
// Address에서 zipcode만 빼기
// type AddressWithoutZip = {
//     city : string;
//     detail : string; 
//   }
type AddressWithoutZip = Omit<Address,'zipCode'>
```




<br />
<br />
<br />

---

{: .highlight } 
> - `pick` 하나의 타입만 가져오는 것
>   - 기존의 타입에서 하나의 타입만 가져오는 것

```ts
type Restaurant = {
  name: string;
  category: number;
}

//  category: number; 하나만 가져오게 됨
type RestaurantOnlyCategory = Pick<Restaurant,'category'>
```







<br />
<br />
<br />

---

{: .highlight } 
> - extends 예제
>   - `타입 재활용 기능`

<br />


```js
//menu타입에서 price를 뺀 것
interface OwnProps extends Omit<menu,'price'>{

}
//FC에서 props 받는 방법
const Store:React.FC<OwnProps> = ({info}) => {
  return ()
}
```


<br />
<br />
<br />

---

{: .highlight } 
> - api에서의 타입스크립트 활용
>   - 응답값 처리하기

<br />

> 타입 선언하기

```ts
// 응답데이터에는 totalPage , page 항상 있다.
// data 타입은 항상 바뀌는 값이니 T로 유동적으로 바뀔 수 있게 해줌
export type ApiResponse<T> {
  data: T[],
  totalPage:number,
  page:number
}

// 사용하기 1. 레스토랑 api 호출
type ResturantResponse = ApiResponse<Restaurant>
// 사용하기 2. 메뉴 api 호출
type MenuResponse = ApiResponse<Menu>
```