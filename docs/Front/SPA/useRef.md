# React REF 관련


{: .highlight }
 HTML요소에 직접 접근해서 DOM API이용후 제어할 때 사용하는 props</br>
 Ref의 값은 컨포넌트의 생명주기를 통해 값이 유지 됨 </br>
변화는 감지 해야하지만 변화가 랜더링을 발생시키면 안되는 값을 다룰때 사용하는 것

</br>
</br>

1.Ref의 변화 -> No 렌더링 -> 변수들의 값이 유지됨</br>
2.State의 변화 -> 렌더링 -> Ref의 값은 그대로 유지됨

>inputRef 객체의 current 속성을 통해 

```
import React, { useRef } from "react";

const countRef = useRef("초기값");

//ref 함수 활용
const upRef = () =>{
    countRef.current = countRef.current + 1;
}

//값
<p>{countRef.current}</p>
```


>React - Dom 새창을 열면 input에 focus
```
    const inputRef = useRef<HTMLInputElement>(null);

     useEffect(()=>{
        if(inputRef.current)
        inputRef.current.focus();
     },[])

     <input ref={inputRef} type="text" placeholder=""></input>

```


{: .new } 
javascript에서 특정 Dom을 선택하는 역할 ex) getElementById <br />
1.특정 DOM에 접근할 때 사용한다.<br />
2.외부 라ㅣ브러리 사용할때 유용하다.<br />
3.원하는 위치에 ref={} 의 형태로 작성하면 된다.<br />
4.포커스를 잡으려면 nameInput.current.focus() 형태로 작성하면 된다.


<br />
<br />

# React useDeferredValue

{: .note-title }
> My note title
>
> 낮은 우선 순위를 지정하기 위한 훅
> useTransition = 함수 실행의 우선 순위를 지정 , useDeferredValue = 값의 업데이트 우선순위 지정
> 리액트가 성능에 따라 성능적으로 여유가 있을 때 업데이트를 시켜주는 훅
> 변수값 업데이트가 느리게 진행되어도 되는 것은 useDeferredValue훅을 이용한다.
> useMemo와 사용 시 불 필요한 재 랜더링을 막으면서 하위 컴포넌트나 상태의 업데이트를 지연시킬 수 있다. 

<br />

> 사용 예제 

```js
  const [name, setName] = useState<string>("");
  //useDeferredValue 훅 선언(덜 중요한 값 deferredName 변수를 사용)
  const deferredName = useDeferredValue(name);
  //useMemo로 deferredName의 값이 변할 때 여유있게 랜더링
  const result = useMemo(() => deferredName + "의 결과", [deferredName]);
  //deferredName를 화면 끊김없이 렌더링
       {deferredName
        ? Array(1000)
            .fill(null)
            .map((v, i) => <div key={i}>{result}</div>)
      : null}
```