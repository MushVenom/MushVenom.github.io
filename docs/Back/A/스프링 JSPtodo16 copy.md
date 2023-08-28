---
title: 016.Spring JSP TodoList
layout: default
parent: JAVA
grand_parent: Back
---

# Spring JSP TodoList

<br />

## Spring JSP TodoList


{: .new }
> - 세션 : 여러 요청에 걸쳐 값을 유지하길 원한다면 세션을 사용해야한다.
>   - `@SesstionAttributes("")`
>   - 세션을 사용할 모든 컨트롤러에 넣어줘야한다.
>   - 세션에 값을 저장하고 그걸 사용
> - `BindingResult result를 이용해`
>   - if(result.hasErrors()) "todo"


<br />

> 세션으로 특정 변수 유지하기 (다른 요청에서 왔던 요청을 유지 가능)


```java
@Controller
@SessionAttributes("name")
public class Todo1Controller {

    @RequestMapping("todo1post")
    public String TodoLoginPost(@RequestParam String name, @RequestParam String password, ModelMap model){
    model.put("name",name);

        return "todo1";
    }
}
```

<br />
<br />
<br />

---


# Todo get 받기 (@vaildation)

{: .new }
> - post로 데이터 받기

<br />
<br />

> Dto.java

```java
@Getter@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class Todo {
    private int id;
    private  String username;
    @Size(max=10, message = "10개 넘을 수 없음")
    private String description;
    private LocalDate targetDate;
    private boolean done;
}
```

{: .new }
> - `Service.java`

```java
@Service
public class TodoService {
    private static List<Todo> todos = new ArrayList<>();

    //고유 카운터
    private static int todosCount;

    //Todo todo 추가 메서드
    public void addTodo(String username, String description, LocalDate targetDate, boolean done){
        Todo todo = new Todo(++todosCount, username, description, targetDate, done);
        todos.add(todo);
    }
}
```



{: .new }
> - `Controller.java`
> - `@Valid`: 로 검증할 수 있음
> - `redirect`: 리다이렉션 가능

```java
@Controller
@SessionAttributes("name")
public class Todo1Controller {

    public Todo1Controller(TodoService todoService){
        super();
        this.todoService = todoService;
    }
    private TodoService todoService;


    //Todo 추가하기 페이지 리다이렉션
    @PostMapping("add-todo")
    public String add2Todo(@Valid Todo todo, ModelMap model){
        System.out.println(todo);
        todoService.addTodo(todo.getUsername(),todo.getDescription(),LocalDate.now(),false);
        return "redirect:list-todos";
        }
}
```

<br />
<br />


## Todo 하나 todo 삭제 하기

{: .new }
> - get요청을 받아 하나의 todo 삭제 하기

<br />

> controller.java

```java
@Controller
@SessionAttributes("name")
public class Todo1Controller {
    // todo 하나 삭제
    @GetMapping("delete-todo")
    public String deleteTodo(@RequestParam int id){
        todoService.deleteById(id);
        return "redirect:list-todos";
    }
}
```

{: .highlight }
> - service.java
>   - 삭제 로직 구현
>   - `Predicate` , `removeIf`를 이용해서 조건에 맞는 것만 삭제

```java
@Service
public class TodoService {

    private static List<Todo> todos = new ArrayList<>();

    //고유 카운터
    private static int todosCount;

    //Todo delete 메서드
    public void deleteById(int id){
        Predicate<? super Todo> predicate
                = todo -> todo.getId() == id;
        //predicate조건에 매칭되면 삭제된다.
        todos.removeIf(predicate);
    }
}
```


<br />
<br />
<br />

## Todo 하나 todo 삭제 하기

{: .new }
> - todo 하나 삭제 하기
> - `Predicate` 이용

<br />


> service.java

```java
@Service
public class TodoService {
    private static List<Todo> todos = new ArrayList<>();

    //고유 카운터
    private static int todosCount;

    //정적변수를 초기화할 때 static블록을 만들어야함
    static {
        todos.add(new Todo(++todosCount,"admin","Len", LocalDate.now().plusYears(1),false));
    }

    //Todo 하나 조회
    public Todo findById(int id){
        Predicate<? super Todo> predicate
                = todo -> todo.getId() == id;
        //predicate조건에 맞는 하나를 get
        Todo todo = todos.stream().filter(predicate).findFirst().get();
        return todo;
    }
}
```

> controller.java로 하나 요청 return

```java
@Controller
@SessionAttributes("name")
public class Todo1Controller {

    //todo 하나 조회하기
    @GetMapping("select")
    public String Select(@RequestParam int id){
        Todo todo = todoService.findById(id);
        System.out.println(todo);
        return "redirect:list-todos";
    }
}
```

<br />
<br />
<br />

## Todo 하나 업데이트 하기

{: .new }
> - `@RequestBody` : todo로 데이터를 받음
> - `BindingResult` : 에러 있는지
> - `@Vaild` : 유효성 검증


<br />

> controller.java

```java
@Controller
public class Todo1Controller {

    //Todo 하나 업데이트
    @PostMapping("update")
    public String update(@Vaild @RequestBody Todo todo, BindingResult result){
        if(result.hasErrors()) return "todo";
        //{id:5 , username:5, ...}
        todoService.updateTodo(todo);
        return "redirect:list-todos";
    }
}
```

> Service.java

```java
@Service
public class TodoService {
    private static List<Todo> todos = new ArrayList<>();

    //고유 카운터
    private static int todosCount;

    //정적변수를 초기화할 때 static블록을 만들어야함
    static {
        todos.add(new Todo(++todosCount,"admin","Len", LocalDate.now().plusYears(1),false));
    }

    //Todo delete 메서드
    public void deleteById(int id){
        Predicate<? super Todo> predicate
                = todo -> todo.getId() == id;
        //predicate조건에 매칭되면 삭제된다.
        todos.removeIf(predicate);
    }

    //update 함수
    public void updateTodo( Todo todo){
        deleteById(todo.getId());
        todos.add(todo);
    }
}


```