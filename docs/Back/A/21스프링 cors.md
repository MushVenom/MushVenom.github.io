---
title: 021.Spring cors 설정
layout: default
parent: JAVA
grand_parent: Back
---

# Spring cors 설정

{: .new }
> - cors 설정
>   - 크로스 오리진 리퀘스트에서는 이런 요청은 거부된다.
>   - http://localhost:3000 -> http://localhost:8090
> - `WebMvcConfigurer`클래스 이용
>   - `addCorsMappings`오버라이드
>       - addMapping : 모든것에 대해 활성화
>       - allowedMethods : 모든 메서드에 대해 활성화
>       - allowedOrigins : 허용할 origin

<br />
<br />

```java
@SpringBootApplication
public class PwnactaSpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(PwnactaSpringBootApplication.class, args);
    }


    //cors 매핑 추가
    @Bean
    public WebMvcConfigurer corsConfigurer(){
        return new WebMvcConfigurer() {
            //cors 설정 오버라이드
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                //특정한 패턴에 대해 크로스 오리진 리퀘스트 처리를 가능하게 한다.
                //모든 것에 대해 활성화 /**
                //어떤 종류의 메소드들이 허용가능한지"*"
                //허용된 오리진 http://localhost:3000
                registry.addMapping("/**")
                        .allowedMethods("*")
                        .allowedOrigins("http://localhost:3000");
            }
        };
    }
}
```