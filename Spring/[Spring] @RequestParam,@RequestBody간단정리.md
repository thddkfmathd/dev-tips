
#### @RequestParam vs @RequestBody

| 특징            | @RequestParam                          | @RequestBody                     |
|---------------|---------------------------------|---------------------------------|
| **데이터** | 쿼리 스트링, 폼 데이터 (`?key=value`) | 요청 바디 (`JSON`, `XML`) |
| **HTTP 메서드** | `GET`, `POST`, `PUT` (쿼리 파라미터) | `POST`, `PUT` (Body 데이터) |
| **Content-Type** | `application/x-www-form-urlencoded` (기본) | `application/json` |
| **데이터 형식** | 단순 문자열, 숫자 등 (`String`, `int`) | `JSON` 객체 (DTO, Map 등) |
| **용도** | 검색, 필터링, 단일 값 전달 | 객체(엔티티) 전체 전달 |

<br>

##### 📌 @RequestParam

###### URL의 쿼리 스트링 또는 Form 데이터/  `GET`, `POST` /단일 값 또는 여러 개의 쿼리 파라미터


```java
  @GetMapping("/user")
  public String getUser(@RequestParam String name, @RequestParam int age) {
      return "User: " + name + ", Age: " + age;
  }
```
```javascript
$.ajax({
    url: "/api/user",
    type: "GET",
    data: { name: "John", age: 25 }, 
    success: function(response) { },
    error: function(error) { }
});

// or "/api/user?name=John&age=25"
```
###### 단일값 dao -> mabatis  `@Param` 필요
```java
public int ex(@Param("name") String name) {
  return sqlSession.selectOne("ex.mapperId", name);
}
```

<br>

##### 📌 @RequestBody

###### 요청 본문(Body)에서 JSON/XML을 Java 객체로 변환 /  `POST`, `PUT` / JSON 데이터를 자동으로 객체로 매핑 
###### `application/json` 필요 / 객체 전체를 받아야 할 때 사용


```java
  @PostMapping("/user")
  public String createUser(@RequestBody User user) {
      return "Created User: " + user.getName() + ", Age: " + user.getAge();
  }
```
```javascript
$.ajax({
    url: "/api/user",
    type: "POST",
    contentType: "application/json", 
    data: JSON.stringify({ name: "John", age: 25 }), 
    success: function(response) {
        console.log("응답: ", response);
    },
    error: function(error) {
        console.error("에러 발생: ", error);
    }
});
```
