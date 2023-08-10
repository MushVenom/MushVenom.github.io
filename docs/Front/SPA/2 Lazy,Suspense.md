---
title: 01_4 Lazy,Suspense
layout: default
parent: SPA
grand_parent: Front
---


## React Suspense 사용

![sus1](https://user-images.githubusercontent.com/86187456/205493252-1dc0b54d-5704-44a6-871d-b8c6a62162bb.png)

{: .note }
> 비동기 작업을 하는 컴포넌트를 자식으로 가짐 , 자식 컴포넌트가 비동기 작업을 진행하는 동안, 콜백에 할당 받은 컴포넌트를 렌더링함
> 
> 자식 컴포넌트의 비동기 작업이 끝나면 리렌더링을 해서 자식 컴포넌트를 보여줌
> 
> Start fetching -> Start rendering -> Finish fetching
> 
> suspense를 활용하면 해당 컴포넌트를 사용하는 측에서 로딩 상태를 표현하는 방식을 정의하고 ,UI 일관성을 유지할 수 있음

- React Query , Recoil , Relay --> suspense 사용하기 더 편함


<br />

> 사용 예제

```js
import { Suspense } from "react";
      <nav>
        <button onClick={() => startTransition(() => setPage(0))}>
          첫 번째 페이지
        </button>
        <button onClick={() => startTransition(() => setPage(1))}>
          두 번째 페이지
        </button>
        <button onClick={() => startTransition(() => setPage(2))}>
          세 번째 페이지
        </button>
      </nav>
      //fallback에 비동기 작업이 끝나기 전까지 렌더링 될 컴포넌트 넣어둠
      <Suspense fallback={<div>로딩 중</div>}>
        {pages[page]}
      </Suspense>
```


<br />

---

![suspense1](https://user-images.githubusercontent.com/86187456/205493279-c83d4a43-62ce-4942-b7cc-af3a4a94ddc9.png)
![suspense2](https://user-images.githubusercontent.com/86187456/205493280-1f5885a7-a991-4f7f-9742-d2611474465a.png)



<br />

# React.lazy

{: .note }
> React.lazy로 불러온 컴포넌트는 단독으로 쓰일 수 없고, React.Suspense 컴포넌트로 하위에서 렌더링되어야 한다.
>React 공식 문서에 따르면 Router 바로 아래에 Suspense를 위치시키고, Route로 보여줄 컴포넌트들을 React.lazy로 불러올 것을 권장함

<br />

> Route , Suspense 예시 (1)

```js
const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => {
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
  }
```

> suspense와 react.lazy의 다른 예시 (2)

```js
import React, { lazy, Suspense } from 'react';

const AvatarComponent = lazy(() => import('./AvatarComponent'));

const renderLoader = () => <p>Loading</p>;
{/* 컴포넌트를 불러올동안 다른 페이지를 임시적으로 보여줌*/}
const DetailsComponent = () => (
  <Suspense fallback={renderLoader()}>
    <AvatarComponent />
  </Suspense>
)

```


> suspense 예시 (3)

```js
const AvatarComponent = lazy(() => import('./AvatarComponent'));
const InfoComponent = lazy(() => import('./InfoComponent'));
const MoreInfoComponent = lazy(() => import('./MoreInfoComponent'));

const renderLoader = () => <p>Loading</p>;

const DetailsComponent = () => (
  <Suspense fallback={renderLoader()}>
    <AvatarComponent />
    <InfoComponent />
    <MoreInfoComponent />
  </Suspense>
)
```

<br />
<br />

---

## React React.lazy 사용


![lazy1](https://user-images.githubusercontent.com/86187456/205501317-6c578758-0986-4d6d-9b08-a5fe5be8b7b2.png)
![lazy2](https://user-images.githubusercontent.com/86187456/205501321-7279e56b-5120-49cb-92c8-6db322a19152.png)

{: .important }
> pages의 이미지 업로드 예제 코드 참고 바람
>
> `React.lazy로 불러온 컴포넌트는 단독으로 쓰일 수 없고`, `React.Suspense 컴포넌트로 하위`에서 렌더링되어야 한다.
>
> lazy 컴포넌트는 반드시 `Suspense 컴포넌트 하위`에서 렌더링되어야 하며 Suspense는 lazy 컴포넌트가 로드되길 기다리는 동안 `로딩 화면과 같은 예비 컨텐츠를 보여줄 수 있게` 해줌



{: .new }
> 사용법
>
> React 공식 문서에 따르면 Router 바로 아래에 Suspense를 위치시키고, Route로 보여줄 컴포넌트들을 React.lazy로 불러올 것을 권장함

<br />

> suspense와 react.lazy의 예시

```js
const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
```

> suspense와 react.lazy의 다른 예시

```js
import React, { lazy, Suspense } from 'react';

const AvatarComponent = lazy(() => import('./AvatarComponent'));

const renderLoader = () => <p>Loading</p>;

const DetailsComponent = () => (
  <Suspense fallback={renderLoader()}>
    <AvatarComponent />
  </Suspense>
)

```


<br />
<br />

{: .important }
> 로딩 페이지 준비 (1)

```jsx
import { Iprops } from "./three";

const LazyImage = ({ src, name }: Iprops) => {
  return <img src={src} alt={name} />;
};

export default LazyImage;
```

{: .important }
> Lazy , Suspense 사용 (2)

```jsx
export interface Iprops {
  src: string;
  name: string;
}
const LazyImage = lazy(() => import("./LazyImage"));
const Three = ({ src, name }: Iprops) => {
  return (
    <div>
      <Suspense fallback={<div>...loading</div>}>
        {" "}
        // 컴포넌트가 다 불러오기 전까지 ...loading이라는 컴포넌트를 보여줍니다.
        Suspense를 사용하지 않으면 에러가 납니다.
        <LazyImage src={src} name={name} /> // 다 불러와지면 해당 컴포넌트를
        보여줍니다.
      </Suspense>
      {name}
    </div>
  );
};

export default Three;
```