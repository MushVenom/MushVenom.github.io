---
title: 01_1 useEffect
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