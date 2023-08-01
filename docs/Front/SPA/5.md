---
title: 01_7 ReactCustomHook
layout: default
parent: SPA
grand_parent: Front
---


# React React.Customhook

{: .new } 
> 1.Hook을 추상화된 로직으로 사용할 수 있도록 결합해주고 다른 컴포넌트 사이에서 공통의 상태 관련 로직을 재사용할 수 있도록 함
>
> 2.`커스텀훅은 반복되는 로직을 묶어 재사용하기 위해 사용`
> 
> 3.커스텀 훅을 사용해서 만든 기능은 각 컴포넌트에서 독립된 상태를 가진다.
> 
> 4.`한 컴포넌트에 커스텀 훅을 여러번 사용해도 독립적인 state가 여러개 생성이 된다.`


<br />



> 개발을 하다 보면 가끔 상태 관련 로직을 컴포넌트 간에 재사용하고 싶은 경우가 생깁니다. 이 문제를 해결하기 위한 전통적인 방법이 두 가지 있었는데, higher-order components와 render props가 바로 그것입니다. Custom Hook은 이들 둘과는 달리 컴포넌트 트리에 새 컴포넌트를 추가하지 않고도 이것을 가능하게 해줍니다. - 리액트 공식 문서-

<br />

{: .important }
인자를 3개 받는 커스텀 훅 만들기 (home.tsx) (1)

```jsx
import { useInput } from "../Customhook/useInput";

const displayMessage = (message: string): void => {
  alert(message);
};

function home() {
    //인자를 3개 받는 커스텀 훅 선언(inputbox 값, inputbox 값 변경, alert창 띄우기)
  const [inputValue, setinputValue, handleSubmit] = useInput(
    "하이",
    displayMessage
  );
  return (
    <div>
      <input value={inputValue} onChange={setinputValue}></input>
      <button onClick={handleSubmit}>버튼</button>
    </div>
  );
}

export default home;
```

</br>


{: .important }
커스텀 훅 제작 (useInput.tsx) (2)

```jsx
//커스텀 훅 returnType 선언
type returnType = [
  string,
  (event: ChangeEvent<HTMLInputElement>) => void,
  MouseEventHandler<HTMLButtonElement>
];

export function useInput(
  //useInput 커스텀 훅이 받는 2개의 인자
  initialForm: string,
  submitAction: (message: string) => void
): returnType {
  //커스텀 훅 내부의 useState 각 커스텀 훅 마다 다르게 관리 됨
  const [inputValue, setinputValue] = useState<string>(initialForm);

  const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
    setinputValue(event.target.value);
  };

  const handleSubmit = (): void => {
    setinputValue("");
    alert(inputValue);
  };
  return [inputValue, handleChange, handleSubmit];
}

```
