---
title: 01_1 Nextjs
layout: default
parent: SSR
grand_parent: Front
---


# Nextjs

Page Layout
{: .label .label-purple }

{: .note }
> app.js 설명
>
> Component : 현재 페이지를 의미한다. 페이지 변환시에 Component가 변경된다.
>
> PageProps : 데이터 패칭 메서드를 통해 미리 가져온 초기 객체

<br />

{: .note }
`_app.js` 아래 코드는 전역적으로 header 추가하는 코드 , public에 images에 이미지를 넣으면 아래 형식 처럼 사용 가능하다

```jsx
//_app.js
export default function Myapp({Component,pageProps}){
    return( 
    <div>
        <Header />
        <Component {...pageProps} />
    </div>
    )
}

//header.js
function Header(){
    <Head>
        <title>타이틀</title>
    </Head>

    return(
        <div>
            <img src="/images/a1" alt="logo">
            <div>헤더</div>
        </div>
    )
}
```

<br />

## _document.js

{: .note }
_document.js html , head , body를 수정할 때 사용하는 파일 , `서버에서만 랜더링 됨` , `onClick같은 이벤트가 발생하지 않는다.` <br /> Document의 Head와 App의 Head는 다르다.

```jsx
import Document , {Html , Head , Main , NextScript} from "next.document"

class MyDocument extends Document {
    render(){
        <Html lang="ko">
            <Head />
            <body>
                <Main />
                <NextScript />
            </body>
        </Hyml>
    }
}

```

<br />

## Dynamic Routes

{: .new } 
page/post/[id].js에 파일을 만든다면, url `/post/1` , `/post/abc`로 들어왔을때 [id].js를 보여주게 된다.

> 사용 예제

```jsx
// 상세 페이지 구현
import {useRouter} from 'next/router'

const Post = () =>{
    const router = useRouter();
    // post/123 --> id : 123
    const { id } = router.query;

    useEffect(()=>{
        //id가 있고 id가 0보다 클때 비동기 요청
        if(id && id>0){
            gerData();
        }
    }.[id])
    return(<div>상세 페이지 구현</div>)
}
```

<br />

## Server Side Rendering 

{: .new } 
> - 정적 생성  
>   - 프로젝트가 빌드하는 시점에 html 생성
>   - `모든 요청에 html 파일 재사용`
>   - nextjs는 이정적 생성 방식을 추천
>   - `getStaticProps` / `getStaticPaths`
>
> - 서버 사이드 랜더링
> 

