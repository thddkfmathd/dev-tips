- JsonNode란?
- JsonNode의 특징 - 불변클래스
  - JsonNode는 읽기 전용 API만 제공 
- JsonNode의 장단점
- JsonNode의 필요성
- JsonNode의 적용 환경
- 최상위 객체 JsonNode - 하위객체 : ObjectNode (객체형 mutable),  ArrayNode  (배열형 mutable) ,MissingNode , ValueNode (텍스트, 숫자, 불리언 등)
- JsonNode메서드
  - get vs path
  - get (엄격 npe 가능성)
    - return값 JsonNode 또는 없을때 null ->  null체크하고 asText() 하는로직으로 구성
    - 뽑아서 체크 :JsonNode dataLengthNode = item.get(Layout.dataLength.name());
    -  if ( dataLengthNode != null && dataLengthNode.isInt() )  ,,,, if( dataValueNode != null && dataValueNode.isTextual() )
  - path : (npe 없음)
    - return값 JsonNode (없을때 MissingNode, NPE발생 안함을 보장 )(MissingNode.asText() == "")
  - asText(null),asText("") 로 기본값
  - 
- JsonNode 불변객체 이유 / 그럼에도 원본을 변경해야한다면?
  - ObjectNode ,ArrayNode 사용 put,set?
  - get이나 path로 뽑은 JsonNode를 다운캐스팅 (ObjectNode ...) 해서 변경 -> 원본 객체 JsonNode에 변경 적용
  - 모든트리?를 JsonNode타입으로 받을수있으나 (최상위) instanseof ObjectNode 와 같이 ObjectNode로 만들어진 객체인지 타입체크이후 다운캐스팅해야함
  - ObjectNode로 강제 캐스팅할때 ArrayNode와 같은 다른 JsonNode 하위 객체로 만들어진 거면 ClassCastException 남
- JsonNode deepCopy 어디까지 deepCopy? depth가 깊다면?
  
```JAVA
asisChildren.forEach(asisChild -> {
	   ObjectNode asisChildObjectNode = (ObjectNode)asisChild
    // asisChildObjectNode 값 변경코드
    // asisChildren 에도 변경 반영 ( asisChild는 asisChildren 내부의 객체 참조값을 공유 ) 
});

```
- 위코드의 forEach 루프 내부에서는 asisChildren에 변경이 반영
- stream은?
  - Stream API는 불변성(immutability)과 side-effect-free 처리를 지향
  - 원본의 불변을 보장하고 새로운 객체를 리턴하는게 기본철학 (순수 함수형 패턴'을 지향 -> 입력만 보고 출력 결정, 같은걸넣으면 같은 값이 나와야함)
  - 수정은가능하나 권장하지않음
```JAVA
// List<JsonNode>로 변환해서 .stream() 쓰는 방법도 있음
StreamSupport.stream(asisChildren.spliterator(), false).forEach(node -> {
    if (node instanceof ObjectNode) {
        ((ObjectNode) node).put("check", "Y");
    }
});
```
- StreamSupport.stream
  -  Iterable한 자료구조에서 Stream을 만듬
  -  .iterator() 처럼 순회가능한 ArrayNode의 Spliterator제공
  -  Spliterator를 StreamSupport.stream(...) 로 감싸면 Stream Api 사용가능

- 비교
순회하면서 수정하거나 검사만 할 때
→ StreamSupport.stream(...).forEach(...)
→ 가장 가볍고 빠름.

필터링하거나 특정 조건 추출 후 다른 스트림 작업 계속해야 할 때
→ List<JsonNode> list = StreamSupport.stream(...).collect(...) 후 .stream()
→ 가독성과 편의성 ↑, 단 중간 복사 비용 존재. 스트림 API 유연하게 사용 가능


- put, set
	- put(String,JsonNode) deprecated된 이유는 더 명확한 타입 체크와 API 일관성을 위해 set()을 쓰도록 권장
 	- **명확한 구분 목적**  
  		- `put()`은 **기본 값(Value)** 을 넣는 함수로 유지됨  
  		- `set()`은 **JsonNode 구조(Node)** 를 설정하는 함수로 통일됨

	- **API 일관성 유지**  
  		- `ObjectNode.set()`  
  		- `ArrayNode.set()`  
  		→ 같은 방식으로 사용 가능하도록 개선됨
   
 메서드 | 목적 | 매개변수 타입 | 현재 상태 |
|--------|------|----------------|------------|
| `put(String, 기본형/문자열)` | 필드에 **간단한 값** (문자열, 숫자, 불린 등)을 넣을 때 사용<br>→ 내부에서 `TextNode`, `IntNode` 등으로 자동 래핑됨 | `int`, `long`, `double`, `boolean`, `String` 등 | 정상 사용 |
| `put(String, JsonNode)` | 예전 버전에서 `JsonNode` 자체를 넣을 때 사용되던 방식 | `JsonNode` | deprecated |
| `set(String, JsonNode)` | **JsonNode 객체** 자체를 필드에 설정할 때 사용 | `JsonNode` | 권장 방식 |
