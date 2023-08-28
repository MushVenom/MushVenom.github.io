---
title: 020.Spring Entity
layout: default
parent: JAVA
grand_parent: Back
---

# Spring Entity

```sql
create sequence post with increment by 50

create sequence user start with 1 increment by 50

create table post (id integer not null, description varchar(255), user_id integer, primary key(id))

create table user (id integer not null, birth_date date, name varchar(255), primary key (id))
```

<br />

## spring Entity

{: .highlight } 
> - post.java
>   - 다대일 관계 (`@ManyToOne`) : 기본값은 연결된 entity도 같이 반환하는 것
>   - fetch 속성은 지연 로딩 or 즉시 로딩
>       - `FetchType.LAZY` (지연 로딩): Order 엔터티를 불러오는데 Customer 엔터티는 불러오지 않고, Order 엔터티의 Customer 정보에 접근할 때 Customer 엔터티를 불러옵니다. 이는 성능 최적화에 도움이 됩니다.
>       - `FetchType.EAGER` (즉시 로딩): Order 엔터티와 Customer 엔터티가 Many-To-One 관계일 때, Order 엔터티를 불러올 때 Customer 엔터티도 함께 불러옵니다. 이는 성능 이슈를 일으킬 수 있습니다.

<br />

```java
@Entity
public class Post {
    @Id
    @GeneratedValue
    private Integer id;
    private String description;

    @ManyToOne(fetch = FetchType.LAZY)
    @JsonIgnore
    private User user;
}
```

<br />
<br />


{: .highlight } 
> - User.java
>   - 사용자는 post를 여러개 쓸 수 있으니 일대다 관계이다.(`@OneToMany`)
>   - 테이블과 연결 시킬 때 사용하므로 `@JsonIgnore`

```java
@Getter@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class UserDTO {
    private Integer id;
    @Size(min=2,message = "이름은 2자 이상이여야합니다.")
    private String name;
    private LocalDate birthDate;

    // 사용자는 post를 여러개 쓸 수 있으니 일대 다 관계이다.
    @OneToMany(mappedBy = "user")
    @JsonIgnore
    private List<Post> posts;
}
```

<br />
<br />


---

{: .highlight } 
> - `controller.java`
>   - Optional을 사용하면, 반환 값이 null일 경우에 대한 추가적인 검사 없이 코드를 더 깔끔하게 작성할 수 있다.
>   - Optional
>       - `resent()` : 메소드를 사용하여 값이 있는지 확인할 수 있다
>       - `get()` : 메소드를 사용하여 값을 가져올 수 있다.

<br />

```java
@RestController
@RequestMapping("v1")
public class ResController {
    
    private UserRepository repository;
    //의존성 주입
    public ResController(UserRepository repository){
        this.repository = repository;
    }

    @GetMapping("jpa/users/{id}/posts")
    public List<Post> PostForUser(@PathVariable int id){
        Optional<UserDTO> userdto = repository.findById(id);

        //Optional클래스는 get메소드를 가지고 있음
        // 
        return userdto.get().getPosts();
    }
}
```